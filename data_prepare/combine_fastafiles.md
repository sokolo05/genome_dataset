## 路径地址
```
/home/laicx/study/01.benchmark/01.dataset/02.human-dataset/HG002_NA24385_son/PacBio_MtSinai_NIST/HG002
```

### 1. 新建 HG002.combine.sh 文件
```
vim HG002.combine.sh
```
### 2. 键入代码
```
#!/bin/bash

set -x

ls -l | grep -n  'm1' > temp.log

awk '{print $9}' temp.log > temp_awk.log

touch HG002.MtSinai_NIST.fasta

cat temp_awk.log | while read filename

do
	#echo "Processing file: $filename"

	cat $filename >> HG002.MtSinai_NIST.fasta
	rm $filename	
done

rm temp.log temp_awk.log
```
