# TOOLS.md - Local Notes

Skills define _how_ tools work. This file is for _your_ specifics — the stuff that's unique to your setup.

## What Goes Here

Things like:

- Camera names and locations
- SSH hosts and aliases
- Preferred voices for TTS
- Speaker/room names
- Device nicknames
- Anything environment-specific

## Examples

```markdown
### Cameras

- living-room → Main area, 180° wide angle
- front-door → Entrance, motion-triggered

### SSH

- home-server → 192.168.1.100, user: admin

### TTS

- Preferred voice: "Nova" (warm, slightly British)
- Default speaker: Kitchen HomePod
```

## Why Separate?

Skills are shared. Your setup is yours. Keeping them apart means you can update skills without losing your notes, and share skills without leaking your infrastructure.

---

Add whatever helps you do your job. This is your cheat sheet.

---

## 飞书配置

### 应用信息

- **App ID**: `cli_a940a9e84cde5bd7`
- **授权方式**: 通过群授权（机器人加入群，群设为知识库管理员）

### 用户信息

- **Jet's open_id**: `ou_9a4f6485b96bc57c6cda92f11b69fe95`

### 知识库

| 知识库名称 | space_id | 权限 |
|-----------|----------|------|
| 具身智能科研工作 | `7488632797588160540` | 只读 |
| kai 的工作区 | `7623009760186518492` | 读写 |

### Memory Search

- **Provider**: `local`
- **Model**: `embeddinggemma-300m-qat-q8_0`
- **SQLite**: `~/.openclaw/memory/main.sqlite`
