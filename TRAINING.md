# GNN4siRNA 训练复现说明

## 环境准备

### 1. 安装 pixi

```bash
curl -fsSL https://pixi.sh/install.sh | bash
```

### 2. 初始化 pixi 项目

```bash
cd GNN4siRNA
pixi init gnn4sirna-env
cd gnn4sirna-env
```

### 3. 配置 pixi.toml

将以下内容替换 `pixi.toml`:

```toml
[workspace]
channels = ["conda-forge"]
name = "gnn4sirna-env"
platforms = ["linux-64"]
version = "0.1.0"

[tasks]

[dependencies]
python = "3.11.*"
pandas = "*"
numpy = "*"
scikit-learn = "*"
scipy = "*"
tensorflow = { version = "2.12.*", channel = "conda-forge" }

[pypi-dependencies]
stellargraph = "*"
chardet = "*"
```

### 4. 安装环境

```bash
pixi install
```

## 运行训练

### 方法一：使用 pixi exec

```bash
cd GNN4siRNA
pixi exec -p gnn4sirna-env python model/GNN4siRNA.py
```

### 方法二：直接调用环境中的 Python

```bash
cd GNN4siRNA/gnn4sirna-env
./.pixi/envs/default/bin/python ../model/GNN4siRNA.py
```

## 训练配置

训练使用 `model/params.py` 中的配置：

- 数据集：dataset_2（702个siRNA-mRNA相互作用）
- batch_size: 60
- epochs: 10
- HinSAGE layer_sizes: [32, 16]
- hop_samples: [8, 4]
- dropout: 0.15
- learning_rate: 0.001
- loss: mse
- 验证：10折交叉验证

## 训练结果

- **Overall PCC (Pearson相关系数): 0.3423**
- **Overall MSE (均方误差): 0.0397**

## 代码修改说明

原代码针对旧版pandas编写，需进行以下修改以兼容新版pandas：

1. `model/GNN4siRNA.py` 第79-83行：修改索引设置方式
2. 第114-115行：使用 `.iloc[]` 进行索引切片
3. 第165行：使用 `.flatten()` 展平预测结果

详细修改可参考已修改的 `model/GNN4siRNA.py`。

## 数据说明

- `data/processed/dataset_2/sirna_kmers.txt` - siRNA的k-mer特征
- `data/processed/dataset_2/target_kmers.txt` - mRNA的k-mer特征
- `data/processed/dataset_2/sirna_target_thermo.csv` - siRNA-mRNA热力学特征
- `data/processed/dataset_2/sirna_mrna_efficacy.csv` - 效能标签