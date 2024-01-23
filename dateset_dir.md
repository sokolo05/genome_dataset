# 一、ref 参考基因组
### 1. hg19.fa
   ```/ifs1/laicx/01.reference/GRCh37-hg19/hg19.fa```
### 2. human_hs37d5.fasta
   ```/ifs1/laicx/01.reference/GRCh37-hg19/human_hs37d5/human_hs37d5.fasta```
### 3. hg38.fa
  ```/ifs1/laicx/01.reference/GRCh38-hg38/hg38.fa```
# 二、fasta/fastq文件
## 1.PacBio测序
### 1.PacBio_CCS_15kb（HG002）
  ```
  /ifs1/laicx/02.human-dataset/HG002_NA24385_son/PacBio_CCS_15kb/HG002/fastq_file
  ---------------------------------------------------------------------
  m54238_180901_011437.Q20.fastq
  m54238_180902_013549.Q20.fastq
            ------
  m54335_180926_225328.Q20.fastq
  m54335_180927_231344.Q20.fastq
  ---------------------------------------------------------------------
  合计：167GB 39个
  ```
### 2. HG00[1-7]_PacBio_MtSinai_NIST
  ```
  -----------HG001.fastq.gz------------
  HG001_NA12878
  HG002_NA24385_son
  ---------------------------------------------------------------------
   
  HG003_NA24149_father
  HG004_NA24143_mother
  HG005_NA24631_son
  HG006_NA24694_father
  HG007_NA24695_mother
  -----------HG007.fastq.gz------------
  ```
## 2.Oxford-Nanopore测序
### 1. HG002_ONT
  ```
  /ifs1/laicx/02.human-dataset/HG002_NA24385_son/UCSC_Ultralong_OxfordNanopore_Promethion
  ----------------------------------------------------------------------------------------
  GM24385_1.fastq    50 GB
  GM24385_2.fastq    134 GB
  GM24385_3.fastq    112 GB
  ```
# 三、bam文件
## 1.PacBio测序
### 1.PacBio_CCS_15kb（pbmm2-hs37d5）
  ```
  /ifs1/laicx/02.human-dataset/HG002_NA24385_son/PacBio_CCS_15kb/HG002/bam_file/HG002.Sequel.15kb.pbmm2.hs37d5.whatshap.haplotag.RTG.10x.trio.bam
  -----------------------------------------------------------------------------------------------------------------------------------------------
  合计：62.6 GB
  ```
### 2.PacBio_MtSinai_NIST（minimap2/NGMLR?）
  ```
  /ifs1/laicx/02.human-dataset/HG002_NA24385_son/PacBio_MtSinai_NIST/bam_file/HG002_PacBio_GRCh37.bam
  ------------------------------------------------------------------------------------------------------------------------------------------------
  合计：93.8 GB
  ```
## 2.Oxford-Nanopore测序
### 1. HG002_GRCh37_ONT（minimap2）
  ```
  /ifs1/laicx/02.human-dataset/HG002_NA24385_son/UCSC_Ultralong_OxfordNanopore_Promethion/bam_file/HG002_GRCh37_ONT-UL_UCSC_20200508.phased.bam
  -----------------------------------------------------------------------------------------------------------------------------------------------
  合计：176 GB
  ```
### 2. HG002_GRCh37_ONT（minimap2-自行比对）
  ```
  /ifs1/laicx/02.human-dataset/HG002_NA24385_son/UCSC_Ultralong_OxfordNanopore_Promethion/
  -----------------------------------------------------------------------------------------------------------------------------------------------
  GM24385_all.bam    280 GB
  GM24385_1.sam.bam  49 GB
  GM24385_2.sam.bam  126 GB
  GM24385_3.sam.bam  106 GB
  ```
