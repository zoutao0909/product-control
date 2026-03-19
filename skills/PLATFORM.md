---
name: platform-core-workflow
summary: 平台级 Skill，定义需求识别、流程路由、执行步骤与留档规范。
when_to_use:
  - 收到任何新需求时
  - 需要识别项目并选择开发流程时
  - 需要统一执行与记录规范时
inputs:
  - 用户需求文本
  - 项目 Skill 文档
outputs:
  - workflow 执行记录
  - 开发决策与步骤状态
version: 1.1
owner: zoutao0909
---

# AI 交付平台 - 平台 Skill

**版本**: 1.0
**最后更新**: 2026-03-19

---

## 平台定位

这是一个流程固化平台，通过 Skill 来描述：
- 开发流程
- 项目架构
- 编码规范
- 开发习惯

**核心目标**：让 AI 能够理解流程，根据项目 Skill 自动完成开发任务。

---

## User's Global Rules（全局规则）

1. **Primary language is Chinese**（默认中文沟通）
2. **Use First Principles thinking**（采用第一性原理进行分析）
3. **Coding tasks: Must provide pseudocode & ASCII architecture first and wait for confirmation**
   - 任何代码类任务，先给出：
     - 伪代码（pseudocode）
     - ASCII 架构图
   - 必须等待用户确认后再写正式代码
4. **Scope: Strict minimal changes**（严格最小改动）
   - 仅改与当前需求直接相关的最小范围
   - 避免顺手重构、避免无关改动
5. **Style: Comprehensive comments are required**（注释必须完整）
   - 注释至少包含：步骤（steps）、输入（inputs）、输出（outputs）

---

## 核心原则

### 1. 一切皆 Markdown
- 所有文档都用 AI 可读的 Markdown 格式
- 每个文档都是一个 Skill，AI 可直接理解
- 无需复杂的代码实现，AI 直接"读"就能执行

### 2. Skill 驱动
- 平台流程在 Skill 中描述
- 项目规范在 Skill 中描述
- AI 根据 Skill 的指令执行

### 3. AI 执行
- AI 根据 Skill 描述直接执行
- 不需要额外的"流程引擎"
- AI 自己就是引擎

---

## 标准开发流程

### 步骤 1：需求分析

**目标**：理解用户需求，确认技术方案

**执行指令**：
1. 识别需求属于哪个项目（关键词匹配）
2. 加载对应项目的 Skill（`skills/projects/{PROJECT}.md`）
3. 分析功能范围和技术方案
4. 输出：需求分析文档（保存到 `workflows/{date}-{feature}.md`）

**输出模板**：
```markdown
# 需求分析：{功能名称}

## 项目
{项目名称}

## 功能范围
- {范围1}
- {范围2}
- {范围3}

## 技术方案
- {方案描述}
- API：{API设计}
- 数据：{数据模型}
```

---

### 步骤 2：方案设计

**目标**：设计详细的实现方案

**执行指令**：
1. 根据项目 Skill 中的架构设计
2. 设计数据模型（如果需要）
3. 设计 API 接口
4. 设计 UI 交互（如果需要）
5. 输出：设计文档（追加到 workflow 文档）

**遵循原则**：
- 数据模型遵循项目 Skill 中的 Prisma 规范
- API 遵循项目 Skill 中的 API 规范
- UI 遵循项目 Skill 中的组件规范

**输出模板**：
```markdown
## 方案设计

### 数据模型
{如果有新模型，描述字段}

### API 设计
**端点**: {HTTP方法} {路径}
**参数**: {参数列表}
**响应**: {响应格式}

### UI 设计
{如果需要，描述组件结构}
```

---

### 步骤 3：代码实现

**目标**：根据方案编写代码

**执行指令**：
1. 根据设计文档编写代码
2. 严格遵循项目 Skill 中的编码规范
3. 遵循项目 Skill 中的偏好
4. 编写单元测试（如果需要）

**代码规范检查清单**：
```
[ ] 使用正确的文件位置（遵循项目 Skill 的目录结构）
[ ] 遵循命名约定
[ ] 添加必要的注释
[ ] 处理错误情况
[ ] 遵循项目的架构原则
```

**执行操作**：
- 创建/修改文件
- 编写代码
- 保存文件

---

### 步骤 4：测试验证

**目标**：验证代码质量

**执行指令**：
1. 运行项目 Skill 中定义的测试命令
2. 确保测试通过
3. 手动验证核心功能
4. 输出：测试报告

**测试检查清单**：
```
[ ] 单元测试通过
[ ] 集成测试通过（如果有）
[ ] E2E 测试通过（核心功能）
[ ] 手动测试正常
```

**命令示例**：
```bash
npm test              # 运行测试
npm run lint          # 代码检查
npm run typecheck     # 类型检查
```

---

### 步骤 5：提交代码

**目标**：将代码提交到 Git 仓库

