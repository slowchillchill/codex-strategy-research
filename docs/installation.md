# 安装与原生角色配置

本仓库保存研究规则、Skill 和原生角色示例。安装后使用普通 `codex -C` 启动，再显式调用研究 Skill；不要求 Profile。本文所述接入方式已在 Codex CLI `0.163.0` 核对，该版本不是最低版本要求。使用其他版本时，按其实际配置能力核对角色注册和 Skill 发现。

## 保留完整规则源

先取得私有仓库的访问权限，再使用已有 Git 认证克隆；不要把令牌或认证文件放入本仓库。

```bash
git clone https://github.com/slowchillchill/codex-strategy-research.git
cd codex-strategy-research
research_source_dir="$(pwd -P)"
research_codex_dir="${CODEX_HOME:-$HOME/.codex}"
```

保留完整源码目录。`SKILL.md` 通过 `../../research/` 引用研究正文，不能只复制入口文件。无需新增本机绑定文件；主会话从实际 Skill 来源定位研究源，并向子角色交接真实目录、规则版本及准确任务。研究规则源、目标业务源码和运行产物各自保留真实身份，不把规则提交当作策略程序版本。

## 接入 Skill

先检查 `~/.agents/skills/codex-strategy-research` 是否存在，包括失效符号链接。已有入口指向当前源时直接复用；指向其他位置时先核对来源，再定向调整，不覆盖其他 Skill。仅在入口不存在时执行：

```bash
mkdir -p "$HOME/.agents/skills"
ln -s "$research_source_dir/skills/codex-strategy-research" "$HOME/.agents/skills/codex-strategy-research"
```

`agents/openai.yaml` 保持 `allow_implicit_invocation: false`。安装或讨论本 Skill 不启动研究；调用 Skill 不改变当前模型、推理等级、权限或并发配置。

## 安装并注册研究角色

将三个示例复制为仓库外的本机运行文件。下列 `cp -i` 只用于首次安装；已有本地副本时按后文合并更新，不用示例覆盖个性化配置。

```bash
mkdir -p "$research_codex_dir/codex-strategy-research/agents"
cp -i codex/agents/research-designer.example.toml "$research_codex_dir/codex-strategy-research/agents/research-designer.toml"
cp -i codex/agents/experiment-runner.example.toml "$research_codex_dir/codex-strategy-research/agents/experiment-runner.toml"
cp -i codex/agents/research-reviewer.example.toml "$research_codex_dir/codex-strategy-research/agents/research-reviewer.toml"
```

将下面的角色表合并到该启动入口实际使用的 `$research_codex_dir/config.toml`。保留已有配置；同名表已存在时核对并更新原表，不重复追加。相对 `config_file` 从声明它的配置文件目录解析，TOML 中不能写未展开的环境变量。

```toml
[agents."research-designer"]
description = "显式启用研究 Skill 后，只读设计研究假设、对照、证据与分析方法。"
config_file = "codex-strategy-research/agents/research-designer.toml"

[agents."experiment-runner"]
description = "显式启用研究 Skill 后，在明确预算与环境内准备、运行和分析实验。"
config_file = "codex-strategy-research/agents/experiment-runner.toml"

[agents."research-reviewer"]
description = "显式启用研究 Skill 后，独立只读审查研究设计、证据与结论。"
config_file = "codex-strategy-research/agents/research-reviewer.toml"
```

注册只使角色可用，不启动研究，也不启用开发治理。三个角色不代替旧的 `scope-planner`、`implementer`、`reviewer`；它们可以并存。主会话负责研究任务、授权和必要的 GitHub 状态维护；研究角色没有旧开发角色的无条件账号核验、Issue 到 PR 交付职责。

角色示例不设置 `model`、`model_reasoning_effort`、`sandbox_mode` 或 `approval_policy`，默认继承父会话配置。安装后可以在各角色本机 TOML 的顶层单独设置原生 `model` 和 `model_reasoning_effort`，使用当前环境实际支持的值。保留主会话和角色原有权限；父会话实时限制仍然适用。设计者、审查者的只读是职责约束，不等同于操作系统级或凭据级隔离。

## 普通启动与验证

```bash
codex -C /absolute/path/to/business-repository
```

