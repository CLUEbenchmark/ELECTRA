# ELECTRA

中文 预训练 ELECTREA 模型: 基于对抗学习 pretrain Chinese Model
(非 官方版本)

包括小号版本、tiny版本的中文的模型
# 模型说明
## 已完成
1. online-dynamic-masking：
  - 采样 mask 位置
  - sample tokens from multinomial-discributions（这里，使用均匀分布 输出 多项式分布的logits；可以使用其他先验分布采样）
2. generator 使用 MLM 优化，discriminator 使用 whole-token 的binary-classification
3. discriminator和generator 不共享任何参数
4. discriminator和generator的loss的权重为：10:1
5. 参数共享机制：
  1. generator、discriminator 共享所有encoder参数
  2. generator、discriminator 从pretrained model初始化所有参数
  3. generator、discriminator 从pretrained model初始化 token_embeddings（可配置初始化参数列表）
  4. generator、discriminator 共享 bert/embeddings/ scope下的所有参数
6. 训练机制：
  1. generator 使用 MLM优化，discriminator 使用 binary-classification 优化，采样过程不可导，discriminator的loss无法回传到generator
  2. adversarial contrastive estimation（使用REINFORECE优化）
7. 支持多种预训练模型结构 如 ALBERT、BERT 等

中文版近期（年前）开源，敬请期待！
# 模型路径
## 当前版本 beta
模型路径 google-drive：https://drive.google.com/open?id=1-cOGrTX6ndGBWCPM0Alik6vVv4Bma1cD

说明：本版本 仅 验证 整体流程是否 正确 包括 dynamic-masking、基于generator的扰动词生成 等

CLUE 结果(electra-all-mask-disc-tiny)：

模型配置

1. online-dynamic-masking
2. discriminator和generator 不共享任何参数
3. discriminator和generator的loss的权重为：10:1
4. discriminator使用whole-token 优化
5. 结果：

![image](http://github.com/CLUEbenchmark/ELECTRA/blob/master/images/electra_tiny_beta_all_mask.png)



# 说明
代码正在整理中

