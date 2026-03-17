# Claude AI Skills v3.0

[![Version](https://img.shields.io/badge/version-3.0-blue.svg)](https://github.com/huamu668/claude-ai-skills)
[![Security](https://img.shields.io/badge/security-hardened-green.svg)](./AI技能升级方案_v3.0_安全增强版.md)
[![License](https://img.shields.io/badge/license-MIT-yellow.svg)](LICENSE)

> 🤖 从工具执行者进化为自主代理：自动接单、智能执行、持续学习、自我进化

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

## 核心特性

### 🎯 自主任务感知引擎
- 文件系统监控
- Git事件监听
- 定时任务触发
- 用户习惯学习

### ⚙️ Agent Loop 执行引擎
- 多轮推理决策（最多10轮）
- 工具自动调用
- 结果智能压缩（RTK技术）
- 自主任务完成

### 🧠 BM25+ 知识库系统
- 基于BM25+算法的高效检索
- 时间衰减机制（半衰期30天）
- 自动知识注入
- 持续学习积累

### 💰 AgentCash API集成
- 付费API统一管理
- USDC余额追踪
- 成本透明计费
- 多种API支持（搜索、图像生成、代码执行等）

### 📚 持续学习系统
- 反馈分析
- 专业研究
- 任务模拟
- 30分钟自动学习周期

## 版本对比

| 维度 | v1.0 | v2.0 | v3.0 |
|------|------|------|------|
| **主动性** | 被动等待 | 半自动 | **全自动感知** |
| **智能度** | 执行命令 | 优化输出 | **自主决策** |
| **学习能力** | 无 | 无 | **持续进化** |
| **知识管理** | 无 | 无 | **BM25+知识库** |
| **Token效率** | 100% | 节省80% | **节省80%+智能选择** |
| **自主性** | 低 | 中 | **高** |

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

| 文件 | 说明 |
|------|------|
| [AI技能升级方案_v3.0_AutonomousAgent.md](./AI技能升级方案_v3.0_AutonomousAgent.md) | 完整技术文档，包含所有模块的详细实现 |
| [AI技能升级方案_v3.0_安全增强版.md](./AI技能升级方案_v3.0_安全增强版.md) | 安全增强版本，修复了所有已知安全问题 |

### 文档对比

- **标准版**：完整的功能实现，适合学习和理解系统架构
- **安全增强版**：在标准版基础上增加了全面的安全防护，推荐生产环境使用

## 技术栈

- **RTK**: Token优化和压缩
- **CashClaw**: 自主代理架构
- **BM25+**: 知识检索算法
- **Python 3.8+**: 核心实现语言
- **NumPy**: 数值计算

## 贡献指南

欢迎提交Issue和PR！

1. Fork 本仓库
2. 创建你的特性分支 (`git checkout -b feature/AmazingFeature`)
3. 提交你的更改 (`git commit -m 'Add some AmazingFeature'`)
4. 推送到分支 (`git push origin feature/AmazingFeature`)
5. 打开一个 Pull Request

## 许可证

本项目采用 MIT 许可证 - 详见 [LICENSE](LICENSE) 文件

## 更新日志

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

**版本**: v3.0-AutonomousAgent
