# Structural Variant Calling Benchmark
This benchmark is based on the publicly available long-read sequencing data of the Ashkenazim son HG002/NA24385.   
I provide each step how to reproduce the final metrics with publicly available tools.
### Ashkenazim son HG002/NA24385 测序数据
#### 1. PacBio 测序
- PacBio_CCS_15kb
  - fasta/fastq文件  
    https://ftp-trace.ncbi.nlm.nih.gov/ReferenceSamples/giab/data/AshkenazimTrio/HG002_NA24385_son/PacBio_CCS_15kb/
  - bam文件 —> GRCh38:pbmm2  
    https://ftp-trace.ncbi.nlm.nih.gov/ReferenceSamples/giab/data/AshkenazimTrio/HG002_NA24385_son/PacBio_CCS_15kb/GRCh38_no_alt_analysis/
  - bam文件 —> hs37d5:pbmm2 
    https://ftp-trace.ncbi.nlm.nih.gov/ReferenceSamples/giab/data/AshkenazimTrio/HG002_NA24385_son/PacBio_MtSinai_NIST/Baylor_NGMLR_bam_GRCh37/
- PacBio_MtSinai_NIST
  - fasta/fastq文件  
    https://ftp-trace.ncbi.nlm.nih.gov/ReferenceSamples/giab/data/AshkenazimTrio/HG002_NA24385_son/PacBio_MtSinai_NIST/PacBio_fasta/
  - bam文件 —> GRCh37/GRCh38:minimap2  
    https://ftp-trace.ncbi.nlm.nih.gov/ReferenceSamples/giab/data/AshkenazimTrio/HG002_NA24385_son/PacBio_MtSinai_NIST/PacBio_minimap2_bam/
  - bam文件 —> GRCh37:NGMLR-v0.2.3  
    https://ftp-trace.ncbi.nlm.nih.gov/ReferenceSamples/giab/data/AshkenazimTrio/HG002_NA24385_son/PacBio_MtSinai_NIST/Baylor_NGMLR_bam_GRCh37/
#### 2. Oxford Nanopore 测序
- UCSC_Ultralong_OxfordNanopore_Promethion
  - fasta/fastq文件  
    https://ftp-trace.ncbi.nlm.nih.gov/ReferenceSamples/giab/data/AshkenazimTrio/HG002_NA24385_son/UCSC_Ultralong_OxfordNanopore_Promethion/
  - bam文件 —> GRCh37/GRCh38:minimap2   
    https://ftp-trace.ncbi.nlm.nih.gov/ReferenceSamples/giab/data/AshkenazimTrio/HG002_NA24385_son/UCSC_Ultralong_OxfordNanopore_Promethion/
