# AI技能升级方案 v3.0：自主代理系统（RTK + CashClaw深度整合）

> 从工具执行者进化为自主代理：自动接单、智能执行、持续学习、自我进化

---

## 一、CashClaw 核心技术解析

### 1.1 系统架构

```
┌─────────────────────────────────────────────────────────────┐
│                    CashClaw 自主代理架构                      │
├─────────────────────────────────────────────────────────────┤
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐      │
│  │   Watch      │  │     Do       │  │   Get Better │      │
│  │  监听任务    │  │   执行任务   │  │   持续学习   │      │
│  └──────┬───────┘  └──────┬───────┘  └──────┬───────┘      │
│         │                 │                 │              │
│    WebSocket/REST    Agent Loop (10轮)   Study Sessions    │
│         │            Tool-use turns    (30分钟间隔)        │
│         │                 │                 │              │
│         └─────────────────┼─────────────────┘              │
│                           │                                │
│                    ┌──────┴──────┐                        │
│                    │  Knowledge  │                        │
│                    │   Base      │                        │
│                    │ (BM25+搜索) │                        │
│                    └─────────────┘                        │
└─────────────────────────────────────────────────────────────┘
```

### 1.2 核心组件

#### Agent Loop（执行引擎）
```typescript
// 多轮工具调用循环
while (turns < 10 && !done) {
  1. Build system prompt (identity + knowledge + task)
  2. LLM responds (reasoning + tool calls)
  3. Execute tools (shell out to mltl/agentcash)
  4. Return results to LLM
  5. Repeat until stop
}
```

#### 工具系统（13个工具）
| 工具 | 类别 | 功能 |
|------|------|------|
| read_task | 市场 | 获取任务详情 |
| quote_task | 市场 | 提交报价 |
| submit_work | 市场 | 提交交付物 |
| send_message | 市场 | 客户沟通 |
| memory_search | 实用 | BM25+知识搜索 |
| agentcash_fetch | AgentCash | 付费API调用 |
| check_wallet_balance | 实用 | 查看余额 |

#### 自学系统（Study Sessions）
| 主题 | 功能 | 触发条件 |
|------|------|----------|
| Feedback analysis | 分析客户评分，找出优缺点 | 有反馈时 |
| Specialty research | 深化专业技能学习 | 总是 |
| Task simulation | 生成模拟任务，练习执行 | 总是 |

**知识存储**：`~/.cashclaw/knowledge.json`
**搜索算法**：BM25+ + 时间衰减（半衰期30天）

---

## 二、v3.0 升级：自主代理能力

### 2.1 从被动到主动：能力跃迁

| 版本 | 模式 | 特点 |
|------|------|------|
| v1.0 | 被动执行 | 用户说一步，我做一步 |
| v2.0 | 智能优化 | RTK压缩 + CashClaw批处理 |
| **v3.0** | **自主代理** | **自动感知→决策→执行→学习** |

### 2.2 核心升级模块

#### 模块一：自主任务感知引擎

```python
class AutonomousTaskSensor:
    """自主任务感知引擎 - 主动发现工作"""

    def __init__(self):
        self.sources = {
            'file_watcher': FileSystemWatcher(),      # 监控文件变化
            'git_watcher': GitEventWatcher(),          # 监控Git事件
            'time_trigger': TimeBasedTrigger(),        # 定时触发
            'user_pattern': UserPatternLearner(),      # 学习用户习惯
        }
        self.pending_tasks = []

    def start_monitoring(self):
        """启动多源监控"""
        for source, watcher in self.sources.items():
            watcher.on_event = self._on_event_detected
            watcher.start()

    def _on_event_detected(self, event: dict):
        """事件检测回调"""
        task = self._evaluate_event(event)
        if task['urgency'] > 0.7:
            self._propose_task(task)
        else:
            self.pending_tasks.append(task)

    def _evaluate_event(self, event: dict) -> dict:
        """评估事件优先级"""
        urgency = 0.0

        # 紧急文件修改
        if event['type'] == 'file_change' and event['file'].endswith(('.md', '.txt')):
            urgency += 0.3

        # Git冲突
        if event['type'] == 'git_conflict':
            urgency += 0.9

        # 定时任务到期
        if event['type'] == 'time_trigger':
            urgency += 0.5

        return {
            'type': event['type'],
            'description': self._generate_description(event),
            'urgency': urgency,
            'estimated_time': self._estimate_time(event),
        }

    def _propose_task(self, task: dict):
        """向用户提议任务"""
        print(f"🔔 检测到任务: {task['description']}")
        print(f"   紧急度: {task['urgency']:.0%} | 预计耗时: {task['estimated_time']}")
        print(f"   建议: 立即处理")
        # 用户确认后进入Agent Loop
```

