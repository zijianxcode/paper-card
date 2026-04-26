# 初稿：Attention Is All You Need

---

## 模块 1：标题区

**主标题：**
抛弃 RNN 和 CNN，只用注意力——2017 年的这个决定重塑了整个 AI 领域

**论文标签：**
NeurIPS 2017 | Google Brain | 被引 10万+

---

## 模块 2：为什么该关心

2017 年之前，处理序列数据（语言、翻译、语音）几乎离不开循环神经网络（RNN/LSTM）。

但 RNN 有个根本性问题：它是顺序计算的。处理第 t 个词必须等前 t-1 个算完。长序列上，这既是速度瓶颈，也让远距离依赖难以捕捉。注意力机制（Attention）已经被用来缓解这个问题，但它始终只是 RNN 的附加组件，不是替代品。

这篇论文问了一个在当时看来近乎激进的问题：**如果彻底扔掉 RNN 和 CNN，仅仅靠注意力机制，能不能做得更好？**

答案是不仅能，而且好得多。这个架构就是 Transformer。

---

## 模块 3：他们是怎么做的

### 核心思路

Transformer 完全基于注意力机制，没有循环、没有卷积。整个架构是一个 encoder-decoder 结构，每层由两个子层组成：

1. **多头自注意力（Multi-Head Self-Attention）**——把输入序列中的每个位置和所有其他位置做注意力计算，一次到位捕获全局依赖
2. **前馈网络（FFN）**——在每个位置上独立做非线性变换

关键创新是三个：

**Scaled Dot-Product Attention（缩放点积注意力）**
把 query 和 key 做点积，除以 √dₖ 做缩放，然后用 softmax 得到权重来加权 value。这个缩放很关键——维度大时点积会膨胀，把 softmax 推到梯度极小的区域。

**Multi-Head Attention（多头注意力）**
不做一次注意力，而是把 query/key/value 投影到 h 个不同的低维子空间，并行做 h 次注意力，拼接起来再做一次投影。这让模型能从不同的表征子空间同时捕获信息。

**位置编码（Positional Encoding）**
由于没有 RNN 的序列结构，模型需要显式知道词的顺序信息。用不同频率的正余弦函数给每个位置编码一个向量，加到输入 embedding 上。

### 架构图（论文 Figure 1）

[截图位置：Transformer 架构图]

---

## 模块 4：结果怎么样

| 任务 | 指标 | 结果 | 意义 |
|------|------|------|------|
| WMT 2014 英→德 | BLEU | **28.4** | 超过此前所有模型（含集成模型）2 个 BLEU |
| WMT 2014 英→法 | BLEU | **41.8** | 单模型 SOTA，训练仅需 3.5 天（8 张 GPU） |
| 训练速度 | - | 仅需之前最佳模型的**一小部分**训练成本 |

更值得关注的是效率：自注意力层每层计算是常数级操作（O(1) sequential operations），而 RNN 需要 O(n) 步。长距离依赖的路径长度也是常数 O(1)，不像 RNN 是 O(n)、CNN 是 O(logₖ(n))。

---

## 模块 5：局限和启发

**局限：**
- 自注意力的计算复杂度是 O(n²·d)，序列很长时计算量和内存开销很大（这个问题后来催生了 Longformer、Linformer、稀疏注意力等工作）
- 没有 RNN 那样的隐状态，某种程度上失去了"在线处理"的自然性质

**启发：**
- Transformer 证明了"简单的架构 + 大规模数据 + 并行计算"是一条可行的道路
- 后续 BERT、GPT 系列、ViT 等工作都是直接站在这个地基上
- 可以说，2017-2024 的 AI 发展史，很大程度上是在消化这篇论文的洞见

---

## 模块 6：论文速览卡片

| 项 | 内容 |
|---|---|
| **论文标题** | Attention Is All You Need |
| **作者** | Ashish Vaswani, Noam Shazeer, Niki Parmar, Jakob Uszkoreit, Llion Jones, Aidan N. Gomez, Łukasz Kaiser, Illia Polosukhin |
| **单位** | Google Brain / Google Research / University of Toronto |
| **发表** | NeurIPS 2017 |
| **关键词** | Transformer, Self-Attention, Sequence-to-Sequence, Machine Translation |
| **链接** | https://arxiv.org/abs/1706.03762 |

---

## 初稿的笔记和问题

1. **主标题有点长**——可以再想想更精炼的版本。比如「告别 RNN：Transformer 的原始构想」？你有想法的话可以调整。
2. **模块 3 的架构图**——需要从论文里截 Figure 1 和 Figure 2（Scaled Dot-Product Attention / Multi-Head Attention），需要你确认截图位置。
3. **被引量**——我写的是 10万+，你可以根据实际数字修正。截至 2026 年应该远不止这个数。
4. **语气和深度**——你觉得整体偏浅还是偏深？外行和学术人士的平衡需要调整吗？
