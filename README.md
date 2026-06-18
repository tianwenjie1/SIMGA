# SIMGA: Selective Gated Mamba for Sequential Recommendation
This is the implementation of the submission "SIGMA: Selective Gated Mamba for Sequential Recommendation".
## Configuration of the environment
The hardware and software we used are listed below to facilitate the environment's configuration. The detailed environment setting can be found in the `requirements.txt`. You can use pip install to reproduce the environment.
- Hardware:
  - GPU: one NVIDIA L4
  - CUDA: 11.8
- Software:
  - Python: 3.10.13
  - Pytorch: 2.1.1 + cu118
- Usage
  - Install Causal Conv1d
    - `pip install causal-conv1d==1.1.3.post1`
  - Install Recbole
    - `pip install recbole==1.2.0`
  - Install Mamba
    - `pip install mamba-ssm==1.1.4`
A detailed configuration process in Colab can be found in the `RecMamba.ipynb`
##  Datasets
The procedures for preprocessing the datasets are listed as follows:
- The raw datasets should be preprocessed using the Conversion tool provided by `https://github.com/RUCAIBox/RecSysDatasets`. After you acquire the atomic files, please put them into `dataset/<Amazon_Fashion/Amazon_Sports_and_Outdoors/Amazon_Video_Games/amazon-beauty/ml-1m/yelp>`. The Yelp Dataset can be found at `https://www.yelp.com/dataset`; The Amazon datasets can be found at `https://cseweb.ucsd.edu/jmcauley/datasets.html\#amazon_reviews`; The MovieLens-1M dataset can be found at `https://grouplens.org/datasets/movielens/`.
- Or you can directly download the atomic files of these datasets using the Baidu disk link provided by Recbole: `https://github.com/RUCAIBox/RecSysDatasets`.

For the procedure of filtering the cold-start users and items, please find the corresponding part in the `model/config.yaml`

## Model Training
- You can directly run the `model/run.py` to reproduce the training procedure.
- For evaluation on grouped users, please refer to `model/run_custrainer.py`.

## Citation
If you found the code and the paper are useful, please kindly cite our paper:
```bibtex
@article{liu2024bidirectional,
  title={Bidirectional gated mamba for sequential recommendation},
  author={Liu, Ziwei and Liu, Qidong and Wang, Yejing and Wang, Wanyu and Jia, Pengyue and Wang, Maolin and Liu, Zitao and Chang, Yi and Zhao, Xiangyu},
  journal={arXiv preprint arXiv:2408.11451},
  year={2024}
}
```


你现在已经证明三件事了：

1. 环境能跑；
2. SIGMA 模型代码能跑；
3. ML-1M 结果已经比较接近论文。

真正卡住的是 Amazon 这些数据。论文表格里的 Games 不是你现在直接下载的原始 Amazon_Video_Games.inter，而是经过某套过滤/预处理后的版本。问题是作者没有直接给一个“最终处理好、统计完全对齐论文”的数据包，所以你现在自己用 RecBole 下载的数据跑出来，统计就对不上。

你现在 Games 原始数据是：

raw users: 2,766,656
raw items: 137,249
raw inters: 4,555,500

论文 Games 要的是大概：

users: 55,145
items: 17,287
avg length: 9.01

而你默认过滤后变成：

users: 13,407
items: 10,314
inters: 118,011

这说明不是训练问题，是数据处理版本不一致。
