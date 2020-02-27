# ELECTRA

中文 预训练 ELECTREA 模型: 基于对抗学习 pretrain Chinese Model
(非 官方版本)

# 模型说明
## 已完成

1. online-dynamic-masking：
	 - 采样 mask 位置
	 - sample tokens from multinomial-discributions（这里，使用均匀分布 输出 多项式分布的logits；可以使用其他先验分布采样）
2. generator 使用 MLM 优化，discriminator 使用 whole-token 的binary-classification
3. discriminator和generator 不共享任何参数
4. discriminator和generator的loss的权重为：10:1
5. 参数共享机制：
	 - generator、discriminator 共享所有encoder参数
	 - generator、discriminator 从pretrained model初始化所有参数
	 - generator、discriminator 从pretrained model初始化 token_embeddings（可配置初始化参数列表）
	 - generator、discriminator 共享 bert/embeddings/ scope下的所有参数
6. 训练机制：
     - generator 使用 MLM优化，discriminator 使用 binary-classification 优化，采样过程不可导，discriminator的loss无法回传到generator
     - adversarial contrastive estimation（使用REINFORECE优化）
7. 支持多种预训练模型结构 如 ALBERT、BERT 等

## 使用说明
模型参数 已经 转成 google-bert 官方 代码 加载（本地已经 将 discriminator 和 generator 分别 导出）

可以直接使用CLUE的评测代码训练和测试

### finetuning tips
1. learning rate 设置为 1e-4
2. epoch=10, 对于分类任务(TNEWS,IFLYTEK,epoch=20)
其余超参数与 bert-base finetuning相同
3. 由于使用roberta的方式训练，finetuning的时候 最好segment_ids都等于0（即使是CMNLI等句子对的任务）

# 模型路径
## 当前版本 beta
模型路径 <a href='https://drive.google.com/open?id=1-cOGrTX6ndGBWCPM0Alik6vVv4Bma1cD'>google-drive</a>

说明：本版本 仅 验证 整体流程是否 正确 包括 dynamic-masking、基于generator的扰动词生成 等

CLUE 结果(electra-all-mask-disc-tiny)：batch_size=32,长文本max_length=256,其余为128

模型配置

1. online-dynamic-masking
2. discriminator和generator 不共享任何参数
3. discriminator和generator的loss的权重为：10:1
4. discriminator使用whole-token 优化
5. 结果：
<img src="http://github.com/CLUEbenchmark/ELECTRA/blob/master/images/electra_tiny_beta_all_mask.jpg"  width="60%" height="60%" />
## 当前版本可能存在的问题
1. dynamic-masking 由于随机性，难以做到 whole-word-masking
2. discriminator训练过程中 即使 加大 鉴别器的loss，其 梯度更新依然缓慢，最终，whole-token的acc=0.93
     - 15% 的mask，generator最多60%的准确率，所以，replaced token占比由 15% 下降到 6%左右
     - 鉴别器最终的acc只有0.93（相当于 全部预测成 original token）

# 说明
代码正在整理中

