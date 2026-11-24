# Changelog

本仓库遵循 [scriv](https://scriv.readthedocs.io/) fragment 工作流：新增条目放入 `changelog.d/`，
发版时合并进本文件（若维护者无此要求则直接追加）。

## [0.1.0] - 2026-11-24

### Added
- 首个金融任务评测 DataFlow Community Extension
- 算子：`FinanceQASampleEvaluator` / `FinanceSQLSampleEvaluator` / `FinanceHallucinationSampleEvaluator` / `EvalReportGenerator`
- Pipeline：`FinanceEvalPipeline`（QA / SQL / 幻觉评测 → 汇总报告）+ YAML 配置
- 评测口径：归一化 + 数值相对容差 + 含数字样本禁用模糊匹配
- 评测样本集：30 条人工金标（`eval/finance_eval_sample_set.jsonl`，模拟撰写数据）+ LLM 扩充脚本
- 数据卡模板与示例（`data_cards/`）
- 合规说明（`COMPLIANCE.md`）
- 离线单测与 CI（ubuntu/windows × py3.10-3.12）
