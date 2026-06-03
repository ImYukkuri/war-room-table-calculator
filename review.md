# War Room Rules Review — Combat Timing / Bombing / Repair

本文件记录本次本地规则复核结论，用于指导当前骰子辅助 UI。

参考来源：

- `warroom_rules.txt`：精简后的本地规则参考。
- `warroom_rules_original_backup.txt`：原 OCR 备份。
- `assets/land.jpg` / `assets/sea.png`：Battle Board 战斗值来源。

---

## 1. 战斗阶段顺序

规则顺序是：

1. Air Battle Stage。
2. Resolve Raids：Strategic Bombing Raid / Convoy Raid。
3. Surface Battle Stage。
4. Battle Debrief。

结论：

- Strategic Bombing 不在 Air Battle 中。
- Strategic Bombing 在 Air Battle 之后、Surface Battle 之前。
- Surface Battle 不是和 Air Battle 同时发生的同一批骰子。

---

## 2. Strategic Bombing timing

规则结论：

- Bombers 可在 Battle Setup 时放入 Strategic Stance。
- 这些 Bombers 必须先经过 Air Battle。
- 只有 survive Air Battle 的 Strategic Bombers 才能执行 Strategic Bombing Raid。
- 每个 raiding Bomber 掷 4 颗骰。
- 每个 Bomber 单独掷骰并结算伤害后，再处理下一个 Bomber。

当前 UI 处理：

- Strategic Bombing 单独显示为 Strategic Dice 区域。
- 由于它发生在 Air Battle 后，界面用 `Roll Post-Air: Strategic + Surface` 按钮一次触发双方 Strategic 与 Surface 区域掷骰。
- 这只是便捷掷骰动作合并；规则时序仍应理解为 Strategic Bombing 在 Surface Battle 之前。

---

## 3. Air Battle 伤亡是否参与后续阶段

规则结论：

- Air Battle Stage 中被 eliminated 的 Air Units 不参与后续步骤。
- 因此不是“所有骰子没有先后顺序”。
- Air Battle 先发生，并会影响后续 Strategic Bombing 和 Surface Battle 的可用 Air Units。

同一 Battle Stage 内：

- 双方 Combat Values 先计算。
- 该 Stage 的结果视为同时。
- 按 batch 掷骰，每个 batch 掷完要先分配伤害，再进入下一个 batch。
- 结果不能留到后续 batch 合并。

---

## 4. Surface Battle 中的 Air Units

规则结论：

- Surface Battle 统计 Land/Naval Units，以及 surviving Air Units。
- 正在执行 Strategic Bombing Raid 的 Bombers 不参与 Surface Battle。
- Damaged Air Units 如果在 Ground / Surface stance，仍参与 Surface Battle。
- 原因：repair 发生在所有 Battle Stages 完成之后。

---

## 5. 单位修复规则

Battle Debrief Step A 才处理修复，发生在 Air、Raid、Surface 都结束后。

规则结论：

- Lightly Damaged 的 Battleship / Carrier / Armor 不需要修复，直接回 Initial Status。
- Damaged Unit 可以花 1 个任意 Resource 修复。
- 有 Port Advantage 时，Naval Units 免费修复。
- 不修或不能修的 Damaged Unit 移到 Eliminated。
- Air Units 如果只是因为无法修复而最终 eliminated，它们仍然已经参与过 Surface Battle 或 Strategic Bombing，因为修复是在所有 Battle Stages 后才发生。

---

## 6. 当前骰子 UI 设计结论

为符合规则和桌面操作速度，当前 UI 分为两层：

### 顶部控制面板

顶部控制面板负责实际掷骰与实体骰子展示：

- `空战`：同时掷 Axis Air 与 Allied Air。
- `地面战 / 海面战`：同时掷 Axis Surface 与 Allied Surface。
- `战略轰炸`：同时掷 Axis Strategic Bombing 与 Allied Strategic Bombing；海战时不显示。
- `新战斗`：清空 Battle Board 所有单位，并清空骰子结果、summary 与阶段按钮状态。
- `重新本场战斗`：保留 Battle Board 单位录入，只清空骰子结果、summary 与阶段按钮状态。

顶部控制面板左右分区显示最近一次点击阶段的实体骰子结果：

- 左侧：Axis 最近掷骰结果，最多显示 30 颗。
- 右侧：Allied 最近掷骰结果，最多显示 30 颗。
- 两侧各自显示本次实体结果的 DiceSummary。

### 左右侧栏

Battle Board 左右两侧只显示摘要卡，不显示实体骰子图片：

- 左侧：Axis Air、Axis Land/Sea、Axis Strategic。
- 右侧：Allied Air、Allied Land/Sea、Allied Strategic。
- 每张卡显示该阶段自动计算的骰子总数和颜色 summary。
- Strategic card 在海战时隐藏。

注意：如果实际战斗中 Air Battle 已经消灭某些 Strategic Bombers 或 Ground/Surface Air Units，玩家应先在 Battle Board 上把对应单位数量调整掉，再点击后续阶段掷骰。当前 Phase 1/骰子辅助不自动执行伤害分配和单位移除。
