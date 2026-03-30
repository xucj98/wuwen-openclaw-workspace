# MEMORY.md - Kai 的长期记忆

这里存储从经验中提炼的原则、每次对话都会用到的内容。

---

## Memory 文件使用规范

### 原则

| 文件 | 存什么 | 不存什么 |
|------|--------|---------|
| **USER.md** | 用户是谁 | 配置参数、具体技术信息 |
| **TOOLS.md** | 环境配置 | 用户偏好 |
| **MEMORY.md** | 经验教训、原则 | 具体事件流水 |
| **memory/YYYY-MM-DD.md** | 每日活动流水 | 需要长期记住的原则 |

### 案例

❌ **错误做法**：把 space_id 放入 MEMORY.md
```
MEMORY.md 里写"飞书 space_id 是 7488xxx..."
→ 这些配置信息应该放 TOOLS.md
```

❌ **错误做法**：把今天配置 memory search 的过程放入 MEMORY.md
```
MEMORY.md 里写"今天安装了 node-llama-cpp，下载了模型..."
→ 这是活动流水，应该放 memory/2026-03-30.md
```

✅ **正确做法**：把分类原则放入 MEMORY.md
```
MEMORY.md 里写"Memory 文件使用规范..."
→ 这是每次对话都要用到的决策框架
```

---

## 任务执行策略

### 原则

- **耗时 >5 分钟的任务应该派生 subagent**
- **多步骤、有不确定性的任务适合派生 subagent**

### 案例

❌ **错误做法**：自己执行耗时任务让用户等待
```
用户说"帮我配置 memory search"
→ 我执行 npm install（9分钟）+ 下载模型（1分钟）
→ 用户等了 10 分钟
```

✅ **正确做法**：派生 subagent 后台执行
```
用户说"帮我配置 memory search"
→ 我派生 subagent 执行安装和配置
→ 告诉用户"我派了一个子任务处理，完成后通知你"
→ 用户可以继续做其他事
```

⚠️ **判断依据**：
- 简单命令（<1分钟）→ 自己执行
- 耗时任务（>5分钟）→ 派生 subagent
- 不确定时 → 优先派生 subagent

---

## 多 Session 写入规范

### 原则

- **每日日志（memory/YYYY-MM-DD.md）采用追加模式，不覆盖已有内容**
- **MEMORY.md 只在主 session 写入，避免多 session 冲突**
- 写入前先读取文件，确保不丢失其他 session 的内容

### 案例

✅ **正确做法**：追加到每日日志
```markdown
## 晚上新增内容

- 配置了 git backup cron job
- 讨论了多 session 写入规范
```

❌ **错误做法**：覆盖整个文件
```markdown
# 2026-03-30
今天做了 xxx...（覆盖了之前 session 写的内容）
```

✅ **正确做法**：MEMORY.md 谨慎写入
```
只在主 session（Jet 的飞书 DM）中写入 MEMORY.md
群聊 session 不写入 MEMORY.md
cron job / subagent 不写入 MEMORY.md
```

### 为什么这样做？

1. **追加不会丢失内容**：多个 session 可以安全地追加到同一文件
2. **MEMORY.md 精贵**：只放长期原则，减少写入频率，降低冲突风险
3. **先读后写**：确保不覆盖其他 session 的内容
