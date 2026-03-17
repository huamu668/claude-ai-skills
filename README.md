# Claude AI Skills v4.0

[![Version](https://img.shields.io/badge/version-4.0-blue.svg)](https://github.com/huamu668/claude-ai-skills)
[![Multi-Agent](https://img.shields.io/badge/multi--agent-teams-orange.svg)](./AI技能升级方案_v4.0_MultiModalAgentTeams.md)
[![Security](https://img.shields.io/badge/security-hardened-green.svg)](./AI技能升级方案_v3.0_安全增强版.md)
[![License](https://img.shields.io/badge/license-MIT-yellow.svg)](LICENSE)

> 🤖 从单一代理到专业团队：多模态感知、MCP工具生态、Agent团队编排
>
> 🌟 灵感来源：[awesome-llm-apps](https://github.com/Shubhamsaboo/awesome-llm-apps) + RTK + CashClaw

## 📋 目录

- [简介](#简介)
- [核心特性](#核心特性)
- [版本对比](#版本对比)
- [快速开始](#快速开始)
- [安全特性](#安全特性)
- [架构设计](#架构设计)
- [文档说明](#文档说明)

## 简介

Claude AI Skills v3.0 是一个基于 RTK + CashClaw 深度整合的自主代理系统。它实现了从被动执行到主动感知的跃迁，具备完整的任务感知、自主决策、持续学习和知识管理能力。

### 核心理念

```
Watch (监听) → Do (执行) → Get Better (进化)
     ↑                              ↓
     └──────── 知识循环 ←───────────┘
```

## 版本演进

```
v1.0 (基础工具) → v2.0 (RTK优化) → v3.0 (自主代理) → v4.0 (Agent团队)
     被动执行          智能压缩          自主感知          团队协作
```

## 核心特性

### 🎭 v4.0 新增特性

#### 👥 Agent团队编排
- 多Agent协作系统
- 任务自动分解与分配
- Agent间冲突解决
- 质量监控与评估

#### 🔧 MCP工具生态
- 支持MCP (Model Context Protocol)
- Browser自动化
- GitHub/Notion集成
- 可扩展工具注册

#### 🎙️ 多模态感知
- 语音识别与合成
- 图像理解与分析
- OCR文字提取
- 多模态融合

#### 🧠 持久化记忆图谱
- 基于图的知识表示
- 经验关联推理
- 自动学习积累
- 情境化回忆

### v3.0 核心特性

#### 🎯 自主任务感知引擎
- 文件系统监控
- Git事件监听
- 定时任务触发
- 用户习惯学习

#### ⚙️ Agent Loop 执行引擎
- 多轮推理决策（最多10轮）
- 工具自动调用
- 结果智能压缩（RTK技术）
- 自主任务完成

#### 🧠 BM25+ 知识库系统
- 基于BM25+算法的高效检索
- 时间衰减机制（半衰期30天）
- 自动知识注入
- 持续学习积累

#### 💰 AgentCash API集成
- 付费API统一管理
- USDC余额追踪
- 成本透明计费
- 多种API支持（搜索、图像生成、代码执行等）

#### 📚 持续学习系统
- 反馈分析
- 专业研究
- 任务模拟
- 30分钟自动学习周期

## 版本对比

| 维度 | v1.0 | v2.0 | v3.0 | **v4.0** |
|------|------|------|------|----------|
| **主动性** | 被动等待 | 半自动 | 全自动感知 | **Agent团队** |
| **智能度** | 执行命令 | 优化输出 | 自主决策 | **多模态理解** |
| **学习能力** | 无 | 无 | 持续进化 | **知识图谱** |
| **知识管理** | 无 | 无 | BM25+知识库 | **图结构记忆** |
| **Token效率** | 100% | 节省80% | 节省80%+智能选择 | **MCP优化** |
| **工具生态** | 无 | 固定工具 | AgentCash API | **MCP协议** |
| **自主性** | 低 | 中 | 高 | **团队协作** |

## 快速开始

### 环境要求

- Python 3.8+
- pip 包管理器
- 2GB 可用磁盘空间

### 安装

```bash
# 克隆仓库
git clone https://github.com/huamu668/claude-ai-skills.git
cd claude-ai-skills

# 安装依赖
pip install -r requirements.txt

# 配置环境变量
cp .env.example .env
# 编辑 .env 文件，填入你的API密钥
```

### 基础使用

```python
from secure_knowledge_base import SecureBm25KnowledgeBase
from secure_agentcash_api import SecureAgentCashAPI
from secure_continuous_learning import SecureContinuousLearning

# 创建知识库
kb = SecureBm25KnowledgeBase()

# 添加知识
kb.add_document(
    'doc1',
    'React hooks are functions that let you use state',
    {'topic': 'react', 'quality': 'high'}
)

# 搜索知识
results = kb.search('react hooks tutorial', top_k=2)
```

## 安全特性

### 🔒 安全增强

本版本已进行全面的安全加固：

| 安全项 | 描述 |
|--------|------|
| **路径遍历防护** | 限制文件操作在指定目录内 |
| **输入验证** | 所有公共方法参数校验 |
| **异常处理** | 全面的错误处理和日志 |
| **资源限制** | 文件大小和数量限制 |
| **XSS防护** | 输入内容自动转义 |
| **速率限制** | API调用频率控制 |
| **审计日志** | 所有操作可追溯 |

### 安全最佳实践

```bash
# 设置数据目录权限
chmod 700 ~/.claude-agent

# 配置环境变量（不提交到版本控制）
export SEARCH_API_KEY=your_key_here
export AGENTCASH_BALANCE=10.0
```

## 架构设计

### v4.0 三层架构

```
┌─────────────────────────────────────────────────────────────────┐
│                     v4.0 Multi-Modal Agent System               │
├─────────────────────────────────────────────────────────────────┤
│  Layer 3: 🎯 Orchestration Layer (编排层)                        │
│  ┌─────────────────────────────────────────────────────────┐   │
│  │  Agent Orchestrator (团队指挥官)                         │   │
│  │  - 任务分解与分配                                        │   │
│  │  - 冲突解决与协调                                        │   │
│  │  - 质量监控与评估                                        │   │
│  └─────────────────────────────────────────────────────────┘   │
├─────────────────────────────────────────────────────────────────┤
│  Layer 2: 👥 Agent Team Layer (团队层)                          │
│  ┌────────────┐ ┌────────────┐ ┌────────────┐ ┌────────────┐  │
│  │  🎨        │ │  🔒        │ │  📊        │ │  🔬        │  │
│  │  Creative  │ │  Security  │ │  Research  │ │  Analysis  │  │
│  │  Agent     │ │  Agent     │ │  Agent     │ │  Agent     │  │
│  └────────────┘ └────────────┘ └────────────┘ └────────────┘  │
│  ┌────────────┐ ┌────────────┐ ┌────────────┐ ┌────────────┐  │
│  │  🎙️        │ │  🌐        │ │  📝        │ │  ⚡        │  │
│  │  Voice     │ │  Web       │ │  Document  │ │  Code      │  │
│  │  Agent     │ │  Agent     │ │  Agent     │ │  Agent     │  │
│  └────────────┘ └────────────┘ └────────────┘ └────────────┘  │
├─────────────────────────────────────────────────────────────────┤
│  Layer 1: 🔧 Foundation Layer (基础层)                          │
│  ┌──────────────┐ ┌──────────────┐ ┌──────────────┐            │
│  │  MCP Hub     │ │  Memory      │ │  RAG Engine  │            │
│  │  工具生态     │ │  Graph       │ │  知识检索    │            │
│  └──────────────┘ └──────────────┘ └──────────────┘            │
└─────────────────────────────────────────────────────────────────┘
```

### v3.0 架构

```
┌─────────────────────────────────────────────────────────────┐
│                    Claude AI Skills v3.0                    │
├─────────────────────────────────────────────────────────────┤
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐      │
│  │   Watch      │  │     Do       │  │   Get Better │      │
│  │  自主感知    │  │   Agent Loop │  │   持续学习   │      │
│  └──────┬───────┘  └──────┬───────┘  └──────┬───────┘      │
│         │                 │                 │              │
│    ┌────┴─────────────────┴─────────────────┴────┐        │
│    │           Secure BM25+ Knowledge Base        │        │
│    │         (安全增强的知识库系统)               │        │
│    └──────────────────────────────────────────────┘        │
│                                                             │
│  ┌─────────────────────────────────────────────────────┐  │
│  │              Secure AgentCash API                    │  │
│  │         (安全的付费API集成层)                       │  │
│  └─────────────────────────────────────────────────────┘  │
└─────────────────────────────────────────────────────────────┘
```

## 文档说明

### 主要文档

| 文件 | 说明 | 版本 |
|------|------|------|
| [AI技能升级方案_v4.0_MultiModalAgentTeams.md](./AI技能升级方案_v4.0_MultiModalAgentTeams.md) | Agent团队、多模态、MCP生态 | **v4.0 (最新)** |
| [AI技能升级方案_v3.0_AutonomousAgent.md](./AI技能升级方案_v3.0_AutonomousAgent.md) | 完整技术文档，包含所有模块的详细实现 | v3.0 |
| [AI技能升级方案_v3.0_安全增强版.md](./AI技能升级方案_v3.0_安全增强版.md) | 安全增强版本，修复了所有已知安全问题 | v3.0-secure |

### 版本特性对比

- **v4.0 (Multi-Agent Teams)**：从单一代理升级到专业团队协作，支持多模态感知和MCP工具生态
- **v3.0 标准版**：完整的功能实现，适合学习和理解系统架构
- **v3.0 安全增强版**：在标准版基础上增加了全面的安全防护，推荐生产环境使用

### 技术整合来源

| 技术 | 来源 | 整合方式 |
|------|------|----------|
| Multi-Agent Teams | awesome-llm-apps | Agent团队编排器 |
| MCP Protocol | awesome-llm-apps | MCP工具中心 |
| Voice AI | awesome-llm-apps | 多模态感知系统 |
| RAG with Memory | awesome-llm-apps | 持久化记忆图谱 |
| RTK Token优化 | 原v2.0 | 贯穿各层 |
| CashClaw架构 | 原v3.0 | 基础Agent架构 |

## 技术栈

### v4.0 新增技术
- **Multi-Agent Orchestration**: Agent团队编排
- **MCP Protocol**: Model Context Protocol工具生态
- **Multimodal LLM**: GPT-4V, Gemini Pro Vision多模态理解
- **Knowledge Graph**: NetworkX持久化记忆图谱
- **Voice AI**: Whisper, TTS语音处理

### v3.0 核心技术
- **RTK**: Token优化和压缩
- **CashClaw**: 自主代理架构
- **BM25+**: 知识检索算法
- **Python 3.8+**: 核心实现语言
- **NumPy**: 数值计算

## 灵感来源与致谢

本项目的v4.0版本深受 [awesome-llm-apps](https://github.com/Shubhamsaboo/awesome-llm-apps) 启发，整合了以下优秀实践：

- Multi-Agent Teams协作模式
- MCP (Model Context Protocol) 工具协议
- Voice AI Agents架构
- Agentic RAG实现
- Chat with X数据连接模式

## 更新日志

### v4.0 (2024) - 最新版本
- ✨ Agent团队编排系统
- ✨ 多模态感知（语音+图像+文本）
- ✨ MCP工具生态系统
- ✨ 持久化记忆图谱
- ✨ Agentic RAG与知识图谱
- 🔗 整合 awesome-llm-apps 最佳实践

### v3.0 (2024)
- ✨ 自主任务感知引擎
- ✨ Agent Loop 执行引擎
- ✨ BM25+ 知识库系统
- ✨ AgentCash API集成
- ✨ 持续学习系统
- 🔒 全面的安全加固

---

**作者**: huamu668

**GitHub**: [https://github.com/huamu668/claude-ai-skills](https://github.com/huamu668/claude-ai-skills)

**版本**: v4.0-MultiModalAgentTeams
