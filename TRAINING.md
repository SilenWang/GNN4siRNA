# GNN4siRNA 训练复现说明

## 环境准备

```bash
cd gnn4sirna-env
pixi shell
cd ..
```

## 运行训练

```bash
cd model
python model/GNN4siRNA.py
```

## 训练配置

训练使用 `model/params.py` 中的原有配置，未进行调整：

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
- 实际训练结果PCC和MSE有所浮动

## 代码修改说明

原代码针对旧版pandas编写，Minimax+Opencode对以下部分进行了修改以保证运行，暂未进行人工核对：

1. `model/GNN4siRNA.py` 第79-83行：修改索引设置方式
2. 第114-115行：使用 `.iloc[]` 进行索引切片
3. 第165行：使用 `.flatten()` 展平预测结果

详细修改可参考已修改的 `model/GNN4siRNA.py`。

## 数据说明

- `data/processed/dataset_2/sirna_kmers.txt` - siRNA的k-mer特征
- `data/processed/dataset_2/target_kmers.txt` - mRNA的k-mer特征
- `data/processed/dataset_2/sirna_target_thermo.csv` - siRNA-mRNA热力学特征
- `data/processed/dataset_2/sirna_mrna_efficacy.csv` - 效能标签