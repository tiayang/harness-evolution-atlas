# Claude Code Harness — 架构演进图谱

> **Live site**: https://tiayang.github.io/harness-evolution-atlas/

从单层 advisory 到 broker daemon — 一个 LLM agent harness 在 8 周内从 CLAUDE.md 单文件演进到 78 hooks + 20 rules + 4 sub-agents + GPG-signed broker daemon 的完整记录。

## What this is

本仓库是 [Claude Code](https://claude.com/claude-code) harness 配置 (`~/.claude/`) 演进过程的可视化文档。每个阶段对应一类反复失败的工程化补救。

## Sections

1. **演进时间线** (V0 → V5) — 5 个主要阶段 + 每阶段的动因与教训
2. **当前架构** — 78 hooks × 13 事件类型 × 12 职责域分布
3. **Dispatcher 收口** — 13 PreToolUse hooks → 5 group 模块的收口方式
4. **T4 Hard-Gate Broker** — Phase 1.1 → 4.1 完整 critic 收敛 + signed receipt 架构
5. **防御模式目录** — 4 个真实失败 case 的多层防御 stack
6. **架构原则** — 10 条 + 3 个元缺陷
7. **组件清单** — 完整盘点 + 核心 lib 调用图

## Numbers (2026-05-26 snapshot)

| | Count | Notes |
|---|---|---|
| Hooks | 78 | 77 active · 1 deprecated |
| Hook event types | 13 | PreTool / Stop / UserPrompt / ... |
| Rules | 20 | `~/.claude/rules/*.md` |
| Sub-agents | 4 | researcher / qa / diagnostician / memory-manager |
| Gate registry | 6 | alignment → agenda → scope → learn-x → harness-kb → closed-loop |
| Enforcement | 70 warn-only · 7 hard-block · 1 dry-run | 90% advisory, 10% deterministic surgical |
| Core lib | ~2,300 LoC | openai_caller + budget_tracker + closed_loop_tracker + decision-review |
| KB wikis | 200+ | 9 categories |
| T4 tests | 89/89 | Phase 4.1 follow-up baseline |

## Key design tensions

- **Advisory 94% ceiling** — single-prompt rules have a ~94% failure rate on high-confidence errors
- **Self-review 60-70% ceiling** — architecture-level changes need independent critic
- **Distribution prior > read instruction** — training data prior beats prompt rules at generation time
- **DoD = world state ≠ exit code** — "local command exit 0" is not "task done"
- **Same-UID = friction not boundary** — T4 broker is high-friction prevention, not adversarial defense

## License

Documentation only. Architecture sketches reflect real configuration state but are not source code redistribution.

## Related

- Sibling site: [T4 Hard-Gate Saga](https://tiayang.github.io/t4-hard-gate-saga/) — earlier deep-dive on the T4 critic-loop design rounds
