# LangGraph 学习笔记

本项目记录 LangGraph 的学习过程，包含中文课程资料、Jupyter Notebook 实验和补充总结。内容覆盖状态与 Reducer、顺序与分支流程、循环、检查点与记忆、中断恢复、工具调用、流式输出、子图及图设计。

**教程来源：BiliBili [BV1z3NY66EY1](https://www.bilibili.com/video/BV1z3NY66EY1/)。** 本仓库在学习过程中补充了代码实验、问题分析和版本差异说明，建议配合原视频学习。

## 目录结构

```text
.
├── course/                  # 中文课程资料及配图
├── notebooks/               # 按章节组织的实验与学习笔记
│   ├── chapter01/           # 基础概念、状态和 Reducer
│   ├── chapter02/           # 顺序、分支、循环和异常处理
│   ├── chapter03/           # 持久化、故障恢复和记忆
│   ├── chapter04/           # 中断、工具调用和项目部署
│   └── chapter05/           # 流式执行、子图和图设计
├── hitl_demo/               # 独立的 LangGraph 本地服务示例
├── main.py                  # 初始化示例入口；主要学习内容在 notebooks/
├── pyproject.toml           # Python 依赖配置
└── uv.lock                  # 锁定的依赖版本
```

`data/` 用于本地生成的检查点数据库，不纳入版本控制。

## 环境安装

准备 Python 3.11+ 和 [uv](https://docs.astral.sh/uv/getting-started/installation/)，然后执行：

```bash
git clone https://github.com/tourist-skk/lang-graph.git
cd lang-graph
uv sync --locked

# 注册项目内核并启动 JupyterLab
uv run python -m ipykernel install --user --name langgraph-learn --display-name "Python (langgraph-learn)"
uv run jupyter lab
```

打开 Notebook 后选择 `Python (langgraph-learn)` 内核，按章节顺序执行单元格。纯状态与流程示例通常无需 API Key；涉及模型调用的示例需要先完成下方配置。

## API Key 配置

在仓库根目录创建 `.env`，填写自己的密钥和服务地址：

```dotenv
# ChatOpenAI / OpenAI 兼容接口
OPENAI_API_KEY=your_api_key
OPENAI_API_BASE=https://your-provider.example/v1

# 使用 DeepSeek 的示例按需配置
DEEPSEEK_API_KEY=your_deepseek_api_key
DEEPSEEK_BASE_URL=https://api.deepseek.com
```

将 `OPENAI_API_BASE` 替换为服务商提供的实际接口地址，并将代码中的 `model=...` 改为当前账户可用的模型。部分示例会读取上述多个变量，请结合初始化单元格配置。

部分 Notebook 的 `load_dotenv(...)` 仍使用本机绝对路径。运行前将相应加载语句替换为以下代码，即可从当前目录向上查找 `.env`；请从仓库目录或其子目录运行：

```python
from dotenv import find_dotenv, load_dotenv

load_dotenv(find_dotenv(usecwd=True), override=True)
```

`.env` 已被 `.gitignore` 排除。请勿将真实密钥写入代码、Markdown 或 Notebook 输出。

## 其他示例

- SQLite 示例需要可写的本地数据库路径；可先执行 `mkdir -p data`，并将代码中的绝对路径改为自己的路径。
- PostgreSQL 示例需另行准备数据库并配置连接信息。
- `hitl_demo/` 有独立的依赖和 `.env`，启动方式见其 [README](hitl_demo/README.md)。
