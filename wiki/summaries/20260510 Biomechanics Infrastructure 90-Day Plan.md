---
origin: self
source: "[[artifacts/strategies/biomechanics-infrastructure-90-day-plan]]"
compiled: 2026-05-10
tags: [biomechanics, reproducibility, infrastructure, OpenSim, OpenCap, OpenSense, SCONE, Moco, RL]
---

# 我的主張

90 天的核心目標不是做產品、研究、UI、AI agent、醫院合作或論文，而是建立一個「可重現、可驗證、可迭代」的 OpenSim/OpenCap/OpenSense/SCONE 技術基座。唯一 KPI 是任何新機器都能 `git clone`、`docker compose up`、`pytest` 並全部通過。

這份策略把真正的護城河定義為可重現的 biomechanical compute stack，而不是模型本身。若底層 biomechanics infrastructure 不穩，後續 AI、agent、醫療、復健、數位孿生與 RL 都會變成不可重現的幻覺工程。

# 實踐經驗

計畫用 3 個月分階段建立基礎設施：

- Month 1：Environment + Deterministic Reproduction。固定 Ubuntu 22.04、Python 3.10、Miniforge、OpenSim 4.5、Moco 1.3 與相關依賴，建立 Docker、version lock、官方 tutorial 重跑、regression metrics 與 CI pipeline。
- Month 2：Pipeline Reproduction。自動化 OpenSense gait、OpenCap local processing、Moco predictive、NMSM tutorial、dataset registry、unified CLI、logs/artifacts/benchmarks。
- Month 3：Control + RL + Integration。跑通 SCONE controller、SCONE/OpenSim integration、osim-rl baseline、cross-stack integration、benchmark suite 與 reproducibility release。

計畫也明確固定 repo 結構，禁止 `misc/`、`final/`、`new_final/`、`v2/` 這類技術墳場式命名，並要求每天遵守 pull、pytest smoke test、benchmark drift 檢查、單一任務開發與 evening commit 的 SOP。

# 未解決的問題

- 「Phase 1 完成標準」實際涵蓋 OpenCap、Moco、OpenSense、NMSM、SCONE、osim-rl 等完整 90 天項目，命名上可能需要改成「90 天完成標準」或拆成各月 gate。
- OpenCap local reprocessing 與 SCONE/OpenSim integration 的官方支援、版本相容性、授權與資料集取得方式仍需要逐項驗證。
- CUDA 被列為 optional，但 RL、NMSM 或影片處理若在 CPU-only 新機器上執行，可能無法符合「2 小時內完成 setup」與完整 benchmark 的期望。
- golden tag `v0.1-reproducible-stack` 的 release checklist 需要定義通過門檻、資料集大小限制、CI 執行時間上限與 deterministic tolerance。

# 跟外部研究的對照

這份策略尚未引用外部研究或官方文件；它是一份自我制定的工程路線圖。後續編譯時應補充 OpenSim、Moco、OpenSense、OpenCap、SCONE、NMSM、osim-rl 的官方安裝與 tutorial 來源，並將版本、資料集、授權、測試門檻轉成可驗證的 repo artifacts。
