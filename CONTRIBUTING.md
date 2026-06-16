# 贡献指南

知识库欢迎所有人贡献内容。**所有改动必须通过 PR 合并**——main 分支受保护。

## 工作流

```
1. Fork / Clone 仓库
2. 在新分支上写文档（不要直接在 main 上改）
3. 跑本地校验脚本（可选）
4. Push 分支 → 开 PR
5. 等 review + approve → 合并
6. 服务器侧 KB 服务自动拉取 + reindex（5 分钟内生效）
```

## 一份"优化过"的文档长啥样

### ✅ 好文档的特征

- 文件名描述性强（`employee-leave-policy.md` 不是 `notes.md`）
- frontmatter 完整（`title` 必填，其他按需）
- 顶层标题用 H1，正文用 H2 起
- 每段 H2 聚焦一个独立小主题（**chunk 友好**）
- 中文为主，必要时英文术语用代码块包裹
- 表格用标准 markdown，不用 HTML
- 没有敏感信息（API key / token / 手机号 / 内部 URL）

### ❌ 常见错误

- 一整篇文档没有 H2/H3 标题 → RAG 只能切成 1 个 chunk，检索差
- 一个文件塞 5 个不相关主题 → 应该拆成 5 个文件
- frontmatter 写错格式 → 解析失败被丢弃
- 复制了带 HTML 标签的原始页面 → 用纯 md 重写
- 误把内部 URL / 凭证贴进去 → 索引后会从回答里漏出来

## 文件命名约定

| 类型 | 命名 | 示例 |
| --- | --- | --- |
| 通用规则 | kebab-case，描述性 | `reimbursement-guide.md` |
| 单一 FAQ | `<topic>-faq.md` | `leave-faq.md` |
| 错误码 | `<product>-error-codes.md` | `miot-spec-error-codes.md` |
| 流程文档 | `<verb>-<object>.md` | `submit-expense-report.md` |

## 文件位置

按主题分子目录：

```
knowledge/
├── README.md                 ← 仓库总览（不动）
├── template.md               ← 新文档模板（复制这个改）
├── CONTRIBUTING.md           ← 本文件（不动）
├── handbook/
│   ├── 01-onboarding.md
│   └── 02-tech-stack.md
├── faq/
│   ├── leave.md
│   └── reimbursement.md
└── ref/
    └── miot-spec-error-codes.md
```

## 本地校验（可选，但建议）

提交 PR 前，可以在本地跑一遍校验，确保格式正确：

```bash
# 1. 复制 .env.example 写一个本地的，然后装依赖
pip install pyyaml markdown-it-py

# 2. 校验 frontmatter + 结构
python scripts/validate.py knowledge/your-new-file.md

# 会输出：
#   ✓ frontmatter OK
#   ✓ H2 sections: 4
#   ✓ avg chunk size: 320 chars
#   ✓ no sensitive patterns found
```

## 提 PR

```bash
git checkout -b feat/add-leave-policy
# 编辑 / 新增文件
git add .
git commit -m "feat: add annual leave policy"
git push origin feat/add-leave-policy
# 然后去 GitHub 开 PR
```

## Review 检查清单（reviewer 用）

- [ ] frontmatter 完整，`title` 必填
- [ ] H1 + H2 结构清晰，**每段 H2 是一独立主题**
- [ ] 无敏感信息（grep 过 `token` `key` `secret` `password`）
- [ ] 文件名 / 路径符合命名约定
- [ ] 没有 HTML 标签污染（除非必要）
- [ ] 一篇文档长度合理（< 5000 字）

## 合并后

- Reviewer merge PR → main
- 服务器侧 cron 拉取（每 5 分钟）→ KB 服务自动 reindex
- 飞书 bot 等所有 consumer 立即可检索到新内容

## 紧急修改

紧急情况（比如线上 FAQ 出错）由维护者直接 push main，但事后应该在 PR 里补一个说明。
