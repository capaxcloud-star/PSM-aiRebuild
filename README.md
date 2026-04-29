✅ **PSM‑aiRebuild 协作宪法 V4‑Lite 最终完整版本（含决策流 + 工程流 + 测试闭环 + 失败路径 + 子 Issue 规则 + 决策变更规则）**  

---

# 🚗 PSM‑aiRebuild  
## 协作宪法 V4‑Lite  
**Minimal Viable Collaboration Loop（Decision + Build + Verify）**

---

# 🎯 目标

当前阶段目标：

- 快速推进开发  
- 最小制度成本  
- 决策清晰可追踪  
- 建立最小可运行协作闭环  
- 所有功能必须完成验证后才能关闭  

原则：

> 推进优先于治理  
> 未验证不得关闭  

---

# 👥 角色定义

| 角色 | 职责 |
|------|------|
| F | 人类产品决策（最终裁定） |
| A | 功能主导（Feature Owner） |
| B | 数据支撑（Data Support） |

当前核心协作关系：

> A ↔ F

---

# ⚖ 权力原则

- 产品行为由 F 决定  
- 功能实现由 A 主导  
- 数据设计由 B 支撑  
- 只有涉及产品行为变化必须提 Issue  

---

# 📝 Issue 协作机制（决策入口）

## ✅ 必须提 Issue 的情况

A 在以下情况必须提 Issue：

- 需求不明确  
- 存在多个实现选项  
- 可能影响产品行为  
- 功能边界不清晰  

否则：

> 直接实现，不提 Issue  

---

# 🏷 Issue 标题格式

```
[模块] 问题简述
```

规则：

- 不编号  
- 不分级  
- 不复杂命名  
- 使用自然语言  

---

# 📄 Issue 内容模板

```md
## 分类
功能 / 数据 / 跨模块 / 调研

## 问题
（必须说明业务影响）

## 选项（如有）
A.
B.
C.

## 建议（可选）
A 当前建议：XXX

---
标签：AI
```

要求：

- 简洁  
- 聚焦决策点  
- 使用业务语言  
- 涉及系统逻辑必须补充业务影响  

---

# ✅ Issue 提交规则（强制）

1. 必须选择 label（仅允许）：

   - question  
   - bug  
   - documentation  

2. 必须选择 Project：  
   `PSM Rebuild`

3. 首次提交必须设置：

```
Project status = AI
```

4. 若需补充说明：

```
Project status = 待澄清
```

---

# ✅ F 的回复规则

F 只需：

- 明确选择  
- 不写长解释  
- 不展开讨论  

示例：

> 选择 A：全员覆盖，admin 禁用自助。  

然后：

```
Project status → todo
```

若信息不足：

```
Project status → 待澄清
```

---

# 🔄 主 Issue 状态定义（严格语义）

| 状态 | 含义 |
|------|------|
| AI | 等待 F 进行产品决策或裁决。 |
| 待澄清 | A 认为信息不足，F 需补充业务影响或选项细节。信息不足 |
| todo | 产品决策完成，可开发 |
| In Progress | 工程进行中 |
| Done | 功能实现完成且测试通过 |
| AI分拣 | FEEDBACK 专用入口，由 A 进行原子化拆解。 |

⚠ 状态必须严格对应真实进度  
⚠ 不允许提前 Done  

---

# 🔄 完整闭环流程

---

## ✅ 无歧义路径

```
A 提 Issue（AI）
        ↓
F → todo
        ↓
A → In Progress
        ↓
A 实现代码完成
        ↓
A 建测试（子 Issue）
        ↓
A 代码可验证 → Test Status = Test Ready
        ↓
A 测试通过 → Test Status = Passed
        ↓
A 关闭测试子 Issue
        ↓
A Close 主 Issue → Done
```

---

## ✅ 有歧义路径

```
A 提 Issue（AI）
        ↓
F → 待澄清
        ↓
A 补充说明
        ↓
F → todo
        ↓
A → In Progress
        ↓
A 实现代码完成
        ↓
A 建测试（子 Issue）
        ↓
A 代码可验证 → Test Status = Test Ready
        ↓
A 测试通过 → Test Status = Passed
        ↓
A 关闭测试子 Issue
        ↓
A Close 主 Issue → Done
```

