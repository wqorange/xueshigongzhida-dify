# 🎓 学工智答 · AI 高校政策智能问答系统

> 用 AI 让高校学生事务从"等待"变"即时" · 第十九届"挑战杯"全国大学生课外学术科技作品竞赛 参赛项目

[![Dify](https://img.shields.io/badge/Dify-Chatflow-blue)](https://dify.ai)
[![LLM](https://img.shields.io/badge/LLM-qwen3.8--max-orange)](https://tongyi.aliyun.com)
[![RAG](https://img.shields.io/badge/RAG-Hybrid+Rerank-green)](docs/架构图.md)

---

## 👋 关于我

**wqorange** — 河南 · 中原工学院软件学院

- **AI Agent / RAG / Dify 工作流编排** 开发者
- 独立完成学工智答 Dify Chatflow 工作流设计、知识库构建、提示词工程
- 第十九届"挑战杯"全国大学生课外学术科技作品竞赛 **"人工智能+"专项赛** 参赛选手

---

## 🎯 项目一句话

> 学工智答是一个基于 **Dify Chatflow + RAG** 的高校政策智能问答系统，为学生提供 **7×24 小时即时响应**的政策咨询服务，将平均等待时间从 48 小时压缩至 **< 3 秒**。

## 📊 核心数据（试点 3 个月）

| 指标 | 数值 | 我的贡献 |
|---|---|---|
| 累计处理咨询 | ~10,000 次 | 工作流设计 + 知识库构建 |
| 首次解答率 | **85%** | 提示词工程 + 查询优化节点 |
| 人工复核准确率 | **91%** | 混合检索 + qwen3-rerank |
| 响应时间 | **< 3 秒** | RAG 流水线调优 |
| 学工人员工作量减少 | **60%+** | 问答自动化覆盖重复咨询 |
| 混合检索 F1 值 | **0.87** | 关键词+向量混合（较 TF-IDF 提升 42%） |

## 🏗️ 技术亮点

### 1. 三段式 Chatflow 架构（查询优化 → 检索 → 回答）

```
开始 → LLM 3 (查询优化) → 知识检索 (混合检索 + rerank) → LLM (主回答) → Answer
```

**为什么加查询优化？** 学生口语化提问（"贫困补助怎么拿"）直接搜知识库召回率低。用独立 LLM 把口语→标准术语 + 关键词 + 政策领域分类，再拿去检索，F1 值从 0.61 提升到 **0.87**。

### 2. 混合检索 + qwen3-rerank 重排序

```
keyword (0.3)  +  vector (0.7)  →  TopK=4  →  qwen3-rerank  →  最终 context
```

纯向量检索容易错过关键词精确匹配（如"国奖"vs"国家奖学金"），纯关键词又不懂语义。混合检索 + 重排序在召回率和精确率之间取得平衡。

### 3. 提示词工程：强制规则 + 正反示例

主 LLM 提示词（3084 字符）包含：
- **助学金 vs 奖学金强制区分规则**：助学金看"家庭经济困难"，奖学金看"成绩"，国家励志奖学金两者都要。回答时主动说明前置条件，避免 AI 混淆
- **"理解后用自己的话回答"**：禁止照抄原文，要求阅读 → 提取 → 重组 → 口语化表达
- **错误示例 + 正确示例**：对比式示例让模型学会正确的回答方式

### 4. 语音交互（普通话 + 英语）

- speech_to_text ✅ 已开启，支持学生语音输入
- 回答结构清晰、条目化，天然适合语音播报场景
- 国际学生可直接用英语提问

### 5. 知识库全生命周期覆盖

当前 9 个知识库，42,846 字，覆盖：第二课堂学分、发展团员、家庭经济困难认定、奖学金政策、团委部门职能、发展党员等全生命周期政策。

## 📁 仓库结构

```
学工智答系统/
├── README.md                    ← 你正在看的（求职导向）
├── LICENSE                      ← MIT
├── docs/
│   ├── 学工智答_项目作品集_公开版.pdf  ← 完整项目作品集
│   ├── 项目需求文档.md                ← MVP + V2 规划、知识库、试点数据
│   └── 架构图.md                      ← 5 张架构图（ASCII + Mermaid）
├── dify_export/
│   ├── workflow.json                  ← Dify Chatflow 工作流 DSL
│   └── knowledge_base.json            ← 9 个知识库元数据导出
├── assets/                            ← 系统截图（待补充）
└── .gitignore                         ← 屏蔽密钥、大文件、DSH 配置
```

## 🚀 技术栈

| 层 | 选型 |
|---|---|
| **平台** | Dify Chatflow |
| **生成模型** | qwen3.8-max（通义万相） |
| **重排序** | qwen3-rerank |
| **向量化** | multimodal-embedding-v1 |
| **检索** | 混合检索 hybrid_search (keyword 0.3 + vector 0.7) |
| **索引质量** | high_quality |
| **语音交互** | speech_to_text（已开启） |

## 📦 本地导入 Dify

```bash
# 1. 克隆仓库
git clone https://github.com/wqorange/xueshigongzhida-dify.git

# 2. 在 Dify 创建新应用（聊天助手 / Chatflow）

# 3. 将 dify_export/workflow.json 中的 workflow 对象导入工作流编排
#    （注意：dataset_ids 需要替换为你 Dify 实例中的实际知识库 UUID）

# 4. 打开知识检索节点，重新绑定知识库

# 5. 发布上线
```

## 📚 深度阅读

| 文档 | 内容 |
|---|---|
| [项目作品集 PDF](docs/学工智答_项目作品集_公开版.pdf) | 完整项目呈现（约 424KB） |
| [项目需求文档](docs/项目需求文档.md) | 背景痛点、功能清单、试点数据、实施规划 |
| [架构图](docs/架构图.md) | 5 张架构图（ASCII + Mermaid，GitHub 可直接渲染） |
| [工作流 DSL](dify_export/workflow.json) | 可直接导入 Dify 的完整 Chatflow 配置 |
| [知识库元数据](dify_export/knowledge_base.json) | 9 个知识库的名称、ID、文档数、字数 |

## 🛣️ 演进路线

- ✅ **MVP**（已实现）：5 节点 Chatflow、混合检索、查询优化、语音转文字
- 🔄 **V2 规划中**：Agent 工作流、动态知识图谱、智能填表、业务系统 API 对接、知识库扩充

## 📄 License

MIT © wqorange
