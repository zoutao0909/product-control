# AI 交付平台

**版本**: 1.0
**状态**: 运行中
**创建时间**: 2026-03-19

---

## 🎯 平台定位

这是一个 **Skill 驱动的 AI 交付平台**，核心设计原则：

> **一切皆 Markdown，每个文档都是一个 Skill，AI 可直接理解并执行。**

---

## 🌟 核心特性

### 1. Skill 驱动
- 平台流程在 `skills/PLATFORM.md` 中描述
- 项目规范在 `skills/projects/` 中描述
- AI 根据 Skill 的指令执行任务

### 2. 无需代码实现
- 不需要传统的状态机代码
- 不需要流程引擎
- AI 自己就是"引擎"

### 3. AI 友好
- 所有文档使用 Markdown 格式
- AI 可直接读取和理解
- 无需解析器或编译器

### 4. 人类友好
- 任何人都能编辑 Markdown
- 无需编程知识
- 易于维护和扩展

---

## 📁 目录结构

```
product-control/
├── skills/
│   ├── PLATFORM.md              # 平台流程 Skill
│   └── projects/
│       └── MOOD-COMPASS.md      # 项目 Skill
├── workflows/                   # 执行记录
│   └── {date}-{feature}.md
├── memory/                      # 平台记忆
│   ├── MEMORY.md                # 长期记忆
│   └── {date}.md                # 日记忆
├── README.md                    # 本文件
└── docs/                        # 设计文档
```

---

## 🚀 快速开始

### 对于 AI（执行开发任务）

1. **读取 Skill**
   ```
   - 读取 skills/PLATFORM.md（了解流程）
   - 识别项目，读取 skills/projects/{PROJECT}.md
   - 检查 memory/MEMORY.md（了解历史决策）
   ```

2. **执行流程**
   ```
   需求分析 → 方案设计 → 代码实现 → 测试验证 → 提交代码
   ```

3. **记录状态**
   ```
   - 在 workflows/ 中记录执行过程
   - 在 memory/ 中记录重要决策
   ```

### 对于人类（维护平台）

1. **添加新项目**
   - 创建 `skills/projects/{PROJECT-NAME}.md`
   - 更新 `skills/PLATFORM.md` 中的项目识别规则
   - 在 `memory/MEMORY.md` 中记录项目信息

2. **修改流程**
   - 更新 `skills/PLATFORM.md`
   - 在 `memory/MEMORY.md` 中记录变更原因

3. **更新项目规范**
   - 更新 `skills/projects/{PROJECT}.md`
   - 后续开发自动应用新规范

---

## 📖 Skill 说明

### 平台 Skill（PLATFORM.md）

描述平台的流程和规则：
- 标准开发流程（5个步骤）
- 项目识别规则
- Skill 加载规则
- AI 执行约定

### 项目 Skill（projects/*.md）

描述项目的规范和偏好：
- 技术栈
- 架构设计
- 编码规范
- 测试规范
- 开发偏好

---

## 🎓 工作原理

### 传统方式 vs Skill 驱动

| 传统方式 | Skill 驱动 |
|---------|-----------|
| 需要写代码实现状态机 | ✅ 不需要，AI 自己执行 |
| 需要实现流程引擎 | ✅ 不需要，AI 就是引擎 |
| 需要复杂的软件系统 | ✅ 只需要 Markdown 文档 |
| AI 调用平台的 API | ✅ AI 读取平台的 Skill |

### 执行示例

**用户**: "给 mood-compass 加个数据导出功能"

**AI 执行**:
1. 读取 `skills/PLATFORM.md` → 了解流程
2. 识别项目 → mood-compass
3. 读取 `skills/projects/MOOD-COMPASS.md` → 了解规范
4. 执行步骤：
   - 需求分析 → 保存到 workflow
   - 方案设计 → 遵循项目架构
   - 代码实现 → 遵循编码规范
   - 测试验证 → 运行 npm test
   - 提交代码 → git push

---

## 📊 试点项目

### mood-compass

- **类型**: Web Application
- **技术栈**: Next.js + TypeScript + Prisma
- **当前版本**: v0.3.0
- **状态**: ✅ 已完成 Phase 3
- **Skill**: `skills/projects/MOOD-COMPASS.md`

**功能**:
- ✅ 心情记录（新增/编辑/删除）
- ✅ 趋势图表
- ✅ 标签统计
- ✅ 时间筛选
- ✅ 低分预警
- ✅ 数据导出（JSON/CSV）
- ✅ 移动端优化

---

## 🔧 维护指南

### 日常维护

- **记录重要决策**: 更新 `memory/MEMORY.md`
- **清理过期记录**: 定期清理 `workflows/` 目录
- **备份数据**: 备份 `memory/` 和 `workflows/`

### 更新 Skill

1. **修改流程**: 更新 `skills/PLATFORM.md`
2. **修改项目规范**: 更新 `skills/projects/{PROJECT}.md`
3. **记录变更**: 在 `memory/MEMORY.md` 中说明

### 添加新项目

1. 创建项目 Skill: `skills/projects/{PROJECT-NAME}.md`
2. 更新项目识别规则: `skills/PLATFORM.md`
3. 记录到 `memory/MEMORY.md`

---

## 🛡️ 安全注意事项

- ⚠️ **PAT 安全**: 定期轮换，使用最小权限
- 🔒 **敏感信息**: 不要在 Skill 中写密码和密钥
- 💾 **数据备份**: 定期备份 memory 和 workflows

---

## 📝 文档约定

### Markdown 规范

- 使用标准 Markdown 语法
- 标题层级清晰（H1 > H2 > H3）
- 使用列表和表格提高可读性
- 代码块标注语言类型

### 文件命名

| 类型 | 规范 | 示例 |
|------|------|------|
| Workflow | `{date}-{feature}.md` | `2026-03-19-export.md` |
| 项目 Skill | `{PROJECT-NAME}.md` | `MOOD-COMPASS.md` |
| 日记忆 | `{date}.md` | `2026-03-19.md` |

---

## 🎯 未来计划

- [ ] 添加更多试点项目
- [ ] 优化 Skill 模板
- [ ] 总结最佳实践
- [ ] 自动化增强

---

## 📞 联系

**维护者**: 涛 邹
**GitHub**: zoutao0909
**邮箱**: zoutao0909@163.com

---

## 📜 许可证

MIT

---

**让 AI 理解流程，让开发更加简单。**
