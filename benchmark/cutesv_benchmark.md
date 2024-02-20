## cutesv_benchmark

参考链接:https://github.com/tjiangHIT/sv-benchmark

## 软件版本
```
conda create --name sv-benchmark3.6 python=3.6
source activate sv-benchmark3.6
conda install minimap2==2.17 pbsv==2.2.0 svim==1.2.0 sniffles==1.0.11 cuteSV==1.0.3 truvari==1.2 samtools bgzip tabix
```

## 获取数据
1. Create directory structure
   ```
   cd /home/laicx/study/01.benchmark/03.benchmark/01.HG002_GRCh37_ONT-2
   mkdir -p ref alns tools/{pbsv,sniffles,svim,cuteSV} giab
   ```
2. Download genome in a bottle annotations
   ```
   FTPDIR=ftp://ftp-trace.ncbi.nlm.nih.gov/giab/ftp/data/AshkenazimTrio/analysis/
   curl -s ${FTPDIR}/NIST_SVs_Integration_v0.6/HG002_SVs_Tier1_v0.6.bed > giab/HG002_SVs_Tier1_v0.6.bed
   curl -s ${FTPDIR}/NIST_SVs_Integration_v0.6/HG002_SVs_Tier1_v0.6.vcf.gz > giab/HG002_SVs_Tier1_v0.6.vcf.gz
   ```
3. Download hg19 reference with decoys and map non-ACGT characters to N
   ```
   curl -s ftp://ftp-trace.ncbi.nih.gov/1000genomes/ftp/technical/reference/phase2_reference_assembly_sequence/hs37d5.fa.gz > ref/human_hs37d5.fasta.gz
   gunzip ref/human_hs37d5.fasta.gz
   sed -i '/^[^>]/ y/BDEFHIJKLMNOPQRSUVWXYZbdefhijklmnopqrsuvwxyz/NNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNN/' ref/human_hs37d5.fasta
   ```
4. Download hg19 tandem repeat annotations
   ```
   curl -s https://raw.githubusercontent.com/PacificBiosciences/pbsv/master/annotations/human_hs37d5.trf.bed > ref/human_hs37d5.trf.bed
   ```

## Alignment
```
cd /home/laicx/study/01.benchmark/03.benchmark/01.HG002_GRCh37_ONT-2/alns
# /home/laicx/study/01.benchmark/03.benchmark/01.cutesv_benchmark/alns
```

## Run pbsv
1. Add MD tag to BAM
   ```
   samtools calmd -bS alns/HG002_GRCh37_ONT-UL_UCSC_20200508.phased.bam ref/human_hs37d5.fasta > alns/HG002_GRCh37_ONT-UL_UCSC_20200508.phased.bam
   ```
2. Discover SV signatures for each alignment, can be done in parallel
   ```
   pbsv discover alns/HG002_GRCh37_ONT-UL_UCSC_20200508.phased.bam tools/pbsv/hg2_ont.svsig.gz --tandem-repeats ref/human_hs37d5.trf.bed
   ```
3. Call and polish SVs
   ```
   pbsv call ref/human_hs37d5.fasta tools/pbsv/hg2_ont.svsig.gz tools/pbsv/hg2_ont.pbsv.vcf -t INS,DEL
   bgzip tools/pbsv/hg2_ont.pbsv.vcf
   tabix tools/pbsv/hg2_ont.pbsv.vcf.gz
   ```
## Run svim
1. Run svim
   ```
   svim alignment tools/svim alns/HG002_GRCh37_ONT-UL_UCSC_20200508.phased.bam ref/human_hs37d5.fasta --min_sv_size 30
   ```
2. Prepare for truvari
   ```
   cat tools/svim/final_results.vcf \
        | sed 's/INS:NOVEL/INS/g' \
        | sed 's/DUP:INT/INS/g' \
        | sed 's/DUP:TANDEM/INS/g' \
        | awk '{ if($1 ~ /^#/) { print $0 } else { if($5=="<DEL>" || $5=="<INS>") { print $0 }}}' \
        | grep -v 'SUPPORT=1;\|SUPPORT=2;\|SUPPORT=3;\|SUPPORT=4;\|SUPPORT=5;\|SUPPORT=6;\|SUPPORT=7;\|SUPPORT=8;\|SUPPORT=9;' \
        | sed 's/q5/PASS/g' > tools/svim/hg2_ont.svim.vcf
   bgzip tools/svim/hg2_ont.svim.vcf
   tabix tools/svim/hg2_ont.svim.vcf.gz
   ```