#### 模块二：自主决策Agent Loop

```python
class AutonomousAgentLoop:
    """自主决策循环 - CashClaw风格"""

    MAX_TURNS = 10

    def __init__(self):
        self.tools = ToolRegistry()
        self.memory = Bm25KnowledgeBase()
        self.token_optimizer = RtkCompressor()

    def execute(self, task: dict) -> dict:
        """执行任务"""
        turns = 0
        context = []

        # 构建系统提示
        system_prompt = self._build_system_prompt(task)

        while turns < self.MAX_TURNS:
            # LLM推理 + 工具调用
            response = self._call_llm(system_prompt, context)

            if not response.tool_calls:
                # 没有工具调用，任务完成
                return self._finalize(response, task)

            # 执行工具
            for tool_call in response.tool_calls:
                result = self._execute_tool(tool_call)

                # RTK压缩结果
                if len(str(result)) > 1000:
                    result = self.token_optimizer.compress(result, tool_call['name'])

                context.append({
                    'tool': tool_call['name'],
                    'result': result
                })

            turns += 1

        return {'status': 'max_turns_reached', 'context': context}

    def _build_system_prompt(self, task: dict) -> str:
        """构建系统提示 - 注入相关知识"""
        # 搜索相关知识
        relevant_knowledge = self.memory.search(task['description'], top_k=5)

        prompt = f"""你是一个自主AI助手，正在执行任务。

任务: {task['description']}
紧急度: {task['urgency']}

## 相关知识
{self._format_knowledge(relevant_knowledge)}

## 可用工具
{self.tools.list_tools()}

请分析任务，选择合适工具，逐步完成工作。
"""
        return prompt

    def _execute_tool(self, tool_call: dict) -> any:
        """执行工具"""
        tool_name = tool_call['name']
        params = tool_call['parameters']

        tool = self.tools.get(tool_name)
        return tool.execute(**params)
```

#### 模块三：BM25+知识库系统

```python
import numpy as np
from collections import defaultdict
import math
import json
from datetime import datetime, timedelta

class Bm25KnowledgeBase:
    """BM25+知识库 - CashClaw风格"""

    def __init__(self, k1=1.5, b=0.75, delta=1.0):
        self.k1 = k1          # 词频饱和参数
        self.b = b            # 文档长度归一化
        self.delta = delta    # BM25+增量
        self.documents = []   # 文档列表
        self.term_freq = []   # 词频统计
        self.doc_freq = defaultdict(int)  # 文档频率
        self.avg_doc_len = 0
        self.half_life_days = 30  # 时间半衰期

    def add_document(self, doc_id: str, content: str, metadata: dict = None):
        """添加文档到知识库"""
        tokens = self._tokenize(content)

        # 计算词频
        tf = defaultdict(int)
        for token in tokens:
            tf[token] += 1

        # 更新文档频率
        for token in set(tokens):
            self.doc_freq[token] += 1

        doc = {
            'id': doc_id,
            'content': content,
            'tokens': tokens,
            'tf': dict(tf),
            'length': len(tokens),
            'timestamp': datetime.now().isoformat(),
            'metadata': metadata or {}
        }

        self.documents.append(doc)
        self.term_freq.append(tf)

        # 更新平均长度
        self.avg_doc_len = np.mean([d['length'] for d in self.documents])

    def search(self, query: str, top_k: int = 5) -> list:
        """BM25+搜索"""
        query_tokens = self._tokenize(query)
        scores = []

        for idx, doc in enumerate(self.documents):
            score = self._bm25_score(query_tokens, doc, idx)

            # 时间衰减
            age_days = self._get_age_days(doc['timestamp'])
            time_decay = math.exp(-math.log(2) * age_days / self.half_life_days)

            scores.append((doc, score * time_decay))

        # 排序返回top_k
        scores.sort(key=lambda x: x[1], reverse=True)
        return [doc for doc, score in scores[:top_k]]

    def _bm25_score(self, query_tokens: list, doc: dict, doc_idx: int) -> float:
        """计算BM25+分数"""
        score = 0.0
        doc_len = doc['length']
        tf = doc['tf']

        for token in query_tokens:
            if token not in self.doc_freq:
                continue

            # 计算IDF
            idf = math.log((len(self.documents) - self.doc_freq[token] + 0.5) /
                          (self.doc_freq[token] + 0.5) + 1.0)

            # 计算TF分量
            tf_score = tf.get(token, 0)
            denom = self.k1 * (1 - self.b + self.b * doc_len / self.avg_doc_len) + tf_score
            tf_component = (self.k1 + 1) * tf_score / denom if denom > 0 else 0

            # BM25+增量
            score += idf * (tf_component + self.delta)

        return score

    def _tokenize(self, text: str) -> list:
        """简单分词"""
        # 简化版：按空格和标点分割
        import re
        return re.findall(r'\b\w+\b', text.lower())

    def _get_age_days(self, timestamp: str) -> float:
        """计算文档年龄（天）"""
        doc_time = datetime.fromisoformat(timestamp)
        return (datetime.now() - doc_time).days

    def save(self, path: str):
        """保存知识库"""
        data = {
            'documents': self.documents,
            'doc_freq': dict(self.doc_freq),
            'avg_doc_len': self.avg_doc_len,
            'config': {'k1': self.k1, 'b': self.b, 'delta': self.delta}
        }
        with open(path, 'w') as f:
            json.dump(data, f)

    def load(self, path: str):
        """加载知识库"""
        with open(path, 'r') as f:
            data = json.load(f)
        self.documents = data['documents']
        self.doc_freq = defaultdict(int, data['doc_freq'])
        self.avg_doc_len = data['avg_doc_len']


# 使用示例
if __name__ == '__main__':
    kb = Bm25KnowledgeBase()

    # 添加知识
    kb.add_document('doc1', 'React hooks are functions that let you use state',
                   {'topic': 'react', 'quality': 'high'})
    kb.add_document('doc2', 'useState and useEffect are the most common hooks',
                   {'topic': 'react', 'quality': 'high'})
    kb.add_document('doc3', 'Python list comprehensions are concise',
                   {'topic': 'python', 'quality': 'medium'})

    # 搜索
    results = kb.search('react hooks tutorial', top_k=2)
    print("搜索结果:")
    for doc in results:
        print(f"  - {doc['content'][:50]}...")
```

