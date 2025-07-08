---

<div align="center">

# **ACVirus**

### Genomic sequence-based annotation of viral contigs within established ICTV virus taxonomic hierarchies

</div>
---


## Introduction

> Metagenomic sequencing has greatly accelerated the discovery of novel viruses without the need for cultivation. **ACVirus** enables accurate annotation of viral contigs based on genomic sequences, integrating them into the established ICTV taxonomy framework. The following is the pipeline of the **ACVirus**

![image-20250423183932745](https://wenguang.oss-cn-hangzhou.aliyuncs.com/figure/image-20250423183932745.png)

<img src="![project](https://wenguang.oss-cn-hangzhou.aliyuncs.com/figure/project.png)" alt="image-20250423195044780" style="zoom: 50%;" />



## Installation Guide

#### Dependency:

Before installation, we need make sure `prodigal` , `diamond` , `mafft` ,  `trimal` , `iqtree` are installed. We recommend using `conda` :

```bash
conda install -c bioconda prodigal diamond mafft trimal iqtree
```

#### Software Setup

Download the latest executable from the [GitHub Releases](https://github.com/xiahaolong/ACVirus/releases) page and add it to your system path:

```bash
tar -xzvf ACVirus.tar.gz
```

To facilitate the streamlined execution of ACVirus, we recommend declaring its decompression path as an environment variable. This integration enables system-wide accessibility through terminal commands. For instance:

```shell
export PATH="{your decompression path}:$PATH"  
```

#### **Extract the database**

```
git clone https://github.com/xiahaolong/ACVirus.git
sudo apt install p7zip-full  #Download and extract software
7z x acvirus_db.7z
```



## Quick Start

#### Classify Contigs

The ACvirus is friendly to use. It requires input as fasta format and will return the Summary of classification results  with the identified virus sequences. 

```bash
ACVirus classify --contig test.fa --data_path acvirus_db  --out output/
```

#####  Parameters

| Parameter     | Description                                      |
| ------------- | ------------------------------------------------ |
| `--contigs`   | Input contig FASTA file                          |
| `--data_path` | Path to the virus database (default: `virus_db`) |
| `--out`       | Output directory (default: `result`)             |

##### Output Files

| Filename            | Description                       |
| ------------------- | --------------------------------- |
| `final_result.tsv`  | Summary of classification results |
| `final_network.csv` | Network edge data                 |
| `final_node.csv` | Network node data                 |

---

#### Visualizing the Viral Network

ACVirus provides two color-coding types for network diagrams:

- Colored by **data source** (e.g., test/database)
- Colored by **taxonomy** (e.g., Family)

```bash
ACVirus draw --node_file output/final_node.csv --edge_file output/final_network.csv --out output
```

##### Optional Parameters

| Parameter            | Description                                        |
| -------------------- | -------------------------------------------------- |
| `--degree_threshold` | Minimum node degree for filtering (default: 0)     |
| `--top_n`            | Number of top taxa to highlight (default: 20)      |
| `--taxa_color`       | Taxonomy level used for coloring (default: Family) |

##### Example Figure

![image-20250424092604276](https://wenguang.oss-cn-hangzhou.aliyuncs.com/figure/image-20250424092604276.png)

<center>Example figure 1a: The visualization of the network graph with [msl40](https://ictv.global/sites/default/files/VMR/VMR_MSL40.v1.20250307.xlsx) as the test set (color by <b>data source</b>)</center>

![image-20250424092803591](https://wenguang.oss-cn-hangzhou.aliyuncs.com/figure/image-20250424092803591.png)

<center>Example figure 1b: The visualization of the network graph with [msl40](https://ictv.global/sites/default/files/VMR/VMR_MSL40.v1.20250307.xlsx) as the test set (color by <b>virus family</b>)</center>

<img src="https://wenguang.oss-cn-hangzhou.aliyuncs.com/figure/image-20250422201539626.png" alt="image-20250422201539626"  />

<center>Example figure 2a: The visualization of the network graph with  5,000 bacteriophagesas the test set (color by <b>data source</b>)</center>


![image-20250423175705655](https://wenguang.oss-cn-hangzhou.aliyuncs.com/figure/image-20250423175705655.png)

<center>Example figure 2b: The visualization of the network graph with  5,000 bacteriophagesas the test set (color by <b>virus family</b>)</center>

---

#### Phylogenetic tree and Genome comparison

Construct a phylogenetic tree, and draw a genome comparison diagram based on the order of the phylogenetic tree:

```bash
ACVirus tree --infile test.fasta --outdir outputfile
```
#####  Parameters

| Parameter     | Description                                      |
| ------------- | ------------------------------------------------ |
| `--infile`    | Input contig FASTA file                          |
| `--outdir`    | Output directory                                 |

##### Example Figure

![tree](https://wenguang.oss-cn-hangzhou.aliyuncs.com/figure/tree.png)
<center>Example figure 3a: Phylogenetic tree</center>

![genome_comparison](https://wenguang.oss-cn-hangzhou.aliyuncs.com/figure/genome_comparison.png)
<center>Example figure 3b: Genome comparison diagram based on the order of the phylogenetic tree</center>

## Other function

#### Build the ictv database

You can construct the database from scratch using the official [ICTV VMR Excel file](https://ictv.global/vmr) in :

```bash
ACVirus create_db --vmr VMR_MSL40.v1.20250307.xlsx --dbpath acvirus_db
```

####  If you encounter NCBI download ssl issues, try:

```bash
export REQUESTS_CA_BUNDLE=/etc/ssl/certs/ca-certificates.crt
export SSL_CERT_FILE=/etc/ssl/certs/ca-certificates.crt
```

#### Parameters

| Parameter         | Description                                                                 |
|-------------------|-----------------------------------------------------------------------------|
| `--dbpath`        | Path to save the database                                                   |
| `--vmr`           | Path to the ICTV VMR Excel file                                             |
| `--restart`       | *(Optional)* If a small portion of the sequences fail to download, enable resume mode to skip the sequences that have already been downloaded.        |

## The comparison results with other softwares

To validate the classification performance of the software tools, we first conducted benchmark testing using the [ICTV MSL39](https://ictv.global/sites/default/files/VMR/VMR_MSL39.v1_20240912.xlsx) dataset.

![image-20250423201625684](https://wenguang.oss-cn-hangzhou.aliyuncs.com/figure/image-20250423201625684.png)

Subsequently, to assess the classification performance under mutated viral conditions, we performed mutagenesis testing on the MSL39 dataset using the Mutation-Simulator (DOI:[10.1093/bioinformatics/btaa716](https://doi.org/10.1093/bioinformatics/btaa716)) software, evaluating the software's efficacy at mutation rates of 90% and 80%.

<img src="https://wenguang.oss-cn-hangzhou.aliyuncs.com/figure/image-20250423201849769.png" alt="image-20250423201849769" style="zoom: 80%;" />

<img src="https://wenguang.oss-cn-hangzhou.aliyuncs.com/figure/image-20250423201918063.png" alt="image-20250423201918063" style="zoom:80%;" />
