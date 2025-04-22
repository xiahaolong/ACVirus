---

<div align="center">

# **ACVirus**

### Genomic sequence-based annotation of viral contigs within established ICTV virus taxonomic hierarchies

</div>
---


## Introduction

> Metagenomic sequencing has greatly accelerated the discovery of novel viruses without the need for cultivation. **ACVirus** enables accurate annotation of viral contigs based on genomic sequences, integrating them into the established ICTV taxonomy framework.




## Installation Guide

#### Dependency:

Before installation, we need make sure `prodigal` and `diamond` are installed. We recommend using `conda` :

```bash
conda install -c bioconda prodigal diamond
```

#### Software Setup

Download the latest executable from the [GitHub Releases](https://github.com/xiahaolong/ACVirus/releases) page and add it to your system path:

```bash
wget https://github.com/xiahaolong/ACVirus/releases/download/ACVirusv1.0/ACVirus.tar.gz
tar -xzvf ACVirus.tar.gz
```

To facilitate the streamlined execution of ACVirus, we recommend declaring its decompression path as an environment variable. This integration enables system-wide accessibility through terminal commands. For instance:

```shell
export PATH="{your decompression path}/ACVirus:$PATH"  
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

| Filename              | Description                       |
| --------------------- | --------------------------------- |
| `finally_result.tsv`  | Summary of classification results |
| `finally_network.csv` | Network node data                 |
| `network_edges.csv`   | Network edge data                 |

---

#### Visualizing the Viral Network

ACVirus provides two color-coding types for network diagrams:

- Colored by **taxonomy** (e.g., Family)
- Colored by **data source** (e.g., test/database)

```bash
ACVirus draw --node_file output/finally_node.csv --edge_file output/finally_network.csv --out output
```

##### Optional Parameters

| Parameter            | Description                                        |
| -------------------- | -------------------------------------------------- |
| `--degree_threshold` | Minimum node degree for filtering (default: 0)     |
| `--top_n`            | Number of top taxa to highlight (default: 20)      |
| `--taxa_color`       | Taxonomy level used for coloring (default: Family) |

##### Example Figure

<img src="https://wenguang.oss-cn-hangzhou.aliyuncs.com/figure/image-20250422201539626.png" alt="image-20250422201539626"  />

## Other function

#### Build the ictv database

You can construct the database from scratch using the official [ICTV VMR Excel file](ICTV VMR Excel file) in :

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

---



