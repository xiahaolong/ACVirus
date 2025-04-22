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
tar -xzvf acvirus_db.tar.gz
```

You may also construct the database from scratch using the official ICTV VMR Excel file:

```bash
ACVirus create_db --vmr acvirus/VMR_MSL40.v1.20250307.xlsx --dbpath acvirus_db
```

#### 🔐 If you encounter NCBI download issues, try:

```bash
export REQUESTS_CA_BUNDLE=/etc/ssl/certs/ca-certificates.crt
```

#### 📝 Parameters

| Parameter      | Description                                                                 |
|----------------|-----------------------------------------------------------------------------|
| `--dbpath`     | Path to save the database                                                   |
| `--vmr`        | Path to the ICTV VMR Excel file                                             |
| `--restart`    | *(Optional)* Enable resume mode to skip already downloaded sequences        |

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

---

### 🧭 For more details, please refer to the [Documentation](https://github.com/xiahaolong/ACVirus/wiki)

---

如果你还有 logo、截图、流程图等素材，可以考虑在开头加一张整体工作流程图（比如 `.png`），进一步提升 README 的可读性和专业感。

需要我也可以帮你生成一个网络图流程图或者交互式 badge（比如安装状态、版本号等），想加的话告诉我就好。