---
name: codex-strategy-research
description: 显式用于研究与探索，或接受已获授权的决策组研究委托，围绕项目问题完成实验设计、执行、分析与独立证据审查。安装、询问或维护不启动研究。
---

# 策略研究与探索

用户显式调用本 Skill，或研究流程按共同交接协议接受有效研究委托后启用。主会话担任研究负责人，恢复研究问题、已批准范围、数据用途、执行环境及剩余预算，按证据价值安排实验、分析与独立审查。同一研究轮次的后续交接沿用有效授权；准备授权不自动包含运行授权。

业务源码和 Git 保持只读，研究产物写入分配的仓库外目录。研究主会话维护所属研究 Issue／Projects，向实际签发组返回结论、证据、成本、限制与困难。具体权限、研究角色和完成条件由研究流程规定。

解析本 `SKILL.md` 的真实文件路径，以所在目录解析下列相对链接，完整读取[共同研究流程](../../research/research-workflow.md)，并读取目标仓库适用 `AGENTS.md` 与当前合同。

本 Skill 明确采用公共授权、证据、异常处理、资源与记录原则。通过非空 `CODEX_GOVERNANCE_CONFIG`，否则 `${XDG_CONFIG_HOME:-$HOME/.config}/codex-governance/local.toml`，只读定位绑定文件；仅以 `paths.source_root` 定位并读取 `governance/common-principles.md`。接收委托或跨组签发时读取同源 `governance/group-collaboration.md`；编写或修复获准的研究辅助代码时读取同源 `governance/engineering-principles.md`。平台访问按研究流程使用同源 `scripts/group_github.py --group research`，需要认证时才核验研究身份。

这些依赖只适用于本 Skill 的当前研究任务，不因被读取而约束其他 Skill 或无关任务，也不启动开发编排。缺失时报告具体缺口，不从个人全局规则或另一副本猜测来源。向子角色交接准确研究源及公共依赖源；同一任务继续沿用有效授权、累计预算和失败事实。

按当前问题和项目明确选用的内容读取：

- 交易项目合同及 A／B 入口：[Polymarket 项目参考](../../research/projects/polymarket.md)。
- 明确选用规则策略方法：[人工规则研究](../../research/rule-based-research.md)。
- 明确选用学习策略方法：[学习策略研究](../../research/learning-based-research.md)。
- 检索互联网、读取知识库或借鉴方法时：[研究资料与方法借鉴](../../research/evidence-sources.md)。
- 出现实现偏离或核心能力缺口时：[工程交接](../../research/engineering-handoff.md)。

需要工程能力时，按[工程交接](../../research/engineering-handoff.md)签发当前授权内的具体依赖，接回准确交付版本与证据，再判断研究结论和后续实验是否仍成立。
