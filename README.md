# dataflow-finance-eval

> DataFlow Community Extension：金融任务评测套件。QA 准确率 / SQL 准确率 / 幻觉率三类确定性评测 + 汇总报告，端到端流水线 + 30 条金标样本集 + 数据卡与合规说明。
>
> 简体中文 ｜ [English](#english) ｜ License: Apache-2.0

## ✨ 功能

| 算子 | 类型 | 说明 |
|---|---|---|
| `FinanceQASampleEvaluator` | 评测 | 事实题归一化精确/模糊匹配，数值题按相对容差比较（自动提取数字与单位），无需 LLM |
| `FinanceSQLSampleEvaluator` | 评测 | 默认归一化文本匹配；安装 sqlglot 后按 AST 比较（与空白/别名无关），无需 LLM |
| `FinanceHallucinationSampleEvaluator` | 评测 | LLM 判定答案是否严格锚定原文（数字一致/引用真实/因果有据），输出幻觉标志与理由 |
| `EvalReportGenerator` | 汇总 | 聚合 QA 准确率、SQL 准确率与幻觉率，输出单行汇总报告 |

评测口径：归一化 + 数值相对容差 + 含数字样本禁用模糊匹配。

Pipeline：`FinanceEvalPipeline`，配套 YAML 配置。

## 🚀 快速开始

```bash
# 1. 安装主框架与扩展
pip install open-dataflow
pip install -e .

# SQL AST 评测建议安装
pip install -e ".[sql]"

# 2. 配置 LLM 服务（仅幻觉评测需要）
export DF_API_KEY=sk-xxx

# 3. 运行 Pipeline
python -c "
from dataflow.utils.storage import FileStorage
from dataflow.serving import APILLMServing_request
from dataflow_finance_eval import FinanceEvalPipeline

storage = FileStorage(first_entry_file_name='./input.jsonl')  # [{'question': ..., 'answer': ...}]
llm = APILLMServing_request(api_url='https://api.openai.com/v1/chat/completions')
FinanceEvalPipeline(llm_serving=llm, storage=storage).run()
"
```

## 📊 评测样本集

- `eval/finance_eval_sample_set.jsonl`：30 条人工金标样本（模拟撰写数据）
- `eval/expand_evalset.py`：LLM 扩充（**发布前须人工抽检 ≥20%**，见 COMPLIANCE.md）

## 🗂 数据卡与合规

- `data_cards/`：数据卡模板与示例（来源/口径/敏感性/质量五节）
- `COMPLIANCE.md`：法规映射、评测方法学边界（诚实披露：样本量、口径、适用范围）

## 🧪 测试

```bash
pip install -e ".[dev]" && pytest -v
```

离线测试：通过 stub 替身模拟 DataFlow 框架，无需安装 open-dataflow / sqlglot / 任何 LLM 服务，CI 可直接运行。

## 📖 相关资源

- 上游框架：[OpenDCAI/DataFlow](https://github.com/OpenDCAI/DataFlow)（SIGMOD'27）
- 生态指南：[DataFlow-Doc Ecosystem](https://opendcai.github.io/DataFlow-Doc/zh/guide/df_ecosystem/)
- 同系列扩展：[dataflow-finance-qa](https://github.com/DingNova/dataflow-finance-qa) · [dataflow-finance-text2sql](https://github.com/DingNova/dataflow-finance-text2sql) · [dataflow-finance-compliance](https://github.com/DingNova/dataflow-finance-compliance)

## <a id="english"></a>English

`dataflow-finance-eval` is a [DataFlow](https://github.com/OpenDCAI/DataFlow) community extension for evaluating financial LLM tasks. It ships 4 operators (`FinanceQASampleEvaluator`, `FinanceSQLSampleEvaluator`, `FinanceHallucinationSampleEvaluator`, `EvalReportGenerator`) with a deterministic evaluation protocol (normalization + numeric relative tolerance; fuzzy matching disabled for answers containing numbers), one end-to-end pipeline, a 30-sample golden set, data-card templates and a compliance guide with honest methodology disclosure. All operator descriptions support `get_desc(lang="zh"/"en")`.

---

⚠️ **Disclaimer**: for research and engineering purposes only. Not investment advice.
