---
title: 3 個月核心戰略：Biomechanics Infrastructure Layer
origin: self
created: 2026-05-10
type: strategy
status: draft
---

# 3 個月核心戰略

目標不是研究。

目標是：

> 建立「可重現、可驗證、可迭代」的 OpenSim/OpenCap/OpenSense/SCONE 技術基座。

禁止：

- 提前做產品
- 提前做 UI
- 提前做 AI agent
- 提前接醫院
- 提前做論文

前 90 天唯一 KPI：

```text
任何一台新機器
git clone
docker compose up
pytest
全部通過
```

這才叫基礎設施。

---

## 90 天總體架構

### Phase 1（Month 1）

主題：

```text
Environment + Deterministic Reproduction
```

目標：

- 建立 version-locked 環境
- 跑通官方 tutorial
- 建 CI/CD
- 建 regression tests

### Phase 2（Month 2）

主題：

```text
Pipeline Reproduction
```

目標：

- OpenSense gait
- OpenCap local processing
- Moco predictive
- NMSM

全部自動化。

### Phase 3（Month 3）

主題：

```text
Control + RL + Integration
```

目標：

- SCONE controller
- osim-rl baseline
- benchmark suite
- reproducibility package

---

## Repo 結構（第一天就固定）

```text
opensim-stack/
├── docker/
├── env/
├── scripts/
├── tests/
├── datasets/
├── opensim/
├── moco/
├── opensense/
├── opencap/
├── nmsm/
├── scone/
├── osim_rl/
├── benchmarks/
├── docs/
└── README.md
```

禁止：

```text
misc/
final/
new_final/
v2/
```

這是技術墳場。

---

## Month 1：Environment Reproduction

### Week 1

#### Task 1：OS / Container Strategy

固定：

| Layer | Version |
| --- | --- |
| OS | Ubuntu 22.04 |
| Python | 3.10 |
| Conda | Miniforge |
| OpenSim | 4.5 |
| Moco | 1.3 |
| OpenCap deps | fixed |
| CUDA | optional |

#### Task 2：Docker Baseline

交付：

```text
docker/
├── Dockerfile.opensim
├── Dockerfile.moco
├── Dockerfile.rl
└── docker-compose.yml
```

驗收：

```bash
docker compose up
opensim-cmd run-tool
```

#### Task 3：Version Lock

建立：

```text
environment.yml
requirements.txt
apt-packages.txt
```

驗收：

```bash
conda env create
```

完全可重建。

### Week 2

#### Task 4：OpenSim 官方 Tutorial 全重跑

完成：

- inverse kinematics
- inverse dynamics
- static optimization
- CMC

交付：

```text
tests/test_opensim_tutorials.py
```

驗收：

```bash
pytest
```

#### Task 5：建立 Regression Metrics

保存：

```text
expected outputs/
```

比較：

- RMSE
- objective values
- motion curves

目的：防止環境漂移。

### Week 3

#### Task 6：Moco Tutorial

完成：

- tracking
- predictive walking
- squat-to-stand

尤其：

```text
Moco predictive squat-to-stand
```

這是第一個真正 benchmark。

#### Task 7：Moco Automation

建立：

```bash
python run_moco.py
```

輸出：

```text
solution.sto
states.sto
controls.sto
```

### Week 4

#### Task 8：CI Pipeline

GitHub Actions：

```text
.github/workflows/
```

自動：

- build env
- run tutorials
- compare outputs

驗收：

```text
push code → CI pass
```

---

## Month 2：Pipeline Reproduction

### Week 5

#### Task 9：OpenSense Gait Pipeline

完成：

```text
IMU → OpenSense → gait estimation
```

交付：

```text
scripts/run_opensense.sh
```

輸出：

- orientation
- gait cycle
- joint angles

#### Task 10：OpenSense Validation

比較：

- 官方結果
- 自己結果

誤差門檻：

```text
RMSE < 5 degrees
```