随后输入 `$` 并从客户端候选列表选择 `codex-strategy-research`，或用 `/skills` 选择，再给出研究问题或当前任务入口及本轮授权。[官方文档](https://learn.chatgpt.com/docs/build-skills)将这种选择作为显式调用方式。准备范围、实际运行预算和数据用途按研究正文确定，不能因启动成功而推定已获准计算。未选择 Profile 也能使用已在基础配置注册的角色；首版不另提供研究 Profile。

安装后开一个新会话进行只读核对。支持 app-server 的客户端可用 `config/read` 检查实际配置，用 `skills/list` 列出发现的 Skills，再读取实际研究入口。CLI 的下列命令可检查未显式选择时的初始输入；它不能单独证明原生角色或显式 skill 正文已经加载：

```bash
codex debug prompt-input '仅核对 codex-strategy-research 的规则来源与原生角色配置，不启动研究，不执行实验或 GitHub 操作。'
```

本次在 CLI `0.163.0` 实测：`allow_implicit_invocation: false` 时，`debug prompt-input` 中的字面 `$codex-strategy-research` 没有自动注入正文。上游 [#40600](https://github.com/openai/codex/issues/40600)记录了同类非交互问题。不要把这一结果改报为显式调用通过，也不要因此允许隐式调用。本次通过 app-server `turn/start` 的正式 `type: "skill"` 输入成功注入正文；这验证了结构化显式入口，没有将 TUI 实机操作冒充已测结果。结构化输入示例中的路径须来自当前 `skills/list`：

```json
{"type":"skill","name":"codex-strategy-research","path":"/absolute/path/to/skills/codex-strategy-research/SKILL.md"}
```

如果所用客户端只能发送纯文本，可明确要求读取安装后 `SKILL.md` 的真实路径，并在同一请求中显式启用本 skill；确认正文已读取后再进行研究交接。不能仅看到名称就认定已加载。

同时检查该入口实际加载的基础配置、三个角色注册及 `config_file` 目标，确认研究源引用可读取。Skill 列表或输入中出现名称不能单独证明原生角色已加载；文件可解析也不能证明角色实际行为正确。需要核对派生时，让主会话向三个已注册角色分别交接一个只读职责核对任务：返回规则来源、职责和缺失条件，不运行实验、不写 GitHub。这项核对不要求启用开发治理或真实运行研究任务。使用普通持久会话验证派生；`--ephemeral` 不保存会话记录，本次在该模式下派生因 `no rollout found for thread id` 失败，不能据此宣称角色内容有误或普通启动不可用。

完整配置输出可能包含本机路径，不提交到源仓库或业务报告。磁盘更新不保证旧会话或既有子代理热加载；使用新会话确认生效。

## 多入口与升级

- 每个启动入口按其真实 `CODEX_HOME` 使用基础配置；未设置时才使用默认目录。分别注册研究角色，不能用另一个入口的成功结果代替。
- 多入口可以共享 Skill 链接和同一组已安装角色文件。共享角色时，各基础配置填写真实绝对 `config_file` 路径；路径只保存在仓库外，不复制整份配置、认证或会话目录。
- 升级先更新研究源，再对照示例合并角色指令和注册描述，保留本地模型、推理、权限及其他个人设置。已安装角色是副本，更新源仓库不会自动更新副本。
- 不改写旧开发角色、其他 Skill 或已有 Profile。若用户另行设置 Profile 并在其中覆盖研究角色，升级时同时核对该实际覆盖，不额外创建 Profile。
- 移动研究源时只定向更新 Skill 链接，并用新会话核对。父级交接采用新的真实源目录和版本，不能继续依据旧路径猜测。
- A、B 的执行主机、数据和资源授权仍由业务合同决定。安装 Skill 或注册角色不授权启动训练服务器、迁移计算或运行回放。

安装前在仓库外备份将要改变的配置，并记录原链接目标或入口不存在的事实。回滚时只撤销本次研究角色注册与链接变化，恢复受影响的原值；保留后来新增的无关设置，不用旧整份配置覆盖它们。角色副本仍被其他入口引用时不删除。回滚规则源或角色后同样用新会话核对，在当前研究记录中说明采用版本的变化。

研究规则升级依据文档差异、引用和职责审查，不以启动真实回测、训练、部署或资金操作作为安装测试。
