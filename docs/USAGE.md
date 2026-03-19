# 使用说明（Skill 驱动）

## 1. 接收需求
用户给出需求后：
1) 读取 `skills/PLATFORM.md`
2) 识别项目并读取 `skills/projects/*.md`
3) 应用全局规则（语言/第一性原理/最小改动/注释规范）
4) 若为代码任务：先输出伪代码 + ASCII 架构图，等待确认
5) 新建 workflow 文档（使用 `templates/WORKFLOW-TEMPLATE.md`）

## 2. 执行开发
按 PLATFORM.md 的 5 步执行：
- 需求分析
- 方案设计
- 代码实现
- 测试验证
- 提交代码

## 3. 记录与沉淀
- 执行记录：`workflows/*.md`
- 长期决策：`memory/MEMORY.md`
- 每日记录：`memory/YYYY-MM-DD.md`

## 4. 新项目接入
1) 复制 `templates/PROJECT-SKILL-TEMPLATE.md`
2) 生成 `skills/projects/{PROJECT}.md`
3) 在 `skills/PLATFORM.md` 加项目识别规则
