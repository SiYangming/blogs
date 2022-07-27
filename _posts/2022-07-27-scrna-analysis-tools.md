---
layout: post
title: "单细胞测序分析工具"
date:   2022-07-27
tags: [blog]
comments: true
author: Si Yangming
toc : true

---

## 2022-scRNAseq数据可用的分析工具综述

![An external file that holds a picture, illustration, etc. Object name is fimmu-13-902017-g002.jpg](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC9231461/bin/fimmu-13-902017-g002.jpg)

| 分析类型<br>Analysis Type                    | 程序<br/>Program                                             | 数据集<br/>Datasets                                          |
| :------------------------------------------- | :----------------------------------------------------------- | :----------------------------------------------------------- |
| 细胞类型识别<br/>**Cell Identity**           | [SingleR](https://bioconductor.org/packages/release/bioc/html/SingleR.html) ([100](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC9231461/#B100)) <br>[dynamicTreeCut ](https://horvath.genetics.ucla.edu/html/CoexpressionNetwork/BranchCutting/)([101](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC9231461/#B101))<br>[scHCL](https://github.com/ggjlab/scHCL) ([102](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC9231461/#B102)) <br>CIBERSORT (bulk RNA seq) ([103](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC9231461/#B103)) | [HCL](https://db.cngb.org/HCL/) ([102](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC9231461/#B102)) |
| 基因腹肌分析<br>**Gene Ontology**            | [EnrichR](https://maayanlab.cloud/Enrichr/) ([104](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC9231461/#B104), [105](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC9231461/#B105))<br>[Metascape](https://metascape.org/gp/index.html#/main/step1) ([107](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC9231461/#B107))<br>[clusterProfiler](https://github.com/YuLab-SMU/clusterProfiler) ([108](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC9231461/#B108))<br>[GSVA](http://www.bioconductor.org/packages/release/bioc/html/GSVA.html) ([109](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC9231461/#B109))<br>[WebGestalt](http://www.webgestalt.org/) ([110](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC9231461/#B110)) | [MSigDB](http://www.gsea-msigdb.org/gsea/msigdb/index.jsp) ([106](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC9231461/#B106))<br>Hallmark ([111](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC9231461/#B111))<br>REACTOME ([112](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC9231461/#B112)) <br>KEGG<br>[HAdb](http://autophagy.lu/clustering/index.htm) ([38](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC9231461/#B38)) |
| 细胞周期状态<br/>**Cell State**              | [SCENIC](https://scenic.aertslab.org/)/ ([113](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC9231461/#B113))<br>[CellCycleScoring](https://satijalab.org/seurat/reference/cellcyclescoring) ([97](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC9231461/#B97)) |                                                              |
| 细胞内互作<br/>**Intercellular Interaction** | [FANTOM5](https://fantom.gsc.riken.jp/) ([114](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC9231461/#B114))<br>[STRING](https://string-db.org/cgi/input?sessionId=bRDMvZAIx0Dn&input_page_show_search=on) ([115](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC9231461/#B115))<br>[CellPhoneDB](https://www.cellphonedb.org/) ([116](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC9231461/#B116), [117](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC9231461/#B117))<br>[CellChat](http://cole-trapnell-lab.github.io/monocle-release/monocle3/) ([118](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC9231461/#B118)) |                                                              |
| 发育轨迹分析<br/>**Trajectory**              | [Monocle2/3](http://cole-trapnell-lab.github.io/monocle-release/monocle3/) ([119](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC9231461/#B119))<br>[Slingshot](https://bioconductor.org/packages/release/bioc/html/slingshot.html) ([120](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC9231461/#B120))<br>[SCORPIUS](https://github.com/rcannood/SCORPIUS) ([121](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC9231461/#B121))<br>[Markov HC](https://github.com/ZhenyiWangTHU/MarkovHC) ([122](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC9231461/#B122))<br>[RNA velocity](http://velocyto.org/) ([123](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC9231461/#B123)) |                                                              |
| **染色体变异**<br/>**Chromosomal Variation** | [inferCNV](https://github.com/broadinstitute/inferCNV) ([44](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC9231461/#B44), [124](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC9231461/#B124))<br>[LIAYSON](https://cran.r-project.org/web/packages/liayson/) ([55](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC9231461/#B55))<br>[VarTrix](https://github.com/10xgenomics/vartrix) ([23](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC9231461/#B23))<br>[GISTIC2](https://github.com/broadinstitute/gistic2) ([125](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC9231461/#B125))<br>[Mutect2](https://software.broadinstitute.org/cancer/cga/mutect) ([126](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC9231461/#B126))<br>[CopyKAT](https://github.com/navinlabcode/copykat) ([127](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC9231461/#B127)) |                                                              |
| **生存分析**<br>（**Survival**）             | [Survminer](https://cran.rproject.org/web/packages/survminer/index.html) ([29](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC9231461/#B29))<br>[KaplanMeier Plotter](http://kmplot.com/analysis/index.php?p=background) ([128](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC9231461/#B128)) |                                                              |
| 癌症基因表达<br>**Cancer Gene Expression**   | CIPHER ([129](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC9231461/#B129))<br>[CancerSEA](http://biocc.hrbmu.edu.cn/CancerSEA/) ([130](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC9231461/#B130))<br>[GEPIA2](http://gepia2.cancer-pku.cn/) ([131](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC9231461/#B131))<br>[UCSC Xena](https://xenabrowser.net/) ([132](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC9231461/#B132)) | [TCGA STAD](https://portal.gdc.cancer.gov/projects/TCGA-STAD) ([133](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC9231461/#B133))<br>[Firebrowse](http://firebrowse.org/) ([134](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC9231461/#B134)) <br>PanCanAtlas |

SCANPY是一个可用于scRNAseq数据预处理、聚类和差异基因表达分析的软件。 ([99](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC9231461/#B99))

单细胞分析参考文献：Hoft, S. G., Pherson, M. D., & DiPaolo, R. J. (2022). [Discovering Immune-Mediated Mechanisms of Gastric Carcinogenesis Through Single-Cell RNA Sequencing](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC9231461/). *Frontiers in immunology*, *13*, 902017. https://doi.org/10.3389/fimmu.2022.902017

## 2019-单细胞RNA序列技术和数据分析

scRNA-seq数据的各种分析概述

![img](https://www.frontiersin.org/files/Articles/441123/fgene-10-00317-HTML/image_m/fgene-10-00317-g001.jpg)

### 表1: scRNAseq测序技术

| Methods              | Transcript coverage | UMI possibility | Strand specific | References                                                   |
| :------------------- | :------------------ | :-------------- | :-------------- | :----------------------------------------------------------- |
| Tang method          | Nearly full-length  | No              | No              | [Tang et al., 2009](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC6460256/#B111) |
| Quartz-Seq           | Full-length         | No              | No              | [Sasagawa et al., 2013](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC6460256/#B98) |
| SUPeR-seq            | Full-length         | No              | No              | [Fan X. et al., 2015](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC6460256/#B29) |
| Smart-seq            | Full-length         | No              | No              | [Ramskold et al., 2012](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC6460256/#B90) |
| Smart-seq2           | Full-length         | No              | No              | [Picelli et al., 2013](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC6460256/#B86) |
| MATQ-seq             | Full-length         | Yes             | Yes             | [Sheng et al., 2017](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC6460256/#B102) |
| STRT-seq and STRT/C1 | 5′-only             | Yes             | Yes             | [Islam et al., 2011](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC6460256/#B52), [2012](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC6460256/#B53) |
| CEL-seq              | 3′-only             | Yes             | Yes             | [Hashimshony et al., 2012](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC6460256/#B45) |
| CEL-seq2             | 3′-only             | Yes             | Yes             | [Hashimshony et al., 2016](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC6460256/#B44) |
| MARS-seq             | 3′-only             | Yes             | Yes             | [Jaitin et al., 2014](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC6460256/#B55) |
| CytoSeq              | 3′-only             | Yes             | Yes             | [Fan H.C. et al., 2015](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC6460256/#B28) |
| Drop-seq             | 3′-only             | Yes             | Yes             | [Macosko et al., 2015](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC6460256/#B77) |
| InDrop               | 3′-only             | Yes             | Yes             | [Klein et al., 2015](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC6460256/#B64) |
| Chromium             | 3′-only             | Yes             | Yes             | [Zheng et al., 2017](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC6460256/#B125) |
| SPLiT-seq            | 3′-only             | Yes             | Yes             | [Rosenberg et al., 2018](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC6460256/#B95) |
| sci-RNA-seq          | 3′-only             | Yes             | Yes             | [Cao et al., 2017](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC6460256/#B13) |
| Seq-Well             | 3′-only             | Yes             | Yes             | [Gierahn et al., 2017](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC6460256/#B34) |
| DroNC-seq            | 3′-only             | Yes             | Yes             | [Habib et al., 2017](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC6460256/#B40) |
| Quartz-Seq2          | 3′-only             | Yes             | Yes             | [Sasagawa et al., 2018](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC6460256/#B97) |

### 表2: 用于scRNA-seq数据比对和表达量计算的工具

| 工具      | 分类                      | URL                                             | 参考文献                                                     |
| :-------- | :------------------------ | :---------------------------------------------- | :----------------------------------------------------------- |
| TopHat2   | Read mapping              | https://ccb.jhu.edu/software/tophat/index.shtml | [Kim et al., 2013](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC6460256/#B62) |
| STAR      | Read mapping              | https://github.com/alexdobin/STAR               | [Dobin and Gingeras, 2015](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC6460256/#B25) |
| HISAT2    | Read mapping              | https://ccb.jhu.edu/software/hisat2/index.shtml | [Kim et al., 2015](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC6460256/#B61) |
| Cufflinks | Expression quantification | https://github.com/cole-trapnell-lab/cufflinks  | [Trapnell et al., 2010](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC6460256/#B113) |
| RSEM      | Expression quantification | https://github.com/deweylab/RSEM                | [Li and Dewey, 2011](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC6460256/#B70) |
| StringTie | Expression quantification | https://github.com/gpertea/stringtie            | [Pertea et al., 2015](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC6460256/#B84) |

### 表3：scRNA-seq数据的亚种群识别方法

| Methods   | URL                                        | References                                                   |
| :-------- | :----------------------------------------- | :----------------------------------------------------------- |
| SC3       | http://bioconductor.org/packages/SC3       | [Kiselev et al., 2017](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC6460256/#B63) |
| ZIFA      | https://github.com/epierson9/ZIFA          | [Pierson and Yau, 2015](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC6460256/#B87) |
| Destiny   | https://github.com/theislab/destiny        | [Angerer et al., 2016](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC6460256/#B5) |
| SNN-Cliq  | http://bioinfo.uncc.edu/SNNCliq/           | [Xu and Su, 2015](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC6460256/#B122) |
| RaceID    | https://github.com/dgrun/RaceID            | [Grun et al., 2015](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC6460256/#B39) |
| SCUBA     | https://github.com/gcyuan/SCUBA            | [Marco et al., 2014](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC6460256/#B78) |
| BackSPIN  | https://github.com/linnarsson-lab/BackSPIN | [Zeisel et al., 2015](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC6460256/#B123) |
| PAGODA    | http://hms-dbmi.github.io/scde/            | [Fan et al., 2016](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC6460256/#B30) |
| CIDR      | https://github.com/VCCRI/CIDR              | [Lin et al., 2017](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC6460256/#B73) |
| pcaReduce | https://github.com/JustinaZ/pcaReduce      | [Zurauskiene and Yau, 2016](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC6460256/#B127) |
| Seurat    | https://github.com/satijalab/seurat        | [Satija et al., 2015](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC6460256/#B99) |
| TSCAN     | https://github.com/zji90/TSCAN             | [Ji and Ji, 2016](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC6460256/#B56) |

### 表4:差异分析工具

| Methods  | Category    | URL                                                          | Referenes                                                    |
| :------- | :---------- | :----------------------------------------------------------- | :----------------------------------------------------------- |
| ROTS     | Single cell | https://bioconductor.org/packages/release/bioc/html/ROTS.html | [Seyednasrollah et al., 2016](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC6460256/#B101) |
| MAST     | Single cell | https://github.com/RGLab/MAST                                | [Finak et al., 2015](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC6460256/#B31) |
| BCseq    | Single cell | https://bioconductor.org/packages/devel/bioc/html/bcSeq.html | [Chen and Zheng, 2018](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC6460256/#B20) |
| SCDE     | Single cell | http://hms-dbmi.github.io/scde/                              | [Kharchenko et al., 2014](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC6460256/#B60) |
| DEsingle | Single cell | https://bioconductor.org/packages/DEsingle                   | [Miao et al., 2018](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC6460256/#B82) |
| Cencus   | Single cell | http://cole-trapnell-lab.github.io/monocle-release/          | [Qiu et al., 2017](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC6460256/#B88) |
| D3E      | Single cell | https://github.com/hemberg-lab/D3E                           | [Delmans and Hemberg, 2016](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC6460256/#B22) |
| BPSC     | Single cell | https://github.com/nghiavtr/BPSC                             | [Vu et al., 2016](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC6460256/#B119) |
| DESeq2   | Bulk        | https://bioconductor.org/packages/release/bioc/html/DESeq2.html | [Love et al., 2014](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC6460256/#B74) |
| edgeR    | Bulk        | https://bioconductor.org/packages/release/bioc/html/edgeR.html | [Robinson et al., 2010](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC6460256/#B93) |
| Limma    | Bulk        | http://bioconductor.org/packages/release/bioc/html/limma.html | [Ritchie et al., 2015](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC6460256/#B92) |
| Ballgown | Bulk        | http://www.bioconductor.org/packages/release/bioc/html/ballgown.html | [Frazee et al., 2015](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC6460256/#B32) |

### 表5：单细胞发育轨迹推断

| Tools      | Dimensionality reduction               | URL                                                          | 参考文献                                                     |
| :--------- | :------------------------------------- | :----------------------------------------------------------- | :----------------------------------------------------------- |
| Monocle    | ICA                                    | http://cole-trapnell-lab.github.io/monocle-release/          | [Trapnell et al., 2014](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC6460256/#B112) |
| Waterfall  | PCA                                    | https://www.cell.com/cms/10.1016/j.stem.2015.07.013/attachment/3e966901-034f-418a-a439-996c50292a11/mmc9.zip | [Shin et al., 2015](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC6460256/#B103) |
| Wishbone   | Diffusion maps                         | https://github.com/ManuSetty/wishbone                        | [Setty et al., 2016](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC6460256/#B100) |
| GrandPrix  | Gaussian Process Latent Variable Model | https://github.com/ManchesterBioinference/GrandPrix          | [Ahmed et al., 2019](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC6460256/#B1) |
| SCUBA      | t-SNE                                  | https://github.com/gcyuan/SCUBA                              | [Marco et al., 2014](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC6460256/#B78) |
| DPT        | Diffusion maps                         | https://media.nature.com/original/nature-assets/nmeth/journal/v13/n10/extref/nmeth.3971-S3.zip | [Haghverdi et al., 2016](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC6460256/#B41) |
| TSCAN      | PCA                                    | https://github.com/zji90/TSCAN                               | [Ji and Ji, 2016](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC6460256/#B56) |
| Monocle2   | RGE                                    | http://cole-trapnell-lab.github.io/monocle-release/          | [Qiu et al., 2017](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC6460256/#B88) |
| Slingshot  | Any                                    | https://github.com/kstreet13/slingshot                       | [Street et al., 2018](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC6460256/#B107) |
| CellRouter | Any                                    | https://github.com/edroaldo/cellrouter                       | [Lummertz da Rocha et al., 2018](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC6460256/#B75) |

### 表6：scRNA-seq数据的可变剪接检测工具

| 工具         | URL                                                 | 参考文献                                                     |
| :----------- | :-------------------------------------------------- | :----------------------------------------------------------- |
| SingleSplice | https://github.com/jw156605/SingleSplice            | [Welch et al., 2016](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC6460256/#B121) |
| Expedition   | https://github.com/YeoLab/Expedition                | [Song et al., 2017](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC6460256/#B105) |
| BRIE         | https://github.com/huangyh09/brie                   | [Huang and Sanguinetti, 2017](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC6460256/#B49) |
| Census       | http://cole-trapnell-lab.github.io/monocle-release/ | [Qiu et al., 2017](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC6460256/#B88) |



Chen, G., Ning, B., & Shi, T. (2019). [Single-Cell RNA-Seq Technologies and Related Computational Data Analysis](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC6460256/). *Frontiers in genetics*, *10*, 317. https://doi.org/10.3389/fgene.2019.00317