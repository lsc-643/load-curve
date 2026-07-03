# AGENTS.md — 负载曲线生成器（Power Profile）

> 给在本仓库工作的 AI 编码助手（Claude Code / Cursor / Copilot / Codex 等）。
> **改任何代码前先读这份**；复杂改动再读 [`ONBOARDING.md`](ONBOARDING.md)（完整上手说明）。
> **凡业务相关的改动，改完必须同步更新 `ONBOARDING.md`（含文末「变更记录」追加一行）**，让后来者了解现状。

## 这是什么

单文件、零依赖的交互式工具 `index.html`：用约 5 个核心输入仿真全年逐时负载曲线（8760 / 闰年 8784 点），是缅甸光储提案 ROI 测算的地基。**无构建步骤**，浏览器直接打开即可运行，所有 JS/CSS/数据都内联在 `index.html` 一个文件里。严格实现工程 SPEC v1.0。

## 三个来源（改对应逻辑前必读对应文档）

- **需求**：[`负载生成规则.md`](负载生成规则.md) — PM 原始文档，含权威飞书原链接。
- **算法**：[`SPEC-AlphaProposal-负载曲线生成算法.md`](SPEC-AlphaProposal-负载曲线生成算法.md) — 工程 SPEC v1.0（定稿，权威）。代码里 `§x` 皆指此。
- **纹理数据**：[`ε_噪声表.xlsx`](ε_噪声表.xlsx) — ε 噪声表；`index.html` 里的 `DAY_FACTOR`/`HOUR_TEX` 由 `tools/gen_texture_js.py` 从它生成。

## 常用操作

```bash
# 预览
python3 -m http.server 5050    # 打开 http://localhost:5050/index.html

# 更新 ε 纹理（改了 ε_噪声表.xlsx 之后）
pip install openpyxl           # 或加 --break-system-packages
python3 tools/gen_texture_js.py
#   把输出的 const DAY_FACTOR / const HOUR_TEX 两行覆盖回 index.html；
#   波动幅度若变，按 stderr 对照表更新 PRESETS 各类型的 amp

# 部署（改完上线）
git commit -m "feat: <改了啥>"
git push                        # GitHub Pages 自动构建
```