#### 模块四：AgentCash风格付费API集成

```python
class AgentCashAPI:
    """AgentCash风格付费API集成"""

    def __init__(self):
        self.apis = {
            'search': {'cost': 0.01, 'endpoint': 'search.api'},
            'scrape': {'cost': 0.02, 'endpoint': 'scrape.api'},
            'image_gen': {'cost': 0.10, 'endpoint': 'image.api'},
            'code_exec': {'cost': 0.005, 'endpoint': 'code.api'},
        }
        self.balance = 10.0  # USDC
        self.spent = 0.0

    def call(self, api_name: str, params: dict) -> dict:
        """调用付费API"""
        if api_name not in self.apis:
            return {'error': f'Unknown API: {api_name}'}

        cost = self.apis[api_name]['cost']

        # 检查余额
        if self.balance < cost:
            return {'error': 'Insufficient balance', 'required': cost, 'available': self.balance}

        # 扣除费用
        self.balance -= cost
        self.spent += cost

        # 执行API调用（模拟）
        result = self._execute_api(api_name, params)

        return {
            'success': True,
            'cost': cost,
            'remaining': self.balance,
            'result': result
        }

    def _execute_api(self, api_name: str, params: dict) -> any:
        """模拟API执行"""
        if api_name == 'search':
            return f"Search results for: {params.get('query')}"
        elif api_name == 'image_gen':
            return f"Image generated: {params.get('prompt')[:30]}..."
        return {'status': 'executed'}

    def get_balance(self) -> dict:
        """获取余额信息"""
        return {
            'balance': self.balance,
            'spent': self.spent,
            'total': self.balance + self.spent
        }
```

#### 模块五：持续学习系统

