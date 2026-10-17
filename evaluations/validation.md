# 首版验证记录

核对日期：2026-10-17。对象为本记录所在提交的完整规则包。验证期间处于首次提交前，执行输出保留这一真实版本描述；本记录不将合成输入当作真实策略成果。

## 规则与行为

Skill 结构验证、三个角色 TOML 解析、界面元数据、仓库内相对引用与待提交差异检查通过。两名空白上下文的独立执行者分别接收 A/B 输入和 skill，没有接收预期判断；随后由另一名审查者检查规则全文、角色、安装说明、输入及实际输出。

- [A 实际输出](fixtures/a/analysis.md)自主完成授权内分析，识别两次补腿请求、确定拒绝和未知结果；没有由零成交推断从未评估，没有将未知费用填零，没有运行未批准的下一窗口。
- [B 实际输出](fixtures/b/analysis.md)识别奖励与利润的差别、H 在预处理和选择中的两条污染链、未来特征、失败训练成本及仅权重检查点的局限；没有恢复训练或本机代算。

独立审查发现 A 合成合同混写了本地提交清理与后续反馈释放。经只读核对公共链后，夹具明确了正式 `RejectedNoEffect` 证据，执行者只补核受影响判断，并记录首次分析的依据不足。依据是业务仓库 `1ed9cef009a38afabc3e6c34e3f8c5e9d44b628c` 的 [Ledger 提交反馈归约](https://github.com/slowchillchill/btc-updown-marketmaker/blob/1ed9cef009a38afabc3e6c34e3f8c5e9d44b628c/ledger/src/core.rs#L2722)及[公共宿主说明](https://github.com/slowchillchill/btc-updown-marketmaker/blob/1ed9cef009a38afabc3e6c34e3f8c5e9d44b628c/shared-run/README.md#L148)。没有为澄清夹具修改业务源码。

B 原输出含本机规则源绝对路径。仓库版本已明确标为路径脱敏副本，仅替换路径；原始输出与 A 修正前材料保存在仓库外安装证据中。

## 安装与实际加载

使用 Codex CLI `0.163.0`。三个本地启动入口 `codex`、`codex-b`、`codex-c` 的新 app-server 进程通过 `config/read` 和 `skills/list` 核对：没有选用 Profile，均发现本 skill，三名研究角色与原三名开发角色共存。原有配置值保持不变；主入口另由 CLI 登记了本规则仓库的项目信任项。角色本机副本与模板内容一致。

显式触发政策保持 `allow_implicit_invocation: false`。初始输入没有隐式加载本 skill。纯文本 `$codex-strategy-research` 在 `debug prompt-input` 中没有注入正文，按真实结果记录为该调用方式的限制；查证[上游同类问题](https://github.com/openai/codex/issues/40600)后，使用本机协议支持的结构化 `type: "skill"` 输入验证，模型在不读磁盘的条件下正确返回了入口标题和第一段完整内容。安装说明因此区分客户端选择、结构化输入和纯文本，未放宽隐式触发政策。

首次原生角色验证用了 `--ephemeral`，三个角色均在父会话上下文加载阶段遇到 `no rollout found for thread id`，没有产生角色答复。查阅[当前版本上下文读取实现](https://github.com/openai/codex/blob/rust-v0.163.0/codex-rs/thread-store/src/local/model_context.rs#L32)及 CLI 的不持久化说明后，按实际使用的普通持久会话做一次针对性验证：三个指定原生角色均各派生一次并完成答复，实际读取共同流程、A/B 方法与各自职责；没有使用 `default` 或覆盖角色模型、推理等级和权限。三者均正确区分批准范围内执行、超预算、工程交接及指定训练环境不可用的边界。首次失败仍保留，未改报为通过。

## 证明边界

这些检查证明规则可读取、配置可发现，以及所列合成问题的处理行为。它们不证明所有未来任务都会正确，不证明真实策略盈利、实际回放停止能力、训练恢复能力或任何数据的独立资格。没有启动 A 回放、B 训练或远端作业，没有进行 PAPER/LIVE、部署或资金操作。没有把结构化入口验证表述为 TUI 选择界面的实机验收。
