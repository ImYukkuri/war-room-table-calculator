# TASK.md - Escort Phase（护航阶段）实现

## 开发目标

为海战流程增加独立的：

```txt
Escort Phase（护航阶段）
```

用于实现：

Cruiser Escort Stance（巡洋舰护航姿态）

规则要求：

* 蓝骰结算后才能发动
* 巡洋舰代替航母/战列舰承受伤害
* 可保护己方及盟友单位
* 受损巡洋舰仍可护航
* 仅沉没后失去能力

---

# 一、设计原则

不要严格模拟：

```txt
伤害分配
→ 询问是否转移
→ 重新分配
```

这种规则书流程。

改为：

```txt
护航拦截
```

流程。

本质效果完全一致。

但更适合当前网页工具架构。

---

# 二、海战阶段顺序调整

原流程：

```txt
海战
↓
伤害分配
↓
修复
```

修改为：

```txt
海战掷骰
↓
蓝骰结算
↓
Escort Phase
↓
自动/手动伤害分配
↓
修复单位
```

---

# 三、蓝骰结算

Escort Phase 开始前：

必须先结算所有蓝骰。

原因：

规则原文：

After blue hits are resolved...

---

结算后：

更新巡洋舰状态。

例如：

```txt
英国巡洋舰

存活 1
受损 1
```

---

只有仍存活的 Escort Cruiser 才能进入 Escort Phase。

---

# 四、进入 Escort Phase 条件

系统检查：

当前参战单位中是否存在：

```txt
Cruiser
+
Escort Stance
```

---

如果不存在：

自动跳过 Escort Phase。

进入伤害分配阶段。

---

如果存在：

进入 Escort Phase。

---

# 五、Escort Phase UI

顶部阶段栏新增：

```txt
空战
空战伤害分配
海战
Escort
海战伤害分配
修复
```

---

当前阶段高亮：

```txt
Escort
```

---

显示提示：

```txt
选择护航巡洋舰，然后选择要拦截的敌方骰子。
每次拦截消耗巡洋舰1点生命。
```

---

# 六、可选巡洋舰

系统自动高亮：

所有：

```txt
Cruiser
+
Escort Stance
+
未沉没
```

热区。

---

点击热区：

进入：

```txt
Escort Mode
```

---

同一时间只能选中一个巡洋舰。

---

再次点击：

取消选择。

---

# 七、骰子拦截机制

选中巡洋舰后：

敌方 Roll Results 区域高亮。

---

点击敌方骰子：

执行：

```txt
护航拦截
```

---

结果：

该骰子：

```txt
ESCORTED
```

状态。

---

显示：

灰色

半透明

不可再次使用

---

同时：

巡洋舰受到：

```txt
1点伤害
```

---

更新热区状态。

---

Battle Log记录：

```txt
UK Cruiser intercepted Red Hit
```

---

# 八、合法拦截骰子

不是所有骰子都能拦截。

---

允许：

红骰

绿骰

白骰

黑骰

---

禁止：

蓝骰

---

原因：

蓝骰已经结算。

---

# 九、白骰与黑骰限制

只有当：

当前白骰/黑骰

有资格命中：

```txt
Carrier
或
Battleship
```

时。

才能被 Escort 拦截。

---

例如：

当前兵种优势允许：

```txt
白骰 → 战列舰
```

则允许拦截。

---

如果：

当前白骰只能打：

```txt
Destroyer
```

则禁止拦截。

---

合法性判断必须复用现有：

```javascript
getValidTargets()
```

逻辑。

禁止硬编码。

---

# 十、生命消耗

每拦截一次：

巡洋舰承受：

```txt
1点伤害
```

---

受损巡洋舰：

仍可继续拦截。

---

只要：

```txt
未沉没
```

即可。

---

例如：

巡洋舰剩余2血。

则可连续拦截2次。

---

# 十一、巡洋舰沉没

如果护航过程中：

生命归零。

---

立即：

```txt
Destroyed
```

---

热区变为：

歼灭状态。

---

停止继续拦截。

---

# 十二、自动模式支持

自动分配模式下：

Escort Phase 自动执行。

---

自动算法：

优先使用 Escort Cruiser 拦截高价值伤害。

---

优先级：

```txt
红骰
↓
绿骰
↓
白骰
↓
黑骰
```

---

直到：

```txt
无可拦截骰子
```

或：

```txt
所有 Escort Cruiser 沉没
```

---

Battle Log记录：

```txt
Auto Escort:
Germany Cruiser intercepted Red Hit
```

---

# 十三、随机模式支持

Random 模式：

随机选择：

```txt
可用 Escort Cruiser
```

和：

```txt
可拦截骰子
```

---

# 十四、手动模式支持

Manual 模式：

玩家手动执行 Escort。

---

系统只负责：

* 高亮合法巡洋舰
* 高亮合法骰子
* 验证规则
* 记录结果

---

# 十五、热区表现

Escort 巡洋舰：

边框改为：

金色高亮

---

Tooltip新增：

```txt
ESCORT STANCE

可代替：
Carrier
Battleship

承受伤害
```

---

# 十六、Roll Results表现

被拦截骰子：

新增状态：

```txt
ESCORTED
```

---

样式：

* 灰色
* 半透明
* 划线（可选）
* Tooltip显示来源巡洋舰

---

示例：

```txt
ESCORTED

By:
UK Cruiser #1
```

---

# 十七、Battle Log

新增日志类型：

```txt
Escort Intercept
```

---

示例：

Germany Cruiser intercepted White Hit

UK Cruiser intercepted Battleship Hit

Japan Cruiser destroyed while escorting

---

# 十八、规则确认事项

Claude必须重新检查规则文档：

Cruiser Escort Stance

确认：

1. 护航巡洋舰是否能保护盟友单位

2. 白骰是否允许护航转移

3. 黑骰是否允许护航转移

4. 多艘巡洋舰同时存在时是否有额外限制

5. 护航伤害是否逐个结算

当前实现应尽量贴近规则原意。

但优先保证网页交互简洁和可操作性。