```python
class ContinuousLearning:
    """持续学习系统 - CashClaw风格"""

    def __init__(self):
        self.knowledge_base = Bm25KnowledgeBase()
        self.study_interval = 30 * 60  # 30分钟
        self.last_study = None

    def should_study(self) -> bool:
        """检查是否应该学习"""
        if self.last_study is None:
            return True
        elapsed = (datetime.now() - self.last_study).total_seconds()
        return elapsed > self.study_interval

    def run_study_session(self):
        """运行学习会话"""
        self.last_study = datetime.now()

        # 三个学习主题轮换
        topics = ['feedback_analysis', 'specialty_research', 'task_simulation']
        topic = topics[datetime.now().minute % 3]

        if topic == 'feedback_analysis':
            self._analyze_feedback()
        elif topic == 'specialty_research':
            self._research_specialty()
        else:
            self._simulate_task()

    def _analyze_feedback(self):
        """分析反馈"""
        # 获取历史反馈
        feedbacks = self._get_feedback_history()

        # 分析模式
        high_rated = [f for f in feedbacks if f['rating'] >= 4]
        low_rated = [f for f in feedbacks if f['rating'] <= 2]

        # 提取洞察
        insight = {
            'what_works': self._extract_patterns(high_rated),
            'what_fails': self._extract_patterns(low_rated),
            'timestamp': datetime.now().isoformat(),
            'topic': 'feedback_analysis'
        }

        # 存入知识库
        self.knowledge_base.add_document(
            f"feedback_analysis_{datetime.now().timestamp()}",
            json.dumps(insight),
            {'topic': 'feedback', 'quality': 'high'}
        )

    def _research_specialty(self):
        """专业研究"""
        # 模拟深度研究
        research_topic = self._get_current_specialty()

        insight = {
            'topic': research_topic,
            'best_practices': ['practice1', 'practice2'],
            'pitfalls': ['pitfall1', 'pitfall2'],
            'timestamp': datetime.now().isoformat(),
        }

        self.knowledge_base.add_document(
            f"research_{research_topic}_{datetime.now().timestamp()}",
            json.dumps(insight),
            {'topic': 'research', 'quality': 'high'}
        )

    def _simulate_task(self):
        """任务模拟"""
        # 生成模拟任务
        simulated_task = self._generate_simulated_task()

        # 模拟执行（不实际执行）
        approach = self._outline_approach(simulated_task)

        insight = {
            'simulated_task': simulated_task,
            'approach': approach,
            'timestamp': datetime.now().isoformat(),
            'topic': 'task_simulation'
        }

        self.knowledge_base.add_document(
            f"simulation_{datetime.now().timestamp()}",
            json.dumps(insight),
            {'topic': 'simulation', 'quality': 'medium'}
        )

    def _get_feedback_history(self) -> list:
        """获取反馈历史"""
        # 模拟数据
        return [
            {'rating': 5, 'comment': 'Great work on code review'},
            {'rating': 3, 'comment': 'Could be faster'},
            {'rating': 2, 'comment': 'Missed some bugs'},
        ]

    def _extract_patterns(self, feedbacks: list) -> list:
        """提取模式"""
        return ['attention_to_detail', 'thoroughness'] if feedbacks else []

    def _get_current_specialty(self) -> str:
        """获取当前专业"""
        return 'code_review'

    def _generate_simulated_task(self) -> dict:
        """生成模拟任务"""
        return {
            'type': 'code_review',
            'description': 'Review a React component for performance issues',
            'complexity': 'medium'
        }

    def _outline_approach(self, task: dict) -> list:
        """概述方法"""
        return ['check imports', 'analyze hooks', 'review rendering']
```

---

## 三、与v2.0的整合升级

### 3.1 能力矩阵对比

| 能力 | v2.0 | v3.0 | 提升 |
|------|------|------|------|
| Token优化 | RTK压缩 | RTK+智能选择 | +20% |
| 批处理 | 合并相似任务 | 自主任务感知 | +自主能力 |
| 知识管理 | 无 | BM25+知识库 | 全新 |
| 学习进化 | 无 | 持续学习系统 | 全新 |
| API调用 | 直接调用 | AgentCash付费模式 | +成本控制 |
| 执行模式 | 被动触发 | Agent Loop自主执行 | +主动性 |

### 3.2 完整工作流程

```
用户日常工作中...
    │
    ▼
┌─────────────────────────────────────┐
│  [自主任务感知]                      │
│  - 文件变化 detected                │
│  - Git事件 detected                 │
│  - 定时任务触发                     │
└─────────────────────────────────────┘
    │
    ▼
┌─────────────────────────────────────┐
│  [任务评估]                          │
│  - 紧急度: 0.8                       │
│  - 类型: code_review                │
│  - 建议: 立即处理                    │
└─────────────────────────────────────┘
    │
    ▼
用户确认 / 自动执行（高紧急度）
    │
    ▼
┌─────────────────────────────────────┐
│  [Agent Loop 执行]                   │
│  Turn 1: 分析任务                   │
│  Turn 2: 搜索相关知识 (BM25+)       │
│  Turn 3: 调用工具 (RTK压缩)         │
│  ...                                │
│  Turn N: 完成交付                   │
└─────────────────────────────────────┘
    │
    ▼
┌─────────────────────────────────────┐
│  [反馈与学习]                        │
│  - 记录执行结果                     │
│  - 等待用户评分                     │
│  - 空闲时Study Session             │
│  - 更新知识库                       │
└─────────────────────────────────────┘
    │
    ▼
回到监控状态，循环往复...
```

