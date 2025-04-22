<div align="center">
  <h1>🦠 ACVirus</h1>
  <h3>Genomic sequence-based annotation of viral contigs within established ICTV taxonomic hierarchies</h3>

---

### Software Introduction
Without relying on cultivation, metagenomic sequencing greatly accelerated the novel virus detection. 

---

### 安装指南
#### 基础安装（Linux）
用户需要安装prodigal、diamond作为软件的依赖，推荐使用conda进行安装
```bash
conda install prodigal
conda install diamond
```
正确安装prodigal、diamond之后正式进入软件的安装流程
```bash
# 下载程序
git clone https://github.com/xiahaolong/ACVirus.git
# 添加到环境变量
export PATH=~:$PATH
```
#### 构建数据库
我们提供了VMR_MSL40.v1.20250307.xlsx版本的数据库序列，解压后可构建数据库便可使用
```bash
解压数据库
tar -xzvf acvirus_db.tar.gz
```
自行构建数据库可以在https://ictv.global/vmr直接下载对应的数据库文件，直接构建即可
```bash
ACVirus create_db --vmr acvirus/VMR_MSL40.v1.20250307.xlsx --dbpath acvirus_db
```
如果出现所有序列都无法从ncbi下载的情况则需要输入以下两条命令
```bash
export REQUESTS_CA_BUNDLE=/etc/ssl/certs/ca-certificates.crt
export REQUESTS_CA_BUNDLE=/etc/ssl/certs/ca-certificates.crt
```
**参数说明**：

| 参数 | 说明 |
|------|------|
| `--dbpath` | 数据库保存路径 |
| `--vmr` | ICTV 提供的 VMR Excel 文件路径 |
| `--restart` | （可选）是否启用断点续传模式（可能由于网络波动导致部分序列下载失败，该参数可以跳过已下载文件补齐序列） |

### 快速使用
#### 执行分类分析
```bash
ACVirus classify --data_path acvirus_db --contig test.fa --out output/
```
**参数说明**：

| 参数 | 说明 |
|------|------|
| `--contigs` | 输入的 contig 序列文件（FASTA 格式） |
| `--data_path` | 数据库路径（默认：`virus_db`） |
| `--out` | 结果输出目录（默认：`result`） |
#### 输出文件说明

| 文件名                  | 描述             |
| ----------------------- | ---------------- |
| `finally_result.tsv`    | 分类结果汇总     |
| `finally_network.csv`   | 网络图节点数据   |
| `network_edges.csv`     | 网络图边数据     |


### 绘制网络图谱
在指定目录生成两个pdf文件，其中一张是以上色的分类标准上色，另一张是以数据来源上色（test/database）
```bash
ACVirus draw --node_file ~/output/finally_node.csv --edge_file ~/output/finally_network.csv --out ~/output
```
**可选参数**：

| 参数 | 说明 |
|------|------|
| `--degree_threshold` | 最小连接度过滤阈值（默认：0） |
| `--top_n` | 上色的分类数量（默认：20） |
| `--taxa_color` | 用于上色的分类标准（默认：Family） |