## Run sniffles
 1. Run sniffles
    ```
    sniffles -s 10 -l 30 -m alns/HG002_GRCh37_ONT-UL_UCSC_20200508.phased.bam -v tools/sniffles/hg2_ont.sniffles.vcf --genotype
    ```
 2. Prepare for truvari
    ```
    cat <(cat tools/sniffles/hg2_ont.sniffles.vcf | grep "^#") \
        <(cat tools/sniffles/hg2_ont.sniffles.vcf | grep -vE "^#" | \
          grep 'DUP\|INS\|DEL' | sed 's/DUP/INS/g' | sort -k1,1 -k2,2g) \
        | bgzip -c > tools/sniffles/hg2_ont.sniffles.vcf.gz
    tabix tools/sniffles/hg2_ont.sniffles.vcf.gz
    ```
## Run cuteSV
 1. Run cuteSV
    ```
    cuteSV alns/HG002_GRCh37_ONT-UL_UCSC_20200508.phased.bam tools/cuteSV/hg2_ont.cuteSV.vcf ./ -s 10 -l 30
    ```
 2. Prepare for truvari
    ```
    grep -v 'INV\|DUP\|BND' tools/cuteSV/hg2_ont.cuteSV.vcf | bgzip -c > tools/cuteSV/hg2_ont.cuteSV.vcf.gz
    tabix tools/cuteSV/hg2_ont.cuteSV.vcf.gz
    ```
## Final comparison
  1. Compare to ground truth
     ```
     truvari -f ref/human_hs37d5.fasta -b giab/HG002_SVs_Tier1_v0.6.vcf.gz\
              --includebed giab/HG002_SVs_Tier1_v0.6.bed -o bench-pbsv --passonly\
              --giabreport -r 1000 -p 0.00 -c tools/pbsv/hg2.pbsv.vcf.gz
     truvari -f ref/human_hs37d5.fasta -b giab/HG002_SVs_Tier1_v0.6.vcf.gz\
              --includebed giab/HG002_SVs_Tier1_v0.6.bed -o bench-svim --passonly\
              --giabreport -r 1000 -p 0.00 -c tools/svim/hg2.svim.vcf.gz
     truvari -f ref/human_hs37d5.fasta -b giab/HG002_SVs_Tier1_v0.6.vcf.gz\
              --includebed giab/HG002_SVs_Tier1_v0.6.bed -o bench-sniffles --passonly\
              --giabreport -r 1000 -p 0.00 -c tools/sniffles/hg2.sniffles.vcf.gz
     truvari -f ref/human_hs37d5.fasta -b giab/HG002_SVs_Tier1_v0.6.vcf.gz\
              --includebed giab/HG002_SVs_Tier1_v0.6.bed -o bench-cuteSV --passonly\
              --giabreport -r 1000 -p 0.00 -c tools/cuteSV/hg2_ont.cuteSV.vcf.gz
     ```
  2. Parse results
     ```
     function sumsv() { cat $1 | grep ':' | tr -d ',' |sed "s/^[ \t]*//"| tr -d '"' |\
               tr -d ' ' | tr ':' '\t' | awk '{ for (i=1; i<=NF; i++)  {
                 a[NR,i] = $i } } NF>p { p = NF } END { for(j=1; j<=p; j++)
                 { str=a[1,j]; for(i=2; i<=NR; i++){ str=str" "a[i,j]; } print str } }' |\
               tail -n 1 | awk '{ printf "%1.4f\t%1.4f\t%1.4f\t%10.0f\t%10.0f\t%10.0f\n", $2,$4,$11,$1,$8,$1+$8 }';}
     cat <(echo -e "Run\tF1\tPrecision\tRecall\tFP\tFN\tFP+FN")\
        <(cat <(for i in bench*; do printf $i"\t";sumsv $i/summary.txt;done) |\
        sed 's/bench//g;s/-//g' | sort -k 2 -n -r) | column -t
     ```  
      Or for markdown  
      ```
      cat <(echo -e "Run\tF1\tPrecision\tRecall\tFP\tFN\tFP+FN")\
          <(echo -e ":-:\t:-:\t:-:\t:-:\t:-:\t:-:\t:-:")\
          <(cat <(for i in bench*; do printf $i"\t";sumsv $i/summary.txt;done) |\
          sed 's/bench//g;s/-//g' | sort -k 2 -n -r) | tr '\t' ' ' | tr -s ' ' | tr ' ' '|' | awk '{print "|"$0"|"}'
      ```
## Coverage titrations
1. Create a new base folder for each coverage and subsample the merged BAM
 ```
 samtools view -bS -s 0.106 alns/GM24385_all.bam > alns/GM24385_all_5x.bam
 samtools view -bS -s 0.213 alns/GM24385_all.bam > alns/GM24385_all_10x.bam
 samtools view -bS -s 0.426 alns/GM24385_all.bam > alns/GM24385_all_20x.bam
 ```
