---

<div align="center">

# 🦠 **ACVirus**

### Genomic sequence-based annotation of viral contigs within established ICTV taxonomic hierarchies

</div>

---

## 📦 Introduction

> Metagenomic sequencing has greatly accelerated the discovery of novel viruses without the need for cultivation. **ACVirus** enables accurate annotation of viral contigs based on genomic sequences, integrating them into the established ICTV taxonomy framework.

---

## ⚙️ Installation Guide

### 🔧 Prerequisites (Linux)

Make sure `prodigal` and `diamond` are installed. We recommend using `conda`:

```bash
conda install prodigal
conda install diamond
```

### 📥 Software Setup

Download the latest executable from the [GitHub Releases](https://github.com/xiahaolong/ACVirus/releases) page and add it to your system path:

```bash
export PATH=~:$PATH
```

---

## 🧬 Database Construction

### 📚 Provided Database

We provide a ready-to-use database: `VMR_MSL40.v1.20250307.xlsx`.

```bash
# Clone and extract the database
git clone https://github.com/xiahaolong/ACVirus.git
sudo apt install p7zip-full  #Download and extract software
7z x acvirus_db.7z
```

You may also construct the database from scratch using the official ICTV VMR Excel file:

```bash
ACVirus create_db --vmr acvirus/VMR_MSL40.v1.20250307.xlsx --dbpath acvirus_db
```

#### 🔐 If you encounter NCBI download issues, try:

```bash
export REQUESTS_CA_BUNDLE=/etc/ssl/certs/ca-certificates.crt
export SSL_CERT_FILE=/etc/ssl/certs/ca-certificates.crt
```

#### 📝 Parameters

| Parameter      | Description                                                                 |
|----------------|-----------------------------------------------------------------------------|
| `--dbpath`     | Path to save the database                                                   |
| `--vmr`        | Path to the ICTV VMR Excel file                                             |
| `--restart`    | *(Optional)* If a small portion of the sequences fail to download, enable resume mode to skip the sequences that have already been downloaded.        |

---

## 🚀 Quick Start

### 🧪 Classify Contigs

```bash
ACVirus classify --data_path acvirus_db --contig test.fa --out output/
```

#### 📝 Parameters

| Parameter      | Description                                |
|----------------|--------------------------------------------|
| `--contigs`    | Input contig FASTA file                    |
| `--data_path`  | Path to the virus database (default: `virus_db`) |
| `--out`        | Output directory (default: `result`)       |

---

## 📁 Output Files

| Filename                 | Description              |
|--------------------------|--------------------------|
| `finally_result.tsv`     | Summary of classification results |
| `finally_network.csv`    | Network node data        |
| `network_edges.csv`      | Network edge data        |

---

## 🖼️ Visualizing the Viral Network

ACVirus can generate two network diagrams:

- Colored by **taxonomy** (e.g., Family)
- Colored by **data source** (e.g., test/database)

```bash
ACVirus draw --node_file ~/output/finally_node.csv --edge_file ~/output/finally_network.csv --out ~/output
```

#### 📝 Optional Parameters

| Parameter            | Description                                     |
|----------------------|-------------------------------------------------|
| `--degree_threshold` | Minimum node degree for filtering (default: 0)  |
| `--top_n`            | Number of top taxa to highlight (default: 20)  |
| `--taxa_color`       | Taxonomy level used for coloring (default: Family) |



