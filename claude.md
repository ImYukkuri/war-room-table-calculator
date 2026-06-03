# TASK.md

# War Room Assistant

War Room（作战室）桌游辅助工具

---

# 项目目标

开发一个 War Room 桌游辅助工具。

主要解决：

1. 战斗录入速度慢
2. 骰子计算复杂
3. 战斗板状态记录困难
4. 压力与勋章管理麻烦
5. 回合结算容易遗漏

项目定位：

不是规则查询器。

不是电子版桌游。

而是：

**桌面共用战斗辅助终端。**

默认一台设备由所有玩家共用。

---

# 技术要求

必须：

* 单HTML运行
* 原生HTML
* 原生CSS
* 原生JavaScript
* LocalStorage自动保存

禁止：

* React
* Vue
* Angular
* Electron
* TypeScript
* Node依赖
* 构建工具

最终目标：

双击 index.html 即可运行。

---

# 当前开发阶段

Phase 1

完成：

* 首页
* 战斗页面框架
* Battle Board Mapping Tool
* 单位录入系统

暂不开发：

* 骰子计算
* 战斗结算
* 压力系统
* 回合系统

---

# 页面结构

整个应用采用单页架构。

页面：

1. Home
2. Battle
3. Round
4. Stress

使用：

showView()

切换页面。

---

# 首页 Home

功能：

## 战斗模式

单选：

* 完整战斗模式
* 快速战斗模式

默认：

完整战斗模式

保存到：

gameState.battleMode

---

## 战场类型

单选：

* 陆战
* 海战

保存到：

gameState.battleType

---

## 操作按钮

* 开始新战斗
* 继续上次战斗
* 清空存档

---

# 战斗页面 Battle

这是当前阶段核心页面。

---

# Battle Board背景

使用：

land.jpg

作为陆战背景。

后续支持：

sea.png

作为海战背景。

背景图必须完整显示。

保持比例缩放。

---

# Battle Board Overlay System

在背景图上覆盖透明交互层。

所有操作区域均由坐标定义。

例如：

{
id: "germany_infantry_defense",
x: 0.35,
y: 0.62,
w: 0.04,
h: 0.05
}

坐标必须采用百分比。

禁止固定像素。

---

# Mapping Tool

必须优先开发。

功能：

显示全部热区。

每个热区：

* 半透明高亮
* 可点击
* 显示名称

调试模式：

显示：

* X
* Y
* W
* H

后续允许拖动调整。

目标：

建立完整Battle Board坐标系统。

---

# 国家布局

采用Battle Board真实布局。

国家作为纵列。

例如：

德国
意大利
日本

英国
美国
苏联
中国

具体位置以后通过Mapping Tool校准。

---

# 姿态布局

采用Battle Board原始姿态。

不允许自创布局。

必须对应实际规则板。

例如：

Air

* Fighter Offensive
* Fighter Defensive
* Bomber Strategic
* Bomber Support

Land

* Anti-Air
* Artillery Offensive
* Artillery Defensive
* Infantry Offensive
* Infantry Defensive
* Armor Offensive
* Armor Defensive

实际名称以后根据规则进一步校验。

---

# 单位录入系统

这是项目最高优先级功能。

目标：

比实体摆棋子更快。

---

# 热区输入

每个热区对应：

国家 × 姿态

例如：

Germany + Infantry Defense

---

支持：

左键：

+1

右键：

-1

滚轮向上：

+1

滚轮向下：

-1

最小值：

0

---

# 输入状态

用户可进入输入模式。

---

# 国家输入模式

点击国家图标。

进入：

INPUT MODE

再次点击退出。

---

# 空战输入模式

点击国家图标上半部分。

进入：

AIR INPUT MODE

---

# 陆战输入模式

点击国家图标下半部分。

进入：

LAND INPUT MODE

---

# 输入模式行为

进入输入模式后：

用户直接按数字键。

例如：

3

则当前热区数量变为：

3

然后自动跳转到下一个热区。

---

再次输入：

1

自动填入下一格。

---

再次输入：

4

自动填入下一格。

---

# 两位数规则

当前版本不处理两位数。

仅接受：

0-9

用户可通过：

鼠标增减

进行修正。

---

# 自动跳转顺序

必须预定义。

例如：

Fighter Offensive
↓
Fighter Defensive
↓
Bomber Strategic
↓
Bomber Support
↓
Anti-Air
↓
Artillery Offensive
↓
Artillery Defensive
↓
Infantry Offensive
↓
Infantry Defensive
↓
Armor Offensive
↓
Armor Defensive

然后切换到下一个国家。

---

# 热区显示

调试模式：

显示全部热区。

颜色：

绿色透明：

可编辑

黄色透明：

当前热区

红色透明：

非法区域

---

显示：

当前值

例如：

[3]

实时显示在热区中央。

---

# 数据结构

使用统一状态。

const gameState = {
battleMode: "normal",

```
battleType: "land",

nations: {},

battleBoard: {},

ui: {}
```

}

---

# Battle Board数据

采用：

battleBoardState

例如：

{
germany: {
infantryDefense: 3,
infantryOffense: 1,
artilleryDefense: 2
}
}

---

# LocalStorage

必须自动保存：

* 当前战斗模式
* 当前战场类型
* 当前录入状态
* Battle Board数据

刷新后自动恢复。

---

# 当前阶段完成标准

完成以下内容即视为第一阶段完成：

[ ] 首页完成

[ ] 完整战斗模式选择

[ ] 快速战斗模式选择

[ ] 陆战选择

[ ] 海战选择

[ ] 页面切换系统

[ ] 加载land.jpg

[ ] Battle Board Overlay

[ ] 调试热区显示

[ ] 国家输入模式

[ ] 键盘快速录入

[ ] 自动跳格

[ ] 左键加1

[ ] 右键减1

[ ] 滚轮调整

[ ] LocalStorage保存

完成后再进入骰子系统开发阶段。
