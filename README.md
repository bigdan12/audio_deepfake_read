# Audio Deepfake Detection 文章综述

| 文章 | 题目 | 会议/期刊 | Code | 出发点 | 做法 | 评分（1-5） |
|------|------|-----------|------|--------|------|-------------|
| 1 | Dual-Branch Knowledge Distillation for Noise-Robust Synthetic Speech Detection | TASLP |[GitHub](https://github.com/fchest/DKDSSD)   | 1. 检测合成音，用来保护用户隐私<br>2. 传统检测缺乏噪声鲁棒性 | 1. 增加 clean 系统进行蒸馏训练<br>2. denoised and noisy 特征融合 | 4.5 |
| 2 | CodecFake+: A Large-Scale Neural Audio Codec-Based Deepfake Speech Dataset | TASLP | [GitHub](https://github.com/ResponsibleGenAI/CodecFake-Source-Tracing) | 1. 主流大模型采用 codec 生成，传统鉴别模型难以区分<br>2. 基于 codec 的检测没有过多的数据 | 1. codec 扩展到 31 个、cosg 扩展到 17 个，更多更全的数据<br>2. 分析了影响的因素 | 3 |
| 3 | SafeEar: Content Privacy-Preserving Audio Deepfake Detection | ACM CCS | [GitHub](https://github.com/LetterLiGo/SafeEar) | 1. 传统合成音检测会保留内容信息，导致内容信息泄露<br>2. 现有的语音合成大模型均采用目标音色韵律进行 clone，不在意内容 | 1. 将语音分割成声学标记（音色韵律信息）和语义（内容）<br>2. 打乱语义信息，让攻击者无法获取到讲话内容<br>3. 提出了一个多语种的测试集 | 4 |
| 4 | SafeSpeech: Robust and Universal Voice Protection Against Malicious Speech Synthesis | USENIX Security | [GitHub](https://github.com/wxzyd123/SafeSpeech) | 通过在生成的音频中添加难以察觉的扰动，来保护用户音频隐私 | 1. 设计一个系统，保证音质的前提下增加扰动（TTS 无法 Finetune）<br>2. STOI and STFT 保证不可见域的稳定性 | 4 |
| 5 | Trident of Poseidon: A Generalized Approach for Detecting Deepfake Voices | CCS | `aisrc1@ssu.ac.kr` | 当前检测模型面临“泛化能力弱”的瓶颈 | 1. 设计三种训练策略提升模型泛化能力<br>2. 使用（VIB）可变信息瓶颈网络进行信息压缩<br>3. 新建一个 DSD-corpus 数据集<br>4. 探索了不同生成系统的共性（家庭性） | 4 |
| 6 | VoiceRadar: Voice Deepfake Detection using Micro-Frequency and Compositional Analysis | NDSS | [GitHub](https://github.com/AlirezaMohammadii/Deepfake-Detection-Voice-Authentication-Framework/) | 传统模型泛化能力差。深度学习模型需特定数据预处理，且对新型生成器泛化不足。 | 1. 将物理模型引入检测框架，利用两个物理现象来建模音频的微小频率变化<br>2. 创建了超 50 万条音频样本的数据集 | 4.5 |
| 7 | "Better Be Computer or I’m Dumb": A Large-Scale Evaluation of Humans as Audio Deepfake Detectors | CCS | - | 现有研究多关注 ML 模型，而人类的鉴别能力未被系统评估。 | 1. 比较人类与机器学习（ML）模型在三个主流音频深度伪造数据集（Wavefake、ASVspoof2021、FakeAVCeleb）上的分类表现。<br>2. 探索人类决策背后的逻辑（如语言特征、直觉、外部噪声等）。 | 3 |
| 8 | Blind and Low Vision Individuals’ Detection of Audio Deepfakes | CCS | - | 盲人和低视力人群高度依赖语音，但是鲜有人探索这类人群的鉴别能力 | 对美国 n=16 名盲人/低视力（各 8 名）开展用户研究，使用一半来自 ASVspoof2021 的样本（含真/伪）与一半用 ElevenLabs 克隆生成的样本，考察其分辨深伪能力与所用线索。 | 3 |
| 9 | Weakly-supervised Audio Temporal Forgery Localization via Progressive Audio-language Co-learning Network | IJCAI | [GitHub](https://github.com/ItzJuny/LOCO) | 1. 音频部分伪造（Partial Forgery Manipulation, PFM）：攻击者通过插入、替换或删除音频片段篡改内容，导致传统语句级检测方法（如 AASIST）失效。<br>2. 监督数据稀缺性：现有方法依赖帧级标注或精确边界标注，但这类标注在真实场景中成本高昂且难以获取。 | 1. 提出弱监督音频时序伪造定位方法（LOCO），仅需语句级标签（真实/伪造二分类）即可定位伪造区域。<br>2. 通过音频-语言协同学习和渐进式优化策略，解决弱监督下的语义不一致捕捉问题。 | 4.5 |
| 10 | Region-Based Optimization in Continual Learning for Audio Deepfake Detection | AAAI | [GitHub](https://github.com/cyjie429/RegO) | 伪造模型的发展，鉴别模型容易出现灾难性遗忘，即学习新任务后忘记旧任务知识。 | 1. 使用 Fisher 信息矩阵（FIM）分别衡量真实语音和伪造语音检测中各神经元的重要性。<br>2. 将神经元划分为四个区域，分别采用不同的梯度更新策略<br>3. 引入 Ebbinghaus 遗忘机制，释放冗余神经元，提升模型泛化能力。 | 4.5 |

# 文章详解
## 1.Dual-Branch Knowledge Distillation for Noise-Robust Synthetic Speech Detection
<img width="1614" height="970" alt="image" src="https://github.com/user-attachments/assets/482ebf92-c133-452b-ac44-17b10042954e" />

### 📌 出发点
- 传统的合成语音检测（Synthetic Speech Detection, SSD）方法在**带噪环境下鲁棒性较差**。
- 实际应用场景中，语音常受噪声干扰，导致现有检测模型性能显著下降。
- 因此，本文旨在提升检测模型在噪声环境下的鲁棒性。

### 🔧 做法

#### 1. 双分支知识蒸馏框架
- **Teacher 模型**：使用干净（clean）数据训练，具备高质量特征表示能力。
- **Student 模型**：使用带噪（noisy）数据训练，目标是学习 Teacher 的泛化能力。

#### 2. 降噪模块集成
- 在 Student 分支中引入语音增强（SE）模块，用于对带噪语音进行降噪。
- 降噪目标：逼近 clean 语音的 STFT 特征。

#### 3. 细节保留的特征融合设计
为防止 MSE Loss 忽略高频细节导致信息丢失，提出以下特征融合策略：

1. **提取双路特征**：
   - 将降噪后音频通过**低通滤波器**（去除高频噪声），再送入特征提取模块，得到 `xE`（保留低频内容）。
   - 原始带噪音频直接送入特征提取模块，得到 `xN`（保留噪声结构信息）。

2. **特征拼接与融合**：
   - 将 `xE` 和 `xN` 拼接，输入卷积层生成融合特征 `xfusion` 和权重 `w`。

3. **内容保护机制**：
   - 使用特定公式重新加权 `xE` 和 `xN`，防止关键语音内容丢失。

4. **最终融合特征**：
   - 融合特征为：  
     <img width="516" height="54" alt="image" src="https://github.com/user-attachments/assets/a356d1cb-d5a3-4395-93ae-47e52b52d0db" />

   - 其中，`w` 由 `xE` 经过平均池化和最大池化后生成（记为 `M`），用于自适应调节融合权重。

#### 4. 双重蒸馏损失函数
- **Hard Loss**：监督 Student 模型的预测结果与真实标签（GT）一致。
- **Soft Loss**：通过 KL 散度引导 Student 模型模仿 Teacher 模型输出的概率分布，实现知识迁移。


### 📈 结果
- 本文方法在**噪声场景下显著优于现有方法**。
- 在多个噪声条件下均取得 **SOTA（State-of-the-Art）性能**。
- 有效解决了传统 SSD 模型在真实世界应用中的噪声敏感问题。

### ✅ 总结
| 项目 | 内容 |
|------|------|
| 核心思想 | 利用 clean 数据指导 noisy 数据的训练（知识蒸馏） |
| 关键创新 | 特征融合机制 + 细节保护 + 双损失蒸馏 |
| 主要优势 | 强大的噪声鲁棒性 |
| 适用场景 | 高噪声环境下的合成语音检测（如智能助手、电话系统等） |

## 2.SafeEar: Content Privacy-Preserving Audio Deepfake Detection
<img width="772" height="280" alt="image" src="https://github.com/user-attachments/assets/bf996362-ff05-494c-a8d8-ccc536d87c26" />


### 📌 出发点
- 传统音频深伪检测模型在提取特征时会保留语音的**语义内容信息**，存在**隐私泄露风险**。
- 攻击者可能通过反向重建或ASR（自动语音识别）从检测特征中恢复原始讲话内容。
- 如何在**不依赖内容信息的前提下准确鉴别真伪语音**，是本文的核心问题。
- 特别地，当前大模型语音克隆主要模仿音色与韵律，而非内容，因此检测应聚焦于声学特征而非语义。



### 🔧 做法

SafeEar 提出一个四组件框架，分离并打乱语义信息，仅基于声学特征进行检测。

#### 1. 语义与声学信息分离（Speech Disentanglement）

- 使用 **RVQ-VAE**（Residual Vector Quantization）对语音进行编码。
- 利用 **HuBERT** 的中间层输出作为监督信号：
  - **第1层 VQ 码本**：强制逼近 HuBERT 第一层输出，使其编码**语义信息**（如词汇、语法）。
  - **后续7层 VQ 码本**：自然聚焦于**声学信息**（如音色、语调、节奏），实现解耦。

> 💡 关键：利用 HuBERT 的层次化语义特性，引导 RVQ 自动分离内容与声学特征。

#### 2. 声学特征增强与内容打乱（Privacy Enhancement）

- **Batch Normalization (BN) 层**：进一步提取和标准化声学特征，削弱内容相关统计特性。
- **Shuffle Layer**：对语义标记序列进行随机打乱  
  - 示例：原始句子 `"你好安大"` → 打乱后 `"大安你好"`  
  - 目的：彻底破坏语义连贯性，使攻击者无法还原原始内容。

> ⚠️ 注意：打乱仅作用于语义标记，不影响声学特征输入分类器。

#### 3. 基于声学特征的真伪分类（Deepfake Detection）

- 输入：从 RVQ 提取的**声学特征序列**
- 模型：使用 **Transformer 架构**进行序列建模与分类
- 输出：音频为真实或伪造的二分类结果

> ✅ 优势：分类器仅依赖音色、韵律等身份相关特征，完全不依赖语义。

#### 4. 鲁棒性增强（Robustness via Data Augmentation）

- 考虑实际部署中可能遇到的编码损失（如 MP3 压缩、AAC 转码等）。
- 在训练阶段引入**数据增强策略**：
  - 模拟多种压缩格式（MP3, OPUS, AAC）
  - 添加轻微噪声、重采样等扰动
- 提升模型在真实场景下的稳定性与泛化能力。



### 🗂️ 数据集

- 使用多个公开数据集进行评估：
  - ASVspoof2021
  - FakeAVCeleb
  - WaveFake
- **CvoiceFake**：作者构建的**多语种中文语音深伪数据集**
  - 包含普通话、粤语等多种中文口音
  - 覆盖多种TTS和VC生成技术
  - 用于验证方法在中文场景下的有效性

---

### 📈 实验结果

#### 1. 检测性能（EER ↓, t-DCF ↓）

- SafeEar 在多个数据集上达到**最优或接近最优的检测性能**。
- 尤其在跨数据集测试中表现出更强的**稳定性和泛化能力**。

#### 2. 内容隐私保护能力（ASR 可恢复性测试）

- 将 SafeEar 的输入特征送入 ASR 模型尝试恢复原始内容。
- 结果显示：
  - **ASR 识别准确率极低**（接近随机）
  - 无法有效还原原始语义内容
- 对比其他方法（如 raw waveform、mel-spectrogram），SafeEar 特征**不具备可读性**

> ✅ 结论：SafeEar 成功实现了**内容不可恢复性**，同时保持高检测精度。

---

### ✅ 总结

| 项目 | 内容 |
|------|------|
| 核心思想 | 解耦语义与声学信息，仅用声学特征检测深伪 |
| 关键技术 | RVQ + HuBERT 引导分离、Shuffle 打乱语义、Transformer 分类 |
| 隐私保障 | 语义被打乱，ASR 无法恢复内容 |
| 检测性能 | EER 和 t-DCF 指标领先，鲁棒性强 |
| 数据贡献 | 发布多语种中文深伪数据集 CvoiceFake |

---

## 3.SafeSpeech: Robust and Universal Voice Protection Against Malicious Speech Synthesis
<img width="1400" height="428" alt="image" src="https://github.com/user-attachments/assets/6763680f-9be4-438c-bdde-8ead2eb0c1c3" />

### 📌 出发点
- 随着 TTS（文本到语音）和语音克隆技术的发展，用户原始语音面临被恶意用于生成**语音深伪（Audio Deepfakes）** 的风险。
- 传统防御方法多集中在“事后检测”，缺乏主动防护机制。
- **SafeSpeech 提出一种主动防御范式**：在用户发布语音前，添加**人类难以察觉的扰动**，使攻击者无法基于该语音成功训练 TTS 模型。
- 目标：实现**鲁棒且通用的语音保护机制**，防止声音被滥用。

> ✅ 核心理念：**“让语音变得不可学习”**，而非仅“可检测”。

---

### 🔧 做法

SafeSpeech 设计了一个扰动生成系统，在保证原始语音可听性和自然度的前提下，注入对抗性扰动。

#### 1. 扰动生成目标
- 扰动需满足：
  - **听觉不可感知**（inaudible）：人类无法察觉音质变化
  - **破坏语音可学习性**：TTS 模型无法从中学习到真实音色

#### 2. 系统设计
- 输入：原始语音信号
- 输出：加扰后的“安全语音”（SafeSpeech）
- 关键技术：
   Pivotal Objective Optimization：提炼各类TTS训练目标的共性，找到能在多数模型上实现高效、普适的扰动优化目标（以mel谱重建损失Lmel为主），无需单独适配各模型损失并大幅加速扰动求解；
   SPEC（Speech PErturbative Concealment）：提出融合KL散度与mel谱距离的混合损失，使TTS模型“只能学到噪声”。相当于让你音频一经扰动，AI训练后只会变得无模仿价值。
   感知无感扰动优化：针对真用户体验，联合引入STOI和STFT两种听感相关损失促进扰动可听友好（听不出来，保真度高）。

<img width="763" height="757" alt="image" src="https://github.com/user-attachments/assets/4475fd06-24b8-4b2b-a0a6-412d78bfc278" />

---

### 🧪 实验结果

#### 1. 对 TTS 模型的防御效果

| 指标                | 使用 SafeSpeech 音频训练 TTS | 使用原始语音训练 TTS |
|---------------------|-------------------------------|------------------------|
| **音色相似度** ↓     | ❌ **显著降低**（最差）         | ✅ 高                   |
| **语音识别率** ↓     | ❌ 极低（难以识别内容）         | ✅ 正常                 |
| **语音自然度 (MCD)** ↓ | ❌ 明显下降                     | ✅ 接近真实              |

> ✅ 结论：SafeSpeech 生成的音频能**有效破坏 TTS 学习过程**，导致生成语音质量极差。

---

#### 2. 自身音质评估（加扰后语音的可用性）

| 指标                     | 结果 |
|--------------------------|------|
| **音色相似度 (SIM)** | 0.85 |
| **语音自然度 (MOS)**     | 3.02（5分制，**接近自然语音**） |

> ✅ 结论：扰动**不影响语音正常使用**，适合在社交平台、语音助手等场景部署。

---

## ✅ 总结

| 项目 | 内容 |
|------|------|
| 核心思想 | 主动防御：通过添加不可察觉扰动，使语音“不可学习” |
| 关键技术 | 对抗扰动生成 + 多模型联合优化 |
| 防御对象 | TTS 克隆、Finetune、Voice Conversion |
| 优势 | 鲁棒性强、通用性好、无需事后检测 |
| 局限 | 需在语音发布前处理，无法保护已泄露语音 |

> 🔗 论文发表于：**USENIX Security**  


---

## 4.Trident of Poseidon: A Generalized Approach for Detecting Deepfake Voices

### 📌 出发点
- 当前音频深伪检测模型面临 **“泛化能力弱”** 的核心瓶颈：
  - 在训练数据分布内表现良好（in-domain）
  - 但面对**新型生成技术、未知系统或跨域数据**时性能急剧下降
- 现有数据集多样性不足，导致模型过拟合特定伪造模式
- 本文目标：提出一种**强泛化、可迁移的通用检测框架**，应对真实世界中不断演进的深伪语音威胁

> ✅ 核心理念：构建“三叉戟”防御体系，全面提升模型泛化能力

---

### 🔧 做法

Trident of Poseidon 提出三大训练策略 + 信息压缩机制 + 高多样性数据集，形成完整泛化增强方案。

#### 1. 三重训练策略优化（The "Trident"）

##### (1) **Supervised Contrastive Learning (SCL)**  
- 目标：增强特征判别性
- 方法：
  - 同类样本（真实 or 伪造）在特征空间中拉近
  - 不同类样本推远
- 优势：提升模型对类别边界的敏感度，增强鲁棒性

##### (2) **Hard Adversarial Regularization (HAR)**  
- 目标：提升模型对“最难伪造”的样本的分辨能力
- 方法：
  - 使用神经 Vocoder 对**真实语音重新合成**（内容不变）
  - 生成带有轻微 AI 痕迹的“伪负样本”（看起来像假，实为真）
  - 将其作为**难负样本**加入训练
- 作用：迫使模型学习更细微的差异，避免依赖表面伪影

##### (3) **Progressive Batch Sampling (PBS)**  
- 目标：提升训练数据多样性和平衡性
- 方法：
  - 每个 batch 中确保真实与伪造样本数量均衡
  - 主动采样来自不同生成系统、语言、增强方式的样本
  - 避免模型偏向某一子集
- 优势：模拟真实复杂环境，提升模型适应能力

---

#### 2. 可变信息瓶颈网络（VIB, Variational Information Bottleneck）

- 问题：高维语音表征包含大量无关信息（如背景噪声、语言内容），易导致过拟合
- 解法：在分类器前端引入 **VIB 模块**
  - 将 Wav2Vec2 提取的高维特征压缩至低维隐空间
  - 只保留与“真假判别”最相关的特征
  - 通过 KL 正则化控制信息量，防止过拟合
- 结果：模型更关注**通用伪造痕迹**，而非特定系统伪影

> 🏗️ 模型架构：  
> `Wav2Vec2 (Feature Extractor)` → `VIB (Information Compression)` → `Classifier`

---

#### 3. DSD-corpus：高多样性深伪语音数据集

- 作者构建了一个大规模、多维度的数据集以支持泛化训练：
  - **40,000+ 条音频**
  - 覆盖：
    - 多种语音伪造系统（TTS、VC、Codec-based）
    - 多语言（中、英、日、韩等）
    - 多录制环境（安静、噪声、电话信道等）
    - 多声码器与编码方式
- 目标：打破“封闭世界”假设，推动开放域检测研究

---

### 🧪 实验结果

#### 1. 泛化性能（EER ↓）

| 测试场景       | EER (%) |
|----------------|---------|
| In-Domain      | **2.07%** |
| Out-of-Domain  | **3.78%** |

- 显著优于现有方法（如 AASIST、RawNet2 等）
- 在跨系统、跨语言、跨数据集测试中均保持稳定表现

---

#### 2. 数据集有效性（DSD-corpus）

- 使用 DSD-corpus 训练的模型：
  - 在除 DF21（封闭分布）外的所有测试集上**表现最优**
  - 表明数据集的**多样性有效驱动了泛化能力**
- 对比使用单一数据集训练的模型，泛化差距明显

---

#### 3. 消融实验（Ablation Study）

| 配置                     | 性能影响 |
|--------------------------|----------|
| PBS（主动采样）           | ✅ 提升最大，关键组件 |
| SCL（对比学习）           | 显著提升判别性 |
| HAR（难样本生成）         | 有效增强鲁棒性 |
| SCL + HAR + PBS（三者结合）| **性能显著优于任一单独策略** |

> ✅ 三叉戟协同作用，缺一不可

---

### 4. 深度伪造语音“家族性”分析

- **假设**：不同生成系统可能共享技术组件（如声码器、训练数据），形成“家族特征”
- **实验发现**：
  - 基于相同声码器（如 HiFi-GAN）生成的语音在跨检测任务中表现出**相似的行为模式**
  - 支持“家族性”假设存在
- **意义**：
  - 可用于未来检测器设计（如家族感知分类）
  - 揭示伪造系统的共性弱点

---

## ✅ 总结

| 项目 | 内容 |
|------|------|
| 核心思想 | 构建“三叉戟”训练策略 + 信息压缩 + 高多样性数据，提升泛化能力 |
| 关键技术 | SCL, HAR, PBS, VIB, DSD-corpus |
| 主要优势 | 强泛化、跨域鲁棒、可解释性增强 |
| 数据贡献 | 发布 DSD-corpus 大规模多源深伪数据集 |
| 开源信息 | 联系邮箱：`aisrc1@ssu.ac.kr`（暂无公开代码链接） |
| 论文发表 | **ACM CCS** ✅ |

---


## 5.VoiceRadar: Voice Deepfake Detection using Micro-Frequency and Compositional Analysis

<img width="780" height="644" alt="image" src="https://github.com/user-attachments/assets/e03f51c6-9e83-4b34-b4fe-fe5d305aa72f" />


### 📌 出发点
- 当前基于纯深度学习的音频深伪检测方法存在以下问题：
  - 严重依赖数据驱动，对**新型生成器泛化能力弱**
  - 需要特定预处理，难以迁移
  - 忽视语音生成过程中的**物理本质差异**
- 真人语音由复杂的生理机制（声带振动、口腔共振、头部运动等）产生，而 AI 语音缺乏这些物理动态
- **VoiceRadar 的核心思想**：将**物理建模**与机器学习结合
  - 利用两个物理现象建模音频中人类难以察觉的“微频率”（micro-frequency）变化
  - 揭示真实语音与 AI 生成语音在物理层面的本质差异

> ✅ 目标：构建一个**物理增强、高泛化、可解释性强**的深伪语音检测系统

---

### 🔧 做法

VoiceRadar 提出一种“物理先验 + 机器学习”的混合框架，通过建模两种物理现象提取深层特征。


#### 1. 两大物理原理

##### (1) **多普勒效应（Doppler Effect）**
- 原理：当声源与听者之间有相对运动时，接收到的频率会发生偏移
- 应用于语音：
  - 真人说话时头部轻微移动、呼吸起伏等会引起微小频率漂移
  - AI 语音是静态合成，缺乏此类动态频率调制
- 提取“观测频率” ，作为物理特征输入模型

##### (2) **鼓膜振动建模（Drumhead Vibrations）**
- 将语音信号建模为鼓面的振动模式
- 用贝塞尔函数（Bessel functions）和对应模式（圆向n、角向m）描述各种微频率（平移、旋转、振动）。

---

#### 2. 具体流程

##### Step 1: Frequency Distribution Analysis（微频率分析）
- 输入：HuBERT 提取的语音 embedding 序列
- 对 embedding 的时间变化进行频谱分析
- 利用贝塞尔函数拟合平移、旋转、振动三类微频率成分
- 输出：每类模式的能量分布与参数特征

##### Step 2: Wave-Based Analysis（波形聚合分析）
- 聚合微频率特征
- 结合多普勒效应公式，计算理论“观测频率” 
- 利用 embedding 的方差、空间分布和标签信息进一步增强特征表达

#### Step 3: Physics-Augmented ML Training（物理增强训练）
- 模型结构：**六层全连接神经网络**
- 损失函数设计：
  - 主损失：**BCE Loss**（二分类任务）
  - 正则项：**物理一致性损失**（约束预测结果与物理模型输出一致）
- 优势：物理先验作为归纳偏置，提升模型泛化性与鲁棒性

#### Step 4: Inference Phase（推理阶段）
- 输入任意音频，提取 HuBERT 特征 → 微频率建模 → 物理特征融合 → 分类器输出
- 实现端到端的“物理感知”检测

---

## 🧪 实验结果

### 1. 标准测试集性能（EER ↓）

<img width="814" height="234" alt="image" src="https://github.com/user-attachments/assets/d5f53b9d-b0ed-486c-8edc-619e7e131c6f" />


> ✅ VoiceRadar 在多个标准数据集上实现**SOTA 性能**

---

### 2. TTS 检测能力

- 测试多种主流 TTS 系统（如 Tacotron2, FastSpeech, VITS, YourTTS）
- 所有系统的 EER **均低于 0.01%**
- TPR（True Positive Rate）、TNR（True Negative Rate）、F1 Score 均接近 **1.0**

> ✅ 对当前主流 TTS 工具具备**近乎完美的检测能力**

---

### 3. Voice Conversion (VC) 检测能力

- 测试多种 VC 方法（如 StarGAN-VC, AutoVC, AdaIN-VC）
- 所有系统的 EER **均低于 0.016%**
- 性能稳定，F1 > 0.99

> ✅ 同样适用于高保真语音转换场景

---

### 4. 泛化能力验证

- 设计跨模型训练/测试实验：
  - 训练集：部分 TTS/VC 模型
  - 测试集：未见过的新生成器
- 结果：
  - VoiceRadar 在所有跨域设置下 EER 仍保持极低（Table VI & VII）
  - 显著优于纯数据驱动模型（如 AASIST、RawNet2）
- 表明：**物理特征具有强可迁移性**，不依赖特定生成伪影

---

## ✅ 总结

| 项目 | 内容 |
|------|------|
| 核心思想 | 将物理建模（多普勒 + 鼓膜振动）融入深度学习，捕捉微频率差异 |
| 关键技术 | 贝塞尔函数建模、HuBERT 特征分析、物理正则化损失 |
| 主要优势 | 高精度、强泛化、可解释性强、对新型生成器鲁棒 |
| 数据贡献 | 构建超 **50万条音频样本** 的大规模数据集（文中未命名，但规模远超现有） |
| 论文发表 | **NDSS 2024** ✅ |



## 6.Weakly-supervised Audio Temporal Forgery Localization via Progressive Audio-language Co-learning Network (LOCO)

  <img width="1717" height="666" alt="image" src="https://github.com/user-attachments/assets/51203104-5404-4eab-96d9-976eff3c4bb2" />


### 📌 出发点

#### 1. 音频部分伪造（Partial Forgery Manipulation, PFM）
- 攻击者通过**插入、替换或删除音频片段**（如“我同意”改为“我不同意”）篡改语义内容。
- 传统**语句级检测方法**（如 AASIST、RawNet）只能判断整段音频真假，**无法定位篡改区域**，在司法取证、新闻验证等场景中严重受限。

#### 2. 监督数据稀缺性
- 现有时序定位方法依赖**帧级标注**或**精确边界标注**（start/end timestamp）
- 但真实场景中：
  - 标注成本极高（需人工逐帧判断）
  - 难以规模化获取
- 因此，亟需一种**仅需语句级标签**（utterance-level: 真/伪）即可实现**高精度时序定位**的方法

> ✅ **LOCO 的目标**：实现**弱监督下的音频时序伪造定位**，无需精细标注，仍能精准识别伪造片段

---

### 🔧 做法

#### 问题建模
- **输入**：原始未裁剪音频 + 语句级标签（0=真，1=伪）
- **输出**：
  - 每帧的伪造概率序列
  - 局部伪造片段的起止时间戳（start, end）

#### 网络结构：LOCO 框架

LOCO 由三大核心模块构成：**协同学习、渐进细化、伪标签优化**

##### 1. 特征提取与双路建模

- **音频分支**：
  - 使用 **XLS-R-300M** 提取音频语义特征
  - 引入 **时序伪造注意力机制（Temporal Forgery Attention, TFA）**：
    - 增强模型对**局部时序异常变化**的敏感度
    - 聚焦于插入/删除等操作引起的突变点

- **语言分支（Prompt-enhanced Text Encoder）**：
  - 使用 **BERT 等文本 encoder** 编码提示信息，生成语义表示
  

#### 2. 协同学习机制（Audio-Language Co-learning）

- 两路特征均采用 **多实例学习（MIL）** 框架：）
  - 通过注意力机制聚合关键帧预测语句标签
- 引入 **KL 散度损失**：
  - 强制音频分支与语言分支的帧级预测分布对齐
  - 确保两路语义一致性，提升模型鲁棒性

#### 3. 渐进式自监督优化（Progressive Refinement）

- 利用模型自身预测生成 **伪标签（pseudo-labels）**
- 设计 **监督语义对比学习（Supervised Contrastive Learning, SCL）**：
  - 正样本对：同一类别的真实帧 or 伪造帧
  - 负样本对：真实帧 vs 伪造帧
  - 目标：最大化同类相似度，最小化异类相似度
- 通过多轮迭代，**渐进优化特征区分性**，提升定位精度

---

## 📊 损失函数设计

### 1. MIL Loss（P2sGrad-MSE）
- 对 top-K 高分帧取平均，计算与语句标签的 MSE
- 使用 **P2sGrad-MSE** 缓解正负样本不平衡问题
 <img width="748" height="113" alt="image" src="https://github.com/user-attachments/assets/ee11825e-5c3d-4139-858a-1ed08e142e24" />


### 2. KL 散度损失（语义一致性）
- 对齐音频与语言分支的帧级预测分布：
  <img width="823" height="64" alt="image" src="https://github.com/user-attachments/assets/cfce09d4-335a-4069-b623-314714d8b3c3" />


### 3. 监督对比损失（SCL）
- 增强类内紧凑性、类间分离性：
  <img width="592" height="159" alt="image" src="https://github.com/user-attachments/assets/217642cd-f509-465a-8bff-abc9c9fca9ca" />

  
---

## 🧪 实验结果

1.检测准确性（Table 1）：LOCO极大超越主流弱监督方法（EER/ACC/AUC），弱标签下几乎可逼近全监督SOTA，且全监督模型一旦失去帧级精细标注表现即大幅下跌。
2.定位质量（Table 2）：mAP、AR等指标上LOCO在三大数据集均达前所未有的水平，远超现有弱监督方法，在部分阈值下也逼近甚至超越全监督方法。
3.消融实验：去除A2LC关键分支（时序注意力、Prompt增强）、移除渐进细化、替换loss函数均会明显恶化定位性能，各模块贡献互补且显著。
4.阈值测试：固定0.5阈值获得最佳mAP，过高阈值反而损害性能

## ✅ 总结

| 项目 | 内容 |
|------|------|
| 核心思想 | 弱监督下实现音频时序伪造定位，无需帧级标注 |
| 关键技术 | XLS-R + TFA + Prompt-enhanced Language Branch + MIL + KL + SCL |
| 主要优势 | 高定位精度、低标注依赖、强泛化性 |
| 创新点 | 音频-语言协同学习、渐进式伪标签优化、TFA 注意力机制 |
| 开源代码 | [GitHub - LOCO](https://github.com/ItzJuny/LOCO) ✅ |
| 论文发表 | **IJCAI** ✅ |


---

# 7.Region-Based Optimization in Continual Learning for Audio Deepfake Detection (RegO)
<img width="1322" height="986" alt="image" src="https://github.com/user-attachments/assets/d3c727c5-28a2-485a-8971-591eb9f1736d" />


## 📌 出发点

音频深度伪造技术不断演进（新TTS、VC、Codec模型层出不穷），检测模型面临两大挑战：

### 1. **灾难性遗忘（Catastrophic Forgetting）**
- 模型在学习新型伪造技术时，会**遗忘对旧伪造类型或真实语音的判别能力**
- 导致在持续部署中性能持续下降

### 2. **标注数据稀缺且分布动态变化**
- 新伪造技术出现快，标注成本高
- 传统一次性训练模型难以适应**持续增长的任务流**

> ✅ **RegO 的目标**：提出一种**面向音频深伪检测的持续学习框架**，在不遗忘旧知识的前提下，高效学习新伪造类型

---

## 🔧 做法

RegO 提出一个三模块协同的持续学习框架，通过**神经元区域划分与差异化优化**实现知识保留与高效更新。

### 1. 重要性区域定位（Importance Region Localization, IRL）

- 目标：识别对不同任务关键的神经元
- 方法：
  - 使用 **Fisher 信息矩阵（FIM）** 评估每个神经元对以下任务的重要性：
    - 真实语音检测（Real Speech Detection）
    - 伪造语音检测（Fake Speech Detection）
  - 根据重要性将神经元划分为四个区域：

| 区域 | 名称 | 判定条件 |
|------|------|----------|
| A | 不重要区域 | 对真实和伪造检测均不重要 |
| B | 仅真实重要 | 仅对真实语音检测重要 |
| C | 仅伪造重要 | 仅对伪造语音检测重要 |
| D | 共享重要区域 | 对两类任务均重要 |

> 💡 核心思想：不同神经元承担不同语义角色，应区别对待

---

### 2. 区域自适应优化（Region-Aware Optimization, RAO）

针对四类区域采用**差异化梯度更新策略**，平衡学习与遗忘：

| 区域 | 名称 | 更新方式 | 目的 |
|------|------|----------|------|
| A | 不重要区域 | 直接 fine-tune（全参数更新） | 快速适应新任务，提升学习效率 |
| B | 仅真实重要 | **平行投影更新**（Parallel Projection） | 保持真实语音判别能力稳定 |
| C | 仅伪造重要 | **正交更新**（Orthogonal Update） | 避免干扰已有伪造知识 |
| D | 共享重要区域 | **自适应更新**（Adaptive Step Size） | 平衡双任务稳定性，防止过拟合 |

> ✅ 通过几何优化策略，在参数空间中实现“选择性学习”

---

### 3. Ebbinghaus 遗忘机制（Ebbinghaus Forgetting Mechanism, EFM）

受人类记忆遗忘规律启发（艾宾浩斯遗忘曲线），设计动态神经元释放机制：

#### 每完成一个任务后执行：
1. **计算神经元活跃度**：
   - 使用梯度幅值、FIM 值等衡量神经元在训练中的“参与度”
2. **按遗忘函数筛选**：
   - 高活跃度神经元：保留，作为长期记忆
   - 低活跃度神经元：标记为“可遗忘”
3. **释放冗余神经元**：
   - 将低活跃度神经元重新初始化或解冻
   - 用于学习后续新任务，提升模型容量利用率

> 💡 类比：定期“清理缓存”，为新知识腾出空间

---

## 🧪 实验结果

### 1. 数据集与基线

| 类别 | 内容 |
|------|------|
| **数据集** | 跨语言、跨任务混合数据流，包含 8 个数据集：<br>• FMFCC<br>• ASVspoof2019<br>• ASVspoof2021<br>• FakeAVCeleb<br>• WaveFake<br>• Logical Access (LA)<br>• Physical Access (PA)<br>• In-The-Wild recordings |
| **对比方法** | 11 种主流持续学习算法：<br>• EWC (Elastic Weight Consolidation)<br>• GEM (Gradient Episodic Memory)<br>• RWM (Random Walk Memory)<br>• LwF, iCaRL, MAS, etc. |

---

### 2. 主要性能（EER ↓, ACC ↑）

<img width="1370" height="630" alt="image" src="https://github.com/user-attachments/assets/fd35f036-008f-4ee6-811b-dd107dc64a1b" />


> ✅ RegO 在所有任务阶段均显著优于基线方法，**遗忘率最低**

---

### 3. 消融实验（Ablation Study）

<img width="1152" height="364" alt="image" src="https://github.com/user-attachments/assets/ef3aea04-f814-471d-a1f6-13a4aa618491" />

> ✅ 三大模块**协同作用显著**，缺一不可

---

### 4. 自适应更新有效性验证
- 在共享重要区域（D区）使用自适应步长：
  - 初始学习率高 → 快速收敛
  - 随训练动态衰减 → 防止震荡
- 结果：模型收敛更快，最终性能更稳定

---

## ✅ 总结

| 项目 | 内容 |
|------|------|
| 核心思想 | 将神经元按“真实/伪造”重要性划分区域，差异化优化 |
| 关键技术 | Fisher 重要性分析 + 区域自适应梯度更新 + Ebbinghaus 遗忘机制 |
| 主要优势 | 有效缓解灾难性遗忘、提升持续学习效率、适用于动态伪造环境 |
| 创新点 | 首次区分“真实 vs 伪造”神经元重要性，提出语音检测专用持续学习框架 |
| 论文发表 | **AAAI** ✅ |


---



# 📚 数据集总览

|  Attack Types  | Years| Dataset   |  Number of Audio  <br>（Subdataset：Real/Fake） 	  |    Language   |
|:-----------:|:------------:|:------------:|:-------------:|:------------:|
|TTS|2019|FoR<br>[Paper](https://ieeexplore.ieee.org/document/8906599) [Dataset](http://bil.eecs.yorku.ca/datasets)|111,000/87,000|English|
|TTS|2021|WaveFake<br>[Paper](https://arxiv.org/abs/2111.02813) [Dataset](https://zenodo.org/record/5642694)|16,283/117,985|English, Japanese|
|TTS|2021|Half-truth<br>[Paper](https://arxiv.org/abs/2104.03617)|53,612/107,224|Chinese|
|TTS|2021|HAD<br>[Paper](https://arxiv.org/abs/2104.03617)|53,612/107,224|Chinese|
|TTS|2021|FakeAVCeleb <br>[Paper](https://arxiv.org/pdf/2108.05080) [Dataset](https://github.com/DASH-Lab/FakeAVCeleb/tree/main)|10,209/11,335|English|
|TTS|2022|ADD 2022<br>[Paper](https://arxiv.org/pdf/2202.08433v2.pdf)|LF: 5,619/46,067<br>PF: 5,319/46,419<br>FG-D: 5,319/46,206|Chinese|
|TTS|2022|CMFD<br> [Paper](https://ieeexplore.ieee.org/document/9921343) [Dataset](https://github.com/WuQinfang/CMFD)|Chinese: 1,800/1,000<br>English: 1,800/1,000|English, Chinese|
|TTS|2022|In-the-Wild<br>[Paper](https://arxiv.org/abs/2203.16263) [Dataset](https://deepfake-demo.aisec.fraunhofer.de/in_the_wild)|19,963/11,816|English|
|TTS|2022|CFAD<br>[Paper](https://arxiv.org/abs/2207.12308) [Dataset](https://zenodo.org/record/8122764)|38,600/77,200|Chinese|
|TTS|2022|Psynd<br>[Paper](https://ieeexplore.ieee.org/document/9956134) [Dataset](https://scholarbank.nus.edu.sg/handle/10635/227398)|2,294|English|
|TTS|2022|TIMIT-TTS<br>[Paper](https://arxiv.org/abs/2209.08000) [Dataset](https://zenodo.org/records/6560159)|0/79,120|English|
|TTS|2023|ODSS<br>[Paper](https://ieeexplore.ieee.org/abstract/document/10374863) [Dataset](https://zenodo.org/records/8370669)|11,032/18,993|English, German,<br>and Spanish|
|TTS|2024|MLAAD<br>[Paper](https://arxiv.org/abs/2401.09512) [Dataset](https://deepfake-total.com/mlaad)|-/76,000|Multi-lingual|
|TTS|2024|CD-ADD<br>[Paper](https://arxiv.org/abs/2404.04904)|300 hours|English|
|TTS|2024|DiffSSD<br>[Paper](https://arxiv.org/abs/2409.13049) [Dataset](https://huggingface.co/datasets/purdueviperlab/diffssd)|24,226/70,000|English|
|TTS|2024|ACCENT<br>[Paper](https://arxiv.org/abs/2409.08346) |53,651/192,461|Multi-lingual|
|TTS|2024|SpoofCeleb<br>[Paper](https://arxiv.org/abs/2409.17285) [Dataset](http://www.jungjee.com/spoofceleb/)|250k+|English|
|TTS|2024|LlamaPartialSpoof<br>[Paper](https://arxiv.org/abs/2409.14743) [Dataset](https://github.com/hieuthi/LlamaPartialSpoof)|10,573/33,479|English|
|TTS|2024|FakeSound<br>[Paper](https://arxiv.org/abs/2406.08052) [Dataset](https://github.com/FakeSoundData/FakeSound)|-/3,798|English|
|TTS|2024|DFADD<br>[Paper](https://arxiv.org/abs/2409.08731) [Dataset](https://github.com/isjwdu/DFADD)|44,455/163,500|English|
|TTS|2024|SONAR<br>[Paper](https://arxiv.org/abs/2410.04324) [Dataset](https://github.com/Jessegator/SONAR)|-/2,274|Multi-lingual| 
|TTS|2025|6KSFx<br>[Paper](https://arxiv.org/abs/2501.17198) [Dataset](https://github.com/nellyngz95/6KSFX)|-/6,000|-| 
|TTS|2025|BangalFake<br>[Paper](https://arxiv.org/html/2505.10885) [Dataset](https://huggingface.co/datasets/sifat1221/banglaFake)|12,260/13,260|Bengali| 
|Replay|2017|ASVspoof 2017<br>[Paper](https://www.asvspoof.org/asvspoof2017overview_cameraReady.pdf) [Dataset](https://datashare.ed.ac.uk/handle/10283/3055)|3,565/14,465|English|
|Replay|2019|ReMASC<br>[Paper](https://arxiv.org/abs/1904.03365) [Dataset](https://github.com/YuanGongND/ReMASC)|9,240/45,472|English, Chinese,<br>Hindi|
|Replay|2019|VSDC<br>[Paper](https://arxiv.org/abs/1909.00935) [Dataset](http://www.secs.oakland.edu/∼mahmood/datasets/audiospoof)|1,687/11,772|English|
|Replay|2024|POLIPHONE<br>[Paper](https://arxiv.org/abs/2410.06221) [Dataset](https://zenodo.org/records/13903412)|41,941|English|
|TTS <br>and VC|2015|AVspoof<br>[Paper](https://ieeexplore.ieee.org/document/7358783) [Dataset](https://zenodo.org/record/4081040)|LA: 15,504/120,480<br>PA: 15,504/14,465|English|
|TTS <br>and VC|2015|ASVspoof 2015<br>[Paper](https://datashare.ed.ac.uk/bitstream/handle/10283/853/ASVspoof2015_evaluation_plan.pdf?sequence=2&isAllowed=y) [Dataset](http://dx.doi.org/10.7488/ds/298)|16,651/246,500|English|
|TTS <br>and VC|2021|FMFCC-A<br> [Paper](https://arxiv.org/abs/2110.09441) [Dataset](https://github.com/Amforever/FMFCC-A)|10,000/40,000|Chinese|
|TTS <br>and VC|2022|SceneFake<br>[Paper](https://arxiv.org/abs/2211.06073) [Dataset](https://zenodo.org/record/7663324#.Y_XKMuPYuUk)|19,838/64,642|English|
|TTS <br>and VC|2022|EmoFake<br>[Paper](http://arxiv.org/abs/2211.05363)|35,000/53,200|English, Chinese|
|TTS <br>and VC|2023|PartialSpoof<br>[Paper](https://arxiv.org/abs/2104.02518) [Dataset](https://zenodo.org/record/4817532#.YLd8Yi2l1hF)|12,483/108,978|English|
|TTS <br> and VC|2023|ADD 2023<br>[Paper](https://arxiv.org/abs/2305.13774)|FG-D: 172,819/113,042<br>RL: 55,468/65,449<br>AR: 14,907/95,383|Chinese|
|TTS <br>and VC|2023|DECRO<br>[Paper](https://dl.acm.org/doi/10.1145/3543507.3583222) [Dataset](https://github.com/petrichorwq/DECRO-dataset)|Chinese: 21,218/41,880<br>English: 12,484/42,799|English, Chinese|
|TTS <br>and VC|2023|HABLA<br>[Paper](10.21437/Interspeech.2023-2272) [Dataset](https://github.com/Ruframapi/HABLA)|22,000/58,000|Spanish|
|TTS <br>and VC|2024|DeepFakeVox-HQ<br>[Paper](https://ieeexplore.ieee.org/document/10800535)|693k/643k|English|
|TTS <br>and VC|2024|VoiceWukong<br>[Paper](https://arxiv.org/abs/2409.06348) [Dataset](https://voicewukong.github.io)|5,300/413,400|English, Chinese|
|TTS <br>and VC|2024|Speech-Forensics<br>[Paper](https://www.ijcai.org/proceedings/2024/0046) [Dataset](https://github.com/ring-zl/Speech-Forensics)|13,100/7,452|English|
|TTS <br>and VC|2024|VoiceEdit<br>[Paper](https://arxiv.org/abs/2402.06304)|-|Multi-lingual|
|TTS <br>and VC|2024|RFP<br>[Paper](https://arxiv.org/abs/2404.17721) [Dataset](https://zenodo.org/records/10202142)|28,115/74,199|English|
|TTS <br>and VC|2025|MADD<br>[Paper](https://ieeexplore.ieee.org/document/10800535)|60,000/129,990|Multi-lingual|
|TTS <br>and VC|2025|XMAD-Bench<br>[Paper](https://arxiv.org/pdf/2506.00462) [Dataset](https://github.com/ristea/xmad-bench/)|414,858|Multi-lingual|
|TTS <br>and Vocoder|2024|Diffuse or Confuse<br>[Paper](https://arxiv.org/abs/2410.06796) [Dataset](https://github.com/AntonFirc/diffusion-deepfake-speech-dataset/)|131,000/183,400|English|
|TTS <br>and Vocoder|2025|ShiftySpeech<br>[Paper](https://arxiv.org/abs/2502.05674) [Dataset](https://huggingface.co/datasets/ash56/ShiftySpeech)|3,000+ hours|English, Chinese,<br>Japanese|
|TTS, VC<br>and Replay|2019|ASVspoof 2019<br>[Paper](https://arxiv.org/abs/1904.05441) [Dataset](https://datashare.ed.ac.uk/handle/10283/3336)|LA: 12,483/108,978<br>PA: 28,890/189,540|English|
|TTS, VC<br>and Replay|2021|ASVspoof 2021<br>[Paper](https://arxiv.org/abs/2109.00535) [Dataset](https://www.asvspoof.org/index2021.html)|LA: 18,452/163,114<br>PA: 126,630/816,480<br>PF: 14,869/519,059|English|
|TTS, VC<br>and Vocoder|2024|SpeechFake<br>[Paper](https://openreview.net/forum?id=GpUO6qYNQG)|3,000+ hours|English|
|VC, Replay<br>and Adversarial|2024|VSASV<br>[Paper](https://www.isca-archive.org/interspeech_2024/hoang24b_interspeech.html) [Dataset](https://github.com/hustep-lab/VSASV-Dataset)|164,000/174,000|Multi-lingual|
|TTS, VC<br>and Adversarial|2024|ASVspoof 5<br>[Paper](https://arxiv.org/abs/2502.08857) [Dataset](https://zenodo.org/records/14498691)|188,819/ 815,262|English|
|Voice Cloning|2021|RTVCSpoof<br>[Paper](https://ojs.aaai.org/index.php/AAAI/article/view/6044/5900)|3,284/4,843|English|
|Voice Cloning|2024|Kratika Dataset<br>[Paper](https://doi.org/10.1145/3658664.3659658)|24,226/25,000|English|
|Vocoder|2022|Yan Dataset<br>[Paper](https://doi.org/10.1145/3552466.3556525)|8,200/63,200|Chinese|
|Vocoder|2023|LibriSeVoc<br>[Paper](https://arxiv.org/abs/2304.13085) [Dataset](https://github.com/csun22/SyntheticVoice-Detection-Vocoder-Artifacts)|13,201/79,206|English|
|Vocoder|2023|Voc.v1-v4<br>[Paper](https://arxiv.org/abs/2210.10570) [Dataset](https://github.com/nii-yamagishilab/project-NN-Pytorch-scripts/tree/master/project/09-asvspoof-vocoded-trn)|2,580/10,320|English|
|Vocoder|2024|MLADDC<br>[Paper](https://openreview.net/forum?id=ic3HvoOTeU) [Dataset](https://speech007.github.io/MLADDC_Nips/)|80k/160k|Multi-lingual|
|Vocoder|2024|CVoiceFake<br>[Paper](https://dl.acm.org/doi/10.1145/3658644.3670285) [Dataset](https://safeearweb.github.io/Project/)|23,544/91,700|Multi-lingual|
|Impersonation|2024|IPAD<br>[Paper](https://arxiv.org/abs/2408.17009)|5,170/18,874|Chinese|
|Text-To-Music (TTM)|2024|FSD<br>[Paper](https://arxiv.org/abs/2309.02232) [Dataset](https://github.com/xieyuankun/FSD-Dataset)|200/500|Chinese|
|Text-To-Music (TTM)|2024|SingFake<br>[Paper](https://arxiv.org/abs/2309.07525) [Dataset](https://github.com/yongyizang/SingFake)|634/671|Multi-lingual|
|Text-To-Music (TTM)|2024|CtrSVDD<br>[Paper](https://arxiv.org/abs/2406.02438) [Dataset](https://github.com/SVDDChallenge/)|32,312/188,486|Multi-lingual|
|Text-To-Music (TTM)|2024|FakeMusicCaps<br>[Paper](https://arxiv.org/abs/2409.10684) [Dataset](https://zenodo.org/records/13732524)|5.5k/27,605|English|
|Text-To-Music (TTM)|2025|SONICS<br>[Paper](https://arxiv.org/abs/2408.14080)|48,090/49,074|English|
|Text-To-Music (TTM)|2025|SingNet<br>[Paper](hhttps://arxiv.org/pdf/2505.09325) [Dataset](https://singnet-dataset.github.io/)|2,963.4 hours|Multi-lingual|
|Codec-based Speech <br>Generation (CoSG)|2024|CodecFake<br>[Paper](https://arxiv.org/abs/2406.07237) [Dataset](https://codecfake.github.io/)|42,752/45,045|English|
|Codec-based Speech <br>Generation (CoSG)|2024|ALM-ADD<br>[Paper](https://arxiv.org/abs/2408.10853) [Dataset](https://github.com/xieyuankun/ALM-ADD)|123/210|English|
|Codec-based Speech <br>Generation (CoSG)|2024|Codecfake<br>[Paper](https://ieeexplore.ieee.org/abstract/document/10830534) [Dataset](https://zenodo.org/records/13838106)|132,277/925,939|English, Chinese|
|Codec-based Speech <br>Generation (CoSG)|2025|ST-Codecfake<br>[Paper](https://arxiv.org/pdf/2501.06514) [Dataset](https://zenodo.org/records/14631091)|13,228/145,778|English, Chinese|
|Codec-based Speech <br>Generation (CoSG)|2025|CodecFake+<br>[Paper](https://arxiv.org/abs/2501.08238)|90,163/1,423,894|English|


# <span id="ae">Audio Enhancement Methods</span>

|                                                                                                                                                                               Method 	                                                                                                                                                                               |                                                                Description 	                                                                 |
|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------:|:--------------------------------------------------------------------------------------------------------------------------------------------:|
|                                                                                                                           SpecAugment <br> [Paper](https://arxiv.org/pdf/1904.08779.pdf)   [Code](https://github.com/DemisEom/SpecAugment)                                                                                                                           |                               Enhancement strategies include time warping, frequency masking and time masking                                |  
|                                                                                                                     WavAugment <br> [Paper](https://arxiv.org/abs/2007.00991)  [Code](https://github.com/facebookresearch/WavAugment)                                                                                                                     | Enhancement strategies include pitch randomization, reverberation, additive noise, time dropout (temporal masking), band reject and clipping | 
|                                                              RawBoost  <br> [Paper](https://arxiv.org/abs/2007.00991)      [Code](https://github.com/TakHemlata/RawBoost-antispoofing)                                                               | Enhancement strategies include linear and non-linear convolutive noise, impulsive signal-dependent additive noise and stationary signal-independent additive noise |    



# 🔧 开源工具汇总

| 仓库名称 | 仓库地址 | 做法 |
|----------|----------|------|
| Codecfake | [https://github.com/xieyuankun/Codecfake](https://github.com/xieyuankun/Codecfake) | 针对 Codecfake 的检测 |
| ST-Codecfake | [https://github.com/xieyuankun/ST-Codecfake](https://github.com/xieyuankun/ST-Codecfake) | 针对 Codecfake 的检测 |
| asv-2021 | [https://github.com/asvspoof-challenge/2021](https://github.com/asvspoof-challenge/2021) | 针对 CM（Countermeasure）的鉴别，包含四个基线模型 |
| Asvspoof5 | [https://github.com/asvspoof-challenge/asvspoof5](https://github.com/asvspoof-challenge/asvspoof5) | 针对 CM 的鉴别，包含两个基线模型 |
| ctrsvdd | [https://github.com/SVDDChallenge/CtrSVDD2024_Baseline](https://github.com/SVDDChallenge/CtrSVDD2024_Baseline) | 文生音乐（Text-to-Music）的鉴别，包含五个基线模型 |
| ShiftySpeech | [https://github.com/Ashigarg123/ShiftySpeech/tree/main](https://github.com/Ashigarg123/ShiftySpeech/tree/main) | 大规模生成数据（3000+小时）鉴别 |
| SSL_Anti-spoofing | [https://github.com/TakHemlata/SSL_Anti-spoofing](https://github.com/TakHemlata/SSL_Anti-spoofing) | 利用 wav2vec2 和数据增强实现 |
| SafeEar | [https://github.com/LetterLiGo/SafeEar](https://github.com/LetterLiGo/SafeEar) |  |

# 📊 现状评估

### 🔗 测试代码
- **项目地址**：[https://github.com/xieyuankun/Codecfake](https://github.com/xieyuankun/Codecfake)
- **模型结构**：  
  `Speech` → `Wav2vec2` → `AASIST` → 输出得分与真假判断

---

### 📈 运行结果

| 音频类型 | 音频特点 | UTMOS1 得分 | UTMOS2 得分 | 鉴别模型得分（Codecfake） |
|----------|----------|-------------|-------------|----------------------------|
| **TTS 录音** | | 自有音色库（选取3600个文件，平均分 3.87） | 自有音色库（选取3600个文件，平均分 2.87） | 自有音色库（共89,777个音频文件，平均得分 92.70）<br>• 低于 90 分：17.95%<br>• 低于 80 分：10.54%<br>• 低于 60 分：5.10% |
| **ASR 录音** | 主要使用了 `changchen` 和 `bmw_studio` 的部分音频 | `changchen`（12,000 个文件，平均分 3.50） | `changchen`（12,000 个文件，平均分 2.20） | `changchen`（12,000 个文件，平均分 97.58）<br>• 90 分以上：95.29%<br>• 80 分以上：96.84%<br><br>`bmw_studio`（83,118 个文件，平均分 98.33）<br>• 90 分以上：96.07%<br>• 80 分以上：98.06% |
| **线上实车语音** | 音频包含明显噪音 | 共12,855 个文件，平均分 1.91 | 共12,855 个文件，平均分 1.39 | 先评分了部分共 67,314 个文件，平均得分 9.74 |
| **爬取录音** | 使用了爬取的 Bilibili 音频进行打分，一个文件可能有几分钟，存在 BGM | 共1,268 个文件，平均分 1.47 | 共1,268 个文件，平均分 1.95 | 使用了部分音频文件共 2,917 个，平均分 71.12<br>• 90 分以上：59.92%<br>• 80 分以上：64.15% |
| **自研 TTS** | `banxia`、`bantong`、`banshu` 三个音色；内容为报纸标题和学习强国类短语 | 共10,089 个音频文件，平均分 3.45 | 共10,089 个音频文件，平均分 2.56 | 共有合成音频文件 10,704 个，平均得分 7.75<br>• 90 分以下：99.50%<br>• 80 分以下：98.71%<br>• 60 分以下：96.51%<br>• 10 分以下：82.53% |
| **微软 TTS** | `xiaoxiao` 微软音色生成 | 共2,271 个音频文件，平均分 3.53 | 共2,271 个文件，平均分 2.85 | 共2,271 个文件，平均分 7.02<br>• 90 分以上：0.18%<br>• 80 分以上：0.97% |
| **CosyVoice2.0 方案** | | 共2,126 个文件，平均分 3.26 | 共2,126 个音频文件，平均分 2.06 | 共2,126 个音频文件，平均分 8.66<br>• 90 分以上：1.83% |
| **SeedVC** | `xiaoxiao_seedvc` | 共2,271 个文件，平均分 2.93 | 共2,271 个文件，平均分 2.38 | 共2,271 个文件，平均分 0.14<br>• 除一个文件为 73 分、一个为 21.52 分外，其余均低于 10 分 |
| **cflow-vc** | | 共3,317 个文件，平均分 2.81 | 共3,317 个文件，平均分 2.05 | 共1,082 个文件，平均分 0.07，评分均在 20 分以下 |

---

### 📊 结果分析

#### （1）从模型的角度来看：
- **UTMOS1**：
  - 在大部分音频类型上表现较为稳定；
  - 但对于存在明显噪声和 BGM 影响的音频上表现较差；
  - 真音频（不考虑噪声/BGM影响）整体得分优于合成音频。
- **UTMOS2**：
  - 打分较为严格，分数之间差异较小；
  - 真音频与合成音频之间的得分差距不大。
- **Codecfake 模型**：
  - 分数波动较大，对真/伪音频有非常明显的区分能力；
  - 但受噪声和 BGM 影响较大；
  - 在带 BGM 的音频上表现优于 UTMOS1 和 UTMOS2。

#### （2）从音频类型来看：
- 真实音频（不考虑噪声和 BGM）在各模型中的表现整体优于合成音频。
- 在真实音频中：
  - **Codecfake** 对干净的真实音频给出最高分；
  - **UTMOS2** 给分最为严格；
- 所有模型均受到一定程度的噪声和 BGM 影响，其中 **Codecfake 在带 BGM 的音频上表现相对更好**。

#### （3）从合成音频来看：
- **TTS 合成音频** 整体得分高于 **VC（Voice Conversion）合成音频**。
- 尤其是 SeedVC 和 cflow-vc 等 VC 方案，在 Codecfake 模型下得分极低（接近 0），表明其生成痕迹明显，易被检测。

---

## 🔍 Zero-Shot 能力测试（CosyVoice2.0）

使用 CosyVoice2.0 进行 zero-shot 合成，提示音频使用不同等级的录音。录音等级划分为四级：

| 等级 | 发言人名称 | 描述 | 录音评分（%） | zero-shot 生成评分（Codecfake） |
|------|------------|------|----------------|-------------------------------|
| 一级 | `zero_shot_prompt`, `yae` | 专业录音水平 | 17.16 | 0.02 |
| 二级 | `dingzhen`, `houyi` | 音质尚可，读法不自然（一字一顿） | 64.56 | 0.04 |
| 三级 | `zxx`, `jay` | 存在明显噪音，韵律自然 | 46.06 | 0.11 |
| 四级 | `sfb` | 会议室录制，存在拖音现象 | 99.99 | 0.46 |

> ⚠️ **现象观察**：
> - Codecfake 对**专业录音水平**的音频反而评分极低（如一级录音仅得 0.02 分）；
> - 而对**低质量录音**（如四级 `sfb`，存在拖音）反而评分最高（0.46）；
> - **可能原因**：专业录音过于正式、发音“完美”，缺乏自然波动，被模型误判为“非人类”或“合成”特征；
> - 相反，带有轻微缺陷（如拖音、噪音）的语音更接近“自然人类”行为，导致评分略高。

---