---

# 🧪 测试机制（子 Issue 模型）

每个主 Issue 必须：

- 建立至少一个测试子 Issue  
- 子 Issue 用于验证决策点  
- 子 Issue 与主 Issue建立关联  
- 测试子 Issue 仅用于验证，不用于决策  

---

# ✅ Test Status 字段定义（主 Issue 字段）

| 状态 | 含义 |
|------|------|
| Not Ready | 尚不可验证 |
| Test Ready | 代码已可验证 |
| Passed | 测试通过 |
| Failed | 测试失败 |

---
# 📥 统一反馈与变更协议 (Feedback Loop)
“不拉扯，不重开，新单解决”
禁止重开：严禁 Re-open 已关闭的 Issue。

无论是开发中、测试中，还是 Issue 关闭后发现问题，F 统一采用以下方式：

F 的操作 (极简输入)：
>新建 Issue，标题：[FEEDBACK] 描述或 #原Issue号。
>内容：直接写“哪里坏了”或“哪里想改”，甚至是一串截图。
>状态：统一设为 AI分拣。

A 的自动化分拣 (工程转换)：
判定逻辑：
>A 识别为 Bug -> 自动转派新单 Status: todo（直接修）
>A 识别为 需求变更 -> 自动转派新单 Status: AI（重新谈）。

引用与关闭：A 在新 Issue 中引用 Ref #FEEDBACK单 和 Ref #原功能单，随后立即关闭该 FEEDBACK 单。
语义隔离：原 Issue 永远记录“当时是怎么定、怎么做的”，新 Issue 记录“后来是怎么改的”，确保历史可溯源。
核心收益：F 不需要做任何逻辑判断，只需要负责“发现不爽”；A 负责把“不爽”翻译成“工程任务”。

---

# 🔒 关闭铁律

```
Test Status ≠ Passed → 不得 Done
```

关闭主 Issue 前必须满足：

- 功能已实现  
- 测试完成  
- 测试子 Issue 已关闭  
- Test Status = Passed  

---

# ❗ 测试失败处理规则

当 `Test Status = Failed`：

- 主 Issue 保持 `In Progress`  
- A 必须修复问题  
- 修复后重新验证  
- 重新验证通过后才能设置为 `Passed`  

禁止：

- 直接关闭主 Issue  
- 删除测试 Issue  
- 重置状态而不修复问题  

---

# 🔁 决策变更规则

当实现阶段发现决策需要修改：

- 不在原 Issue 内反复拉扯  
- 新开 Issue 进行新的决策  
- 原 Issue 保持原决策闭环  

原则：

> 一个 Issue 只对应一次决策闭环  

---

# 🛠 A 的开发规则

A：

- 只处理 `todo` 状态 Issue  
- 开发开始后改为 `In Progress`  
- 实现完成后建立测试子 Issue  
- 测试通过并关闭测试子 Issue后关闭主 Issue  
- 不反复讨论已裁决问题  
- 不拖延已决策问题  

---

# 🚫 当前阶段不做

- 不做复杂编号  
- 不做优先级体系  
- 不做版本治理  
- 不做测试矩阵  
- 不做回归分级  
- 不做复杂自动化门禁  

原因：

> 当前阶段目标是建立节奏，而不是建设制度  

---

# 🔁 最小稳定闭环模型

```
Issue = 决策入口
todo = 可开发信号
In Progress = 工程进行中
Test Passed = 可关闭条件
Done = 完成确认
```

---

# 🚀 升级条件（未来）

当出现以下情况之一时，考虑升级到 V5：

- Issue 数量 > 100  
- 测试数量显著增长  
- 回归冲突频繁  
- 跨模块依赖复杂  
- 需要多环境验证  

在此之前：

> 保持轻量，快速推进  

---

# ✅ 当前模型定义

**Human‑Fast Loop Engineering Model**

- 决策驱动  
- 工程分层  
- 测试闭环  
- 低制度成本  
- 快速反馈  

---

（End of V4‑Lite Final Complete Edition）
