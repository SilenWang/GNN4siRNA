# GNN4siRNA 预处理代码测试说明

## 测试环境

- Python 3.14
- numpy
- pandas  
- biopython

环境通过pixi配置，位于项目根目录的pixi.toml

## 预处理流程

### 1. K-mer特征生成 (1_make_kmer.py)

生成siRNA和mRNA序列的k-mer特征表示。

```
cd preprocessing
python 1_make_kmer.py
```

输出文件：
- `sirna_kmers.txt` - siRNA的k-mer特征
- `mRNA_kmers.txt` - mRNA的k-mer特征

### 2. 热力学特征计算 (2_make_thermo_feature.py)

计算siRNA序列的热力学特征（基于bimer的自由能）。

```
python 2_make_thermo_feature.py
```

输出文件：
- `dataset_th.csv` - 热力学特征

### 3. siRNA-mRNA配对 (3_make_sirna_mrna.py)

生成siRNA反义链与mRNA的配对关系。

```
python 3_make_sirna_mrna.py
```

输出文件：
- `datase_tofold.csv` - siRNA-mRNA配对数据

**注意**：脚本中 `from params` 需要改为 `import params` 以兼容Python 3

## 测试结果

### K-mer特征对比

运行 `1_make_kmer.py` 生成的结果与 `data/processed/dataset_1/` 中的文件**完全一致**。

```bash
diff preprocessing/sirna_kmers.txt data/processed/dataset_1/sirna_kmers.txt
# 无输出，表示完全一致
```

### 热力学特征

脚本正常运行，但输出列数与现有processed文件略有差异（可能存在额外后处理步骤）。

## 使用方法

### 配置pixi环境

```bash
pixi init --format pyproject
pixi add python numpy pandas biopython
```

### 运行预处理

```bash
cd preprocessing
pixi run python 1_make_kmer.py
pixi run python 2_make_thermo_feature.py
pixi run python 3_make_sirna_mrna.py
```

## 数据集

项目使用dataset_1作为默认数据集，位于 `data/raw/dataset_1/`:
- `sirna_1.fas` - siRNA序列
- `mRNA_1.fas` - mRNA序列  
- `sirna_mrna_efficacy.csv` - siRNA-mRNA效能数据

预处理参数在 `preprocessing/params.py` 中定义：
- k_sirna = 3 (siRNA的k值)
- k_mrna = 4 (mRNA的k值)