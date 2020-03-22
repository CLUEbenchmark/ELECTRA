# ELECTRA

中文 预训练 ELECTREA 模型: 基于对抗学习 pretrain Chinese Model


code Repost from google official code: https://github.com/google-research/electra

具体使用说明：参考 官方链接

# Electra Chinese tiny模型路径
## google drive
electra-tiny <a href='https://drive.google.com/file/d/1UP4byt4-kgenwST0KvyMYNbln6FfaSLp/view?usp=sharing'>google-drive</a>
## baidu drive
electra-tiny <a href='https://pan.baidu.com/s/14b-IiCkjRg-6XIYPXnezZA'>baidu-pan</a>
code:rs99


## 模型说明
1. 与 tinyBERT 的 配置相同
2. generator 为 discriminator的 1/4

# How to use official code
## Steps
1. 修改 configure_pretraining.py 里面的 数据路径、tpu、gpu 配置
2. 修改 model_size：可在 code/util/training_utils.py 里面 自行定义模型大小
3. 数据输入格式：原始的input_ids, input_mask, segment_ids，训练过程中会在线 做 uniform mask sampling（不需要离线 生成 masked input ids）

# Performance
## gen+disc:
electra-tiny
| metric | value | 
| --- | --- | 
| disc_accuracy | 0.95093095 | 
| disc_auc | 0.9762006 |
| disc_loss | 0.14071295 |
| disc_precision | 0.8018275 |
| disc_recall | 0.6088053 |
| loss | 9.516352 |
| masked_lm_accuracy | 0.46732807 |
| masked_lm_loss | 2.8209455 |
| sampled_masked_lm_accuracy | 0.3504382 |

The model are trained on CLUE 10G Chinese Corpus with 1M-steps

## Downstream finetuning on <a href='https://www.cluebenchmarks.com/small_model_classification.html'>CLUE benchmark</a>:
注：only use pretrained electra-tiny with layer-wise learning rate decay without any distilaltion、data-augmentation. learning rate is set to 1e-4 for each task and run 10-epochs. (According to official results, the results may have large variance)

|     | AFQMC | TNEWS | IFLYTEK | CMNLI  | WSC  | CSL |
| --- | ---   | ---   | ---     | ---    | ---  |---  |
| Metrics | Acc | Acc | Acc | Acc  | Acc  | Acc |
| ELECTRA-tiny | 70.319 | 54.280 | 53.538 |  73.745 | 64.336  | 78.700 | 
| Roberta-tiny | 69.904 | 54.150 | 56.808 |  74.037 | 64.336  | 74.133 |

注：
1. electra 在 多分类问题上面 可能会有 performance 下降
2. gen、disc的规模 配比 比较hacky，与 mask的方法 等相关


<a href='https://www.cluebenchmarks.com/NLPCC.html'>报名NLPCC-高性能小模型测评</a>