### Week 6

#### Task 11：OpenCap Local Reprocessing

核心任務。

不是只跑 cloud。

要做到：

```text
video → local pipeline → OpenSim outputs
```

#### Task 12：OpenCap Automation

建立：

```bash
python process_opencap.py
```

輸出：

```text
.trc
.mot
.osim
```

### Week 7

#### Task 13：NMSM Tutorial

完成：

- neural musculoskeletal modeling tutorial
- training
- inference

建立 benchmark：

```text
inference latency
prediction accuracy
```

#### Task 14：Dataset Registry

建立：

```text
datasets/
├── opensim/
├── moco/
├── opensense/
├── opencap/
└── nmsm/
```

每個 dataset：

```text
metadata.json
checksum
license
```

### Week 8

#### Task 15：Unified CLI

建立：

```bash
python cli.py run moco
python cli.py run opensense
python cli.py run opencap
```

統一入口。

#### Task 16：Logging / Artifacts

保存：

```text
logs/
artifacts/
benchmarks/
```

---

## Month 3：Control + RL

### Week 9

#### Task 17：SCONE Installation

建立：

```text
scone/
```

跑：

- walking controller
- balance controller

#### Task 18：SCONE/OpenSim Integration

建立：

```text
controller → OpenSim simulation
```

輸出：

- gait stability
- energy cost

### Week 10

#### Task 19：osim-rl Baseline

完成：

- installation
- PPO baseline
- reward shaping

#### Task 20：RL Benchmark

保存：

```text
reward curves
training logs
videos
```

### Week 11

#### Task 21：Cross-stack Integration

建立：

```text
OpenCap
→ OpenSim
→ Moco
→ SCONE
→ RL
```

完整鏈條。

#### Task 22：Benchmark Suite

建立：

```bash
python benchmark.py
```

輸出：

| module | runtime | success |
| --- | --- | --- |

### Week 12

#### Task 23：Reproducibility Release

最重要交付。

必須包含：

```text
README
INSTALL
Dockerfiles
environment.yml
sample datasets
tests
CI workflows
benchmarks
```

#### Task 24：Golden Snapshot

tag：

```bash
v0.1-reproducible-stack
```

這才是 Phase 1 完成。

---

## Robert 每日 SOP

### Morning

#### 1. Pull 最新

```bash
git pull
```

#### 2. 跑 smoke test

```bash
pytest
```

#### 3. 看 benchmark

檢查：

- runtime drift
- convergence drift
- missing outputs

### Afternoon

#### 4. 單一任務開發

禁止 multi-tasking。

一次只做：

```text
一個 tutorial
一個 parser
一個 benchmark
```

### Evening

#### 5. Commit

格式：

```bash
git commit -m "feat(moco): add squat-to-stand automation"
```

---

## 技術原則

### 原則 1

```text
先 deterministic
後 intelligent
```

### 原則 2

```text
先 reproducible
後 scalable
```

### 原則 3

```text
先 benchmark
後 optimization
```

### 原則 4

```text
沒有 automated tests
等於沒完成
```

---

## Phase 1 完成標準

必須全部成立：

```text
□ 官方 tutorial 全重現
□ CI pass
□ Docker 可重建
□ 所有 benchmark deterministic
□ 新機器 2 小時內完成 setup
□ 所有 pipeline 有 automated tests
□ OpenCap local processing 成功
□ Moco squat-to-stand 成功
□ OpenSense gait 成功
□ NMSM tutorial 成功
□ SCONE controller 成功
□ osim-rl baseline 成功
```

---

## 最後判斷

這 90 天不是在「做產品」。

是在建立：

```text
Biomechanics Infrastructure Layer
```

這層做不好：

後面所有 AI、Agent、醫療、復健、數位孿生、RL 都會變成：

```text
不可重現的幻覺工程
```

真正的護城河不是模型。

是：

```text
可重現的 biomechanical compute stack
```
