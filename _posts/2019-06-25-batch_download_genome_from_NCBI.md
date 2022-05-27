---
layout: post
title: "NCBI批量下载基因组序列"
date:   2020-06-25
tags: [基因组,序列,NCBI]
comments: true
author: Si Yangming
toc : true
---

```shell
Get the summary as a tabular text file.
curl -O ftp://ftp.ncbi.nlm.nih.gov/genomes/refseq/bacteria/assembly_summary.txt

# Filter for complete genomes.
awk -F "\t" '$12=="Complete Genome" && $11=="latest"{print $20}' assembly_summary.txt > ftpdirpaths

# Identify the FASTA files (.fna.) other files may also be downloaded here.
awk 'BEGIN{FS=OFS="/";filesuffix="genomic.fna.gz"}{ftpdir=$0;asm=$10;file=asm"_"filesuffix;print ftpdir,file}' ftpdirpaths > ftpfilepaths

# Download everything in parallel
mkdir -p all
cat ftpfilepaths | parallel -j 20 --verbose --progress "cd all && curl -O {}"
```

Download All The Bacterial Genomes From Ncbi

[https://www.ncbi.nlm.nih.gov/genome/browse/#!/overview/](https://www.ncbi.nlm.nih.gov/genome/browse/#!/overview/)

[https://www.biostars.org/p/61081/](https://www.biostars.org/p/61081/)

GNU Parallel

[http://www.gnu.org/software/parallel/](http://www.gnu.org/software/parallel/)

参考链接：

[https://www.biostars.org/p/275452/](https://www.biostars.org/p/275452/)

[https://www.biostars.org/p/61081/](https://www.biostars.org/p/61081/)