**执行指令**：
1. 检查代码变更：`git status`
2. 暂存文件：`git add .`
3. 提交：`git commit -m "feat: {功能描述}"`
4. 推送：`git push`

**提交信息规范**（Conventional Commits）：
```
feat: 添加新功能
fix: 修复 Bug
docs: 文档更新
test: 测试相关
refactor: 代码重构
style: 代码格式
chore: 构建/工具
```

---

## 项目识别规则

根据关键词匹配项目：

| 关键词 | 项目 |
|--------|------|
| mood, 心情, compass | mood-compass |
| user, 用户, auth, 登录 | mood-compass |
| export, 导出 | mood-compass |

**匹配流程**：
1. 提取用户需求中的关键词
2. 查找关键词对应的项目
3. 加载项目 Skill：`skills/projects/{PROJECT-NAME}.md`

---

## Skill 加载规则

### 加载顺序

1. **平台 Skill**：`skills/PLATFORM.md`（本文件）
   - 了解流程步骤
   - 了解执行规则

2. **项目 Skill**：`skills/projects/{PROJECT-NAME}.md`
   - 了解项目架构
   - 了解编码规范
   - 了解开发偏好

3. **历史记录**：`workflows/{date}-{feature}.md`
   - 了解之前的执行情况
   - 避免重复工作

4. **平台记忆**：`memory/MEMORY.md`
   - 了解重要决策
   - 了解用户偏好

### Skill 文件约定

| 文件 | 位置 | 用途 |
|------|------|------|
| 平台 Skill | `skills/PLATFORM.md` | 描述平台流程 |
| 项目 Skill | `skills/projects/{NAME}.md` | 描述项目规范 |
| Workflow | `workflows/{date}-{feature}.md` | 记录执行过程 |
| 平台记忆 | `memory/MEMORY.md` | 记录重要决策 |
| 日记忆 | `memory/{date}.md` | 记录日常操作 |

---

## AI 执行约定

### 每次执行前

1. **读取 Skill**
   ```
   - 读取 skills/PLATFORM.md
   - 识别项目，读取 skills/projects/{PROJECT}.md
   - 检查 memory/MEMORY.md（了解历史决策）
   ```

2. **检查状态**
   ```
   - 检查 workflows/ 目录，看是否有进行中的任务
   - 如果有，继续执行；如果没有，创建新的
   ```

### 执行过程中

1. **按照流程步骤执行**
   - 需求分析 → 方案设计 → 代码实现 → 测试验证 → 提交代码

2. **遵循项目 Skill 的规范**
   - 代码位置
   - 命名规范
   - 编码风格
   - 测试要求

3. **记录执行状态**
   - 在 workflow 文档中记录每一步的执行情况
   - 标记完成状态（✅ / ⏳ / ❌）

### 执行完成后

1. **更新 workflow 文档**
   ```markdown
   ## 总结
   **完成时间**: {时间}
   **总耗时**: {时长}
   **状态**: ✅ 已完成
   ```

2. **更新 MEMORY.md**（如果是重要决策）
   ```markdown
   ## {日期}
   - {决策内容}
   - {原因}
   ```

---

## 异常处理

### 需求不清晰

**处理方式**：
1. 询问用户，澄清需求
2. 记录澄清后的需求到 workflow 文档

### 技术方案有风险

**处理方式**：
1. 在方案设计步骤中标注风险
2. 提供多个备选方案
3. 等待用户确认后再执行

### 测试失败

**处理方式**：
1. 分析失败原因
2. 修复代码
3. 重新运行测试
4. 如果无法修复，标记为 Blocked，等待人工介入

---

## 平台维护

### 添加新项目

1. 创建项目 Skill：`skills/projects/{PROJECT-NAME}.md`
2. 更新平台 Skill 中的项目识别规则
3. 在 MEMORY.md 中记录项目信息

### 修改流程

1. 更新 `skills/PLATFORM.md`
2. 在 MEMORY.md 中记录变更原因
3. 通知所有相关人员

### 更新项目规范

1. 更新 `skills/projects/{PROJECT}.md`
2. 在 MEMORY.md 中记录变更内容
3. 后续开发自动应用新规范

---

## 附录

### 文件命名规范

| 类型 | 规范 | 示例 |
|------|------|------|
| Workflow | `{date}-{feature}.md` | `2026-03-19-export-feature.md` |
| 项目 Skill | `{PROJECT-NAME}.md` | `MOOD-COMPASS.md` |
| 日记忆 | `{date}.md` | `2026-03-19.md` |

### 状态标记

- ✅ 已完成
- ⏳ 进行中
- ❌ 失败
- 🚫 阻塞
- ⏸️ 暂停

---

## 联系与反馈

如有问题或建议，请：
1. 更新 `memory/MEMORY.md` 记录
2. 或直接联系平台维护者
