# 飞书文档操作指南

## 核心概念

**Wiki token 可以直接用于 Docx API**

- URL 中的 token（如 `OjuDwAIO1iZ4ERk36uRcVSufnuc`）可以直接调用 docx API
- 飞书会自动做映射，不需要先获取 obj_token
- 也可以通过 wiki API 获取 obj_token 后再查询（等效）

---

## 知识库权限

| 知识库名称 | space_id | 权限 |
|-----------|----------|------|
| 具身智能科研工作 | `7488632797588160540` | 只读 |
| kai 的工作区 | `7623009760186518492` | **读写** |

**写入操作应使用 "kai 的工作区"**。

---

## 操作流程

### 1. 读取飞书文档

```
直接调用 docx API:
GET /open-apis/docx/v1/documents/<wiki_token>/blocks

返回文档的所有 blocks，包括文本、标题、表格等。
```

### 2. 创建飞书文档

```
1. 创建文档:
   POST /open-apis/docx/v1/documents
   Body: { "title": "文档标题" }
   返回: document.document_id

2. 写入内容:
   POST /open-apis/docx/v1/documents/<document_id>/blocks/<document_id>/children
   Body: { "index": null, "children": [...] }
```

### 3. 更新文档内容（只修改部分）

```
PATCH /open-apis/docx/v1/documents/<document_id>/blocks/batch_update?document_revision_id=-1

Body: {
  "requests": [
    {
      "block_id": "<要修改的block_id>",
      "update_text": {
        "elements": [{"text_run": {"content": "新内容"}}],
        "style": {"align": 1},
        "fields": [1]
      }
    }
  ]
}

注意：
- 需要先 GET blocks 获取 block_id
- fields 是必需字段，[1] 表示修改对齐方式
```

### 4. 认证

```
POST /open-apis/auth/v3/tenant_access_token/internal
Body: { "app_id": "<APP_ID>", "app_secret": "<APP_SECRET>" }
返回: tenant_access_token
```

---

## Block 类型对照

| block_type | 类型 |
|------------|------|
| 2 | text |
| 3 | heading1 |
| 4 | heading2 |
| 5 | heading3 |
| 12 | bullet |
| 31 | table |

---

## 常见错误

| 错误 | 原因 | 解决 |
|------|------|------|
| `not found` | token 不存在或无权限 | 检查 token 是否正确，确认知识库权限 |
| wiki nodes create 404 | API 路径或权限问题 | 直接用 docx API 创建文档 |

---

## 凭证

- **App ID**: `cli_a940a9e84cde5bd7`
- **App Secret**: 见 TOOLS.md
- **Jet's open_id**: `ou_9a4f6485b96bc57c6cda92f11b69fe95`
