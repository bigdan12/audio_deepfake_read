# Audio Deepfake Detection 


A summary of recent research papers on audio deepfake detection, including key insights, methods, and resources.

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
