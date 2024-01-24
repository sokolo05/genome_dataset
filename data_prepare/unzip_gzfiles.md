## 路径地址
```
/home/laicx/study/01.benchmark/01.dataset/02.human-dataset/HG002_NA24385_son/PacBio_MtSinai_NIST/HG002
```

### 1. 新建 unzipfile.sh 文件
```
vim unzipfile.sh
```
### 2. 键入代码
```
  #!/bin/bash
  # file_add="/home/laicx/study/01.benchmark/01.dataset/02.human-dataset/HG002_NA24385_son/PacBio_MtSinai_NIST/HG002"
  # # 切换到 当前 文件夹
  # cd "$file_add"
  cd /home/laicx/study/01.benchmark/01.dataset/02.human-dataset/HG002_NA24385_son/PacBio_MtSinai_NIST/HG002
  # 遍历 当前 文件夹下所有文件
  for file_name in *
  do
      # 如果文件以 m 开头且以 .fastq.gz 或 .fastq.gz 结束
      if [[ "$file_name" == m* && "$file_name" == *.gz ]]; then
          # 解压为 fastq 格式
          gunzip "$file_name"
          # gzip -dfv $file_name
          echo "unzip over" + $file_name
      else
          # 跳过其他格式的文件
          continue
      fi
  done
```
