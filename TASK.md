# TASK-BATTLE-PHASE-2

## 开发前要求

先阅读项目规则文档（War Room规则摘要）。

重点关注：

* Air Battle
* Surface Battle
* Force Advantage
* Damage Assignment
* Repair Step
* Black Dice
* White Dice

当前热区系统已完成，不允许重构热区架构。

后续所有功能基于现有热区实现。

---

# 页面总体流程

新增顶部阶段导航条。

当前阶段必须高亮。

流程固定为：

```text
① 空战
↓
② 空战伤害分配
↓
③ 陆战
↓
④ 陆战伤害分配
↓
⑤ 修复单位
```

阶段之间采用按钮切换。

只有当前阶段完成后才能进入下一阶段。

---

# 战斗模式

轴心国与同盟国分别拥有独立伤害分配模式。

例如：

Axis:
Auto

Allies:
Manual

允许同时存在。

配置位置：

战斗页面顶部。

---

支持以下五种模式：

## 1 Auto

自动分配伤害

使用本文档中的 Auto Allocation Algorithm

---

## 2 Random

随机分配

所有合法目标随机选择。

仍然遵守：

* 受损优先
* 轻伤优先
* 伤害类型限制

---

## 3 Same Nation First

优先同国

优先继续打击已经开始承受该颜色伤害的国家。

例如：

Germany Infantry
已承担1点黄色伤害

下一发黄色伤害：

优先继续分配给Germany。

---

## 4 Largest Stack

最大堆叠

优先伤害当前该兵种数量最多的国家。

例如：

Germany Infantry 6

Italy Infantry 2

Yellow Hit

优先Germany。

---

## 5 Manual

完全手动。

系统只负责：

* 判断合法目标
* 高亮合法目标
* 记录结果

不进行推荐。

---

# Auto Allocation Algorithm

Auto模式核心算法。

---

## 第一原则

优先遵守兵种内部伤害优先级。

---

## 第二原则

优先全歼一个国家的该兵种。

示例：

Germany Infantry 2

Italy Infantry 6

Yellow Hit 2

结果：

Germany全部消灭。

而不是：

Germany 1
Italy 1

---

## 第三原则

若只能全歼其中一个国家：

优先全歼单位数量较少的国家。

示例：

Germany Infantry 2

Italy Infantry 6

Yellow Hit 2

结果：

Germany全部消灭。

---

## 第四原则

若无法全歼任何国家：

优先伤害该兵种数量最多的国家。

示例：

Germany Infantry 8

Italy Infantry 3

Yellow Hit 2

结果：

Germany承担2。

---

## 第五原则

若数量相同：

按照国家显示顺序。

从左到右。

---

# 兵种内部伤害优先级

所有模式均遵守。

包括：

Auto
Random
Same Nation First
Largest Stack

---

## Infantry

Offense
优先于
Defense

---

## Artillery

Offense
优先于
Defense

---

## Armor

Offense
优先于
Defense

原因：

Offense仅2生命。

Defense拥有完整生命轨。

---

## Fighter

Offense
优先于
Defense

---

## Bomber

Ground Support
优先于
Strategic

---

## Destroyer

Escort
优先于
Defense
优先于
Offense

---

# 多生命单位

适用于：

Armor
Carrier
Battleship

---

状态：

Healthy
Light Damage
Heavy Damage
Destroyed

---

伤害优先级：

Heavy Damage
优先于
Light Damage
优先于
Healthy

---

白骰与黑骰：

优先命中：

Heavy Damage

之后：

Light Damage

之后：

Healthy

---

# Roll Results 与伤害分配联动

新增：

Damage Assignment Mode

---

进入伤害分配阶段后：

Roll Results区域保留。

不隐藏。

---

所有骰子必须保留实体显示。

例如：

🟡
🟡
🔵
⚫

---

已分配骰子：

显示灰色。

并添加：

Assigned状态。

---

当前待分配骰子：

高亮闪烁。

---

未分配骰子：

正常显示。

---

# 分配流程

系统始终锁定一个骰子。

例如：

当前：

Yellow

---

系统：

高亮所有合法目标。

---

玩家点击目标。

---

骰子变为Assigned。

---

自动跳转下一枚骰子。

---

若当前势力所有骰子完成：

自动切换到另一势力。

---

双方全部完成：

允许进入下一阶段。

---

# Force Advantage

伤害分配阶段必须实时检查。

---

若当前阶段存在：

Force Advantage

则：

黑骰
白骰

合法目标同步变化。

---

高亮逻辑必须同步更新。

---

# 热区显示升级

现有热区已扩大。

允许显示更多信息。

---

每个热区将当前数字改为：

```text
存活（未进入伤害分配阶段时则为部署单位数）
受损
歼灭
```

三个数字。

---

布局示例：

```text
8
2
3
```

或者：

```text
Alive 8

Damaged 1 其实总会是1，因为受伤单位总是要优先被分配伤害变成Destroyed

Destroyed 3
```

---

颜色：

Alive
绿色

Damaged
橙色

Destroyed
红色

---

# 多生命单位显示

Armor
Carrier
Battleship

---

示例：

Alive 4

damaged 1 轻伤重伤使用同一个数字，因为轻伤单位总是要优先分配伤害，变成重伤

Destroyed 3

---

但是轻伤重伤要做颜色区分：

Healthy
绿色

Light
黄色

Heavy
橙红色

Destroyed
红色

---

# 热区伤害记录系统

每个热区保存：

Assigned Dice Log

---

记录：

Dice Color

Assignment Time

Source Phase

---

示例：

使用die-token实体骰子记录

---

# Tooltip

在分配伤害阶段点击热区：

展开Tooltip。

---

显示：

当前区域已分配骰子。使用die-token实体骰子记录

---

支持：

关闭

清空

撤销

---

# 快速分配操作

左键：

自动添加伤害。

---

滚轮上滚：

自动添加伤害。

---

优先顺序：

颜色骰
↓
白骰
↓
黑骰

---

# 快速撤销操作

右键：

撤销伤害。

---

滚轮下滚：

撤销伤害。

---

撤销优先级：

黑骰
↓
白骰
↓
颜色骰

---

# Mapping Tool区域用途变更

热区已完成。

Mapping Tool不再作为主要功能区。

改为：

Battle Log

---

显示：

阶段切换

骰子结果

自动分配结果

撤销记录

修复记录

Force Advantage变化

---

用于追踪整个战斗过程。

```
```