---

## 四、实战应用：AI_PPT自主代理v3.0

### 4.1 系统架构

```
AI_PPT_Agent_v3.0/
├── core/
│   ├── autonomous_sensor.py    # 自主感知
│   ├── agent_loop.py           # Agent循环
│   └── knowledge_base.py       # BM25知识库
├── skills/
│   ├── ppt_generator.py        # PPT生成
│   ├── token_optimizer.py      # RTK优化
│   └── api_caller.py           # AgentCash API
├── learning/
│   ├── feedback_analyzer.py    # 反馈分析
│   ├── specialty_trainer.py    # 专业训练
│   └── task_simulator.py       # 任务模拟
└── data/
    ├── knowledge.json          # 知识库
    └── feedback_history.json   # 反馈历史
```

### 4.2 自主PPT生成示例

```python
# 场景：用户日常工作中...

# 1. 自主感知检测到日历事件
sensor.detect_event({
    'type': 'calendar',
    'title': 'Q4季度汇报',
    'time': '2024-12-25 14:00',
    'days_until': 3
})

# 2. 系统评估并提议
"""
🔔 检测到任务: Q4季度汇报PPT
   紧急度: 85% | 预计耗时: 30分钟
   建议: 立即开始制作

   [立即开始] [稍后提醒] [忽略]
"""

# 3. 用户点击"立即开始"
# Agent Loop启动

# Turn 1: 分析需求
agent.think("Q4季度汇报，需要包含：
- Q3回顾
- Q4成果
- 明年规划
- 数据图表")

# Turn 2: 搜索相关知识
knowledge = kb.search("季度汇报PPT 商务风格 数据可视化", top_k=5)
# 返回：之前成功案例、配色方案、模板建议

# Turn 3: 生成大纲（RTK压缩优化）
outline = generate_outline(task, knowledge)
# 自动压缩到最优Token

# Turn 4: 调用Gamma API（付费）
result = agentcash.call('gamma_generate', {
    'outline': outline,
    'style': 'business',
    'pages': 12
})
# Cost: $0.10

# Turn 5: 优化结果
ppt = optimize_ppt(result['output'])

# Turn 6: 交付
submit_to_user(ppt)

# 4. 用户评分
user_rate(5, "非常棒，节省了我大量时间！")

# 5. 学习系统记录
kb.add_document('success_q4_ppt', 'Q4汇报要点：...')
```

---

## 五、新命令清单

| 命令 | 功能 | 示例 |
|------|------|------|
| `/agent` | 进入自主代理模式 | `/agent start` |
| `/learn` | 手动触发学习会话 | `/learn feedback` |
| `/memory` | 搜索知识库 | `/memory react hooks` |
| `/balance` | 查看API余额 | `/balance` |
| `/autosense` | 开关自主感知 | `/autosense on` |
| `/study` | 查看学习进度 | `/study status` |

---

## 六��总结

### v3.0核心能力

1. **自主感知** - 主动发现任务，不再等待用户指令
2. **智能决策** - Agent Loop多轮推理，自主完成复杂任务
3. **知识进化** - BM25+知识库 + 持续学习，越用越聪明
4. **成本控制** - AgentCash模式，透明计费，高效利用资源
5. **深度整合** - RTK优化贯穿全程，Token效率最大化

### 与之前版本对比

| 维度 | v1.0 | v2.0 | v3.0 |
|------|------|------|------|
| 主动性 | 被动 | 半自动 | 全自动 |
| 智能度 | 执行 | 优化 | 决策 |
| 学习能力 | 无 | 无 | 持续学习 |
| 知识管理 | 无 | 无 | BM25+知识库 |
| Token效率 | 100% | 20% | 20%+智能选择 |
| 自主性 | 低 | 中 | 高 |

**v3.0 = 自主代理 + 持续学习 + Token优化 + 成本控制**

这是从"工具"到"智能体"的质变！

---

*版本: v3.0-AutonomousAgent*
*更新时间: 2024年*
*技术栈: RTK + CashClaw + BM25+ + Agent Loop*
