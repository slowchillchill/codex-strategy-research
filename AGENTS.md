# 仓库维护规则

本仓库维护 Polymarket 策略研究的 skill、方法、角色模板与安装说明。维护这些文件不等于启动研究，也不授权回放、训练、PAPER、LIVE 或资金操作。

普通维护适用个人工程原则：通过非空 `CODEX_GOVERNANCE_CONFIG`，否则 `${XDG_CONFIG_HOME:-$HOME/.config}/codex-governance/local.toml`，只读定位绑定文件；仅以 `paths.source_root` 定位并完整读取 `governance/engineering-principles.md`。缺失时报告真实缺口，不猜测路径。读取原则不启用 GitHub 开发治理。本仓库只有用户明确调用 `$codex-stage-orchestrator` 并交付开发任务时才启用该治理。

- 研究共同规则只维护在 `research/research-workflow.md`；A/B 方法、资料使用与工程交接分别维护。入口和角色引用正文，不复制完整规则。
- 交易项目的业务合同、数据用途、实验配置与结果继续归交易项目的研究 Issues 及仓库外证据；这里不保存运行状态、数据集、模型、钱包或认证材料。
- 本机账号、安装路径、模型选择和配置副本留在仓库外。示例使用占位符，不提交真实绑定。
- 规则变化先核对用户目标与真实失败条件。重要行为变化使用独立内容审查与隔离案例验证；不为文字格式增加运行时约束。
- 文件所有权明确，审查期间冻结候选；不把研究角色的只读职责宣称为操作系统隔离。
- 正文使用中文，精确标识保持原样。每次回复以“亲爱的chill”开头。
