# Mood Compass - 项目 Skill

**版本**: 1.0
**最后更新**: 2026-03-19

---

## 项目信息

| 属性 | 值 |
|------|-----|
| ID | mood-compass |
| 名称 | Mood Compass |
| 类型 | Web Application |
| 仓库 | https://github.com/zoutao0909/mood-compass |
| 本地路径 | /root/.openclaw/workspace/mood-compass |
| 当前版本 | v0.3.0 |

---

## 技术栈

| 类别 | 技术 | 版本 |
|------|------|------|
| 语言 | TypeScript (strict mode) | ^5.0.0 |
| 框架 | Next.js (App Router) | ^16.0.0 |
| 数据库 | Prisma + SQLite | 6.x |
| 测试（单元） | Vitest | ^1.0.0 |
| 测试（E2E） | Playwright | Latest |
| 样式 | Tailwind CSS | ^3.0.0 |
| 验证 | Zod | ^3.0.0 |
| 图表 | Recharts | ^2.0.0 |
| 日期 | date-fns | ^3.0.0 |

---

## 架构设计

### 目录结构

```
mood-compass/
├── app/
│   ├── api/{feature}/
│   │   └── route.ts              # API 路由（每个功能独立目录）
│   ├── {page}/
│   │   └── page.tsx              # 页面组件
│   ├── layout.tsx                # 根布局
│   └── globals.css               # 全局样式（Tailwind）
├── lib/
│   ├── prisma.ts                 # Prisma 客户端（单例）
│   └── {domain}.ts               # 业务逻辑（如果需要）
├── prisma/
│   ├── schema.prisma             # 数据模型定义
│   └── migrations/               # 数据库迁移
├── e2e/                          # E2E 测试（Playwright）
│   └── {feature}.spec.ts
├── package.json
├── vitest.config.ts              # 单元测试配置
├── playwright.config.ts          # E2E 测试配置
└── tsconfig.json                 # TypeScript 配置
```

### 架构原则

1. **App Router 模式**
   - 使用 Next.js 16 的 App Router
   - 路由基于文件系统自动生成

2. **API 路由分离**
   - 每个功能独立一个 `route.ts` 文件
   - 遵循 RESTful 设计

3. **业务逻辑抽离**
   - 复杂业务逻辑放在 `lib/` 目录
   - API 路由只负责请求/响应处理

4. **单例模式**
   - Prisma 客户端使用全局单例
   - 避免开发环境创建多个连接

5. **类型安全**
   - 所有代码使用 TypeScript
   - 启用 strict 模式
   - 优先使用类型推断，必要时定义接口

---

## API 规范

### 路由约定

**文件位置**: `app/api/{resource}/route.ts`

**HTTP 方法**: 
- `GET` - 查询
- `POST` - 创建
- `PUT` - 更新
- `DELETE` - 删除

**导出函数**:
```typescript
export async function GET(req: Request) { }
export async function POST(req: Request) { }
export async function PUT(req: Request) { }
export async function DELETE(req: Request) { }
```

### 输入验证

**使用 Zod Schema**:

```typescript
import { z } from "zod";

const CreateSchema = z.object({
  score: z.number().int().min(1).max(10),
  tags: z.array(z.string()).default([]),
  note: z.string().max(500).optional(),
});

// 在 handler 中验证
export async function POST(req: Request) {
  const json = await req.json();
  const parsed = CreateSchema.safeParse(json);
  
  if (!parsed.success) {
    return NextResponse.json(
      { error: parsed.error.flatten() },
      { status: 400 }
    );
  }
  
  // 使用 parsed.data
}
```

### 响应格式

**统一格式**:

```typescript
// 成功响应
return NextResponse.json({ data: result });

// 创建成功（201）
return NextResponse.json(created, { status: 201 });

// 错误响应
return NextResponse.json(
  { error: "错误信息" },
  { status: 400 }
);

// 文件下载
return new NextResponse(content, {
  headers: {
    "Content-Type": "text/csv",
    "Content-Disposition": "attachment; filename=data.csv",
  },
});
```

### 错误处理

**使用 try-catch**:

```typescript
export async function POST(req: Request) {
  try {
    const json = await req.json();
    // 业务逻辑
    const result = await prisma.model.create({ data });
    return NextResponse.json(result, { status: 201 });
  } catch (error) {
    console.error("API Error:", error);
    return NextResponse.json(
      { error: "Internal Server Error" },
      { status: 500 }
    );
  }
}
```

### 查询参数

**解析 URL 参数**:

```typescript
export async function GET(req: Request) {
  const { searchParams } = new URL(req.url);
  const days = searchParams.get("days");
  const format = searchParams.get("format") || "json";
  
  // 使用参数
}
```

### 动态路由

**文件位置**: `app/api/{resource}/[id]/route.ts`

**访问参数**:

```typescript
export async function PUT(
  req: Request,
  { params }: { params: Promise<{ id: string }> }
) {
  const { id } = await params;
  // 使用 id
}
```

---

## 组件规范

### 页面组件

**客户端组件**（需要交互）:

```typescript
"use client";

import { useState, useEffect } from "react";

export default function Page() {
  const [data, setData] = useState(null);
  
  useEffect(() => {
    // 数据获取
  }, []);
  
  return (
    <main className="mx-auto max-w-4xl p-6">
      {/* UI */}
    </main>
  );
}
```

**服务端组件**（纯展示）:

```typescript
// 不需要 "use client"

export default async function Page() {
  const data = await fetchData();
  
  return (
    <main>
      {/* UI */}
    </main>
  );
}
```

### 状态管理

**优先级**:
1. `useState` - 简单局部状态
2. `useReducer` - 复杂状态逻辑
3. Zustand - 全局状态（目前不需要）

**示例**:

```typescript
// 简单状态
const [count, setCount] = useState(0);

// 复杂对象
const [form, setForm] = useState({
  score: 6,
  tags: "",
  note: "",
});

// 更新部分字段
setForm(prev => ({ ...prev, score: 7 }));
```

### 事件处理

**命名约定**: `on{Action}`

```typescript
const handleSubmit = async (e: React.FormEvent) => {
  e.preventDefault();
  // 处理提交
};

const handleChange = (e: React.ChangeEvent<HTMLInputElement>) => {
  setValue(e.target.value);
};
```

### 样式规范

**必须使用 Tailwind CSS**:

```typescript
// ✅ 正确
<div className="flex items-center gap-2 rounded-lg border p-4">

// ❌ 错误
<div style={{ display: 'flex', padding: '16px' }}>
```

**移动优先**:

```typescript
// 基础样式应用于移动端，响应式前缀用于大屏
<div className="text-sm md:text-base p-4 md:p-6">
```

**常用组合**:
```
- 容器: mx-auto max-w-4xl p-6 md:p-10
- 卡片: rounded-xl border bg-white p-4 md:p-6
- 按钮: rounded-lg bg-black px-4 py-2 text-white
- 输入框: rounded border px-3 py-2
```

---

## 数据模型规范

### Prisma Schema

**文件位置**: `prisma/schema.prisma`

**模型定义**:

```prisma
model MoodEntry {
  id        Int      @id @default(autoincrement())
  score     Int
  tags      String
  note      String?
  createdAt DateTime @default(now())
  updatedAt DateTime @updatedAt
}
```

**字段约定**:
- `id` - 使用自增整数
- `createdAt` - 创建时间，自动填充
- `updatedAt` - 更新时间，自动更新
- 可选字段使用 `?`

### 数据库操作

**查询**:

```typescript
// 查询所有
const entries = await prisma.moodEntry.findMany({
  orderBy: { createdAt: "desc" },
  take: 50,
});

// 条件查询
const entries = await prisma.moodEntry.findMany({
  where: { score: { gte: 5 } },
});

// 关联查询（如果有）
const entry = await prisma.moodEntry.findUnique({
  where: { id },
  include: { user: true },
});
```

**创建**:

```typescript
const entry = await prisma.moodEntry.create({
  data: {
    score: 6,
    tags: "work, exercise",
    note: "Good day",
  },
});
```

**更新**:

```typescript
const entry = await prisma.moodEntry.update({
  where: { id },
  data: { score: 7 },
});
```

**删除**:

```typescript
await prisma.moodEntry.delete({
  where: { id },
});
```

### 迁移命令

```bash
# 创建迁移
npx prisma migrate dev --name {migration-name}

# 重置数据库（开发环境）
npx prisma migrate reset

# 生成客户端
npx prisma generate

# 打开 GUI
npx prisma studio
```

---

## 编码规范

### TypeScript 规范

1. **启用 strict 模式** (tsconfig.json)
   ```json
   {
     "compilerOptions": {
       "strict": true
     }
   }
   ```

2. **禁止使用 any**
   ```typescript
   // ❌ 错误
   const data: any = fetchData();
   
   // ✅ 正确
   interface Data { ... }
   const data: Data = fetchData();
   ```

3. **使用接口定义数据结构**
   ```typescript
   interface MoodEntry {
     id: number;
     score: number;
     tags: string;
     note: string | null;
     createdAt: Date;
   }
   ```

4. **使用类型推断**
   ```typescript
   // ✅ 推荐（类型推断）
   const entries = await prisma.moodEntry.findMany();
   
   // ❌ 不必要（重复声明）
   const entries: MoodEntry[] = await prisma.moodEntry.findMany();
   ```

### 命名约定

| 类型 | 规范 | 示例 |
|------|------|------|
| 文件名 | kebab-case | `mood-entry.ts` |
| 组件名 | PascalCase | `MoodEntry.tsx` |
| 函数名 | camelCase | `getMoodEntries()` |
| 常量 | UPPER_SNAKE_CASE | `MAX_ENTRIES` |
| 类型/接口 | PascalCase | `MoodEntry` |
| API 路由 | kebab-case | `/api/mood-entries` |

### 注释规范

**复杂逻辑必须注释**:

```typescript
// 计算平均分（排除异常值）
const avgScore = entries
  .filter(e => e.score >= 1 && e.score <= 10)
  .reduce((sum, e) => sum + e.score, 0) / entries.length;
```

**API 必须有 JSDoc**:

```typescript
/**
 * 获取心情记录列表
 * @param days - 最近多少天（可选）
 * @returns 心情记录数组
 */
export async function getMoodEntries(days?: number) {
  // ...
}
```

**组件 Props 注释**:

```typescript
interface MoodCardProps {
  /** 心情分数 (1-10) */
  score: number;
  /** 标签列表 */
  tags: string[];
  /** 可选备注 */
  note?: string;
}
```

---

## 测试规范

### 单元测试

**框架**: Vitest

**位置**: `tests/unit/{module}.test.ts` 或 `__tests__/{module}.test.ts`

**覆盖率**: ≥ 70%

**命令**:
```bash
npm test              # 运行测试
npm run test:watch    # 监听模式
npm run test:coverage # 生成覆盖率报告
```

**测试结构**:

```typescript
import { describe, it, expect } from 'vitest';

describe("模块名称", () => {
  it("应该做某事", () => {
    // Arrange
    const input = ...;
    
    // Act
    const result = ...;
    
    // Assert
    expect(result).toBe(expected);
  });
});
```

**API 测试示例**:

```typescript
import { describe, it, expect } from 'vitest';

describe("Mood API", () => {
  it("应该创建心情记录", async () => {
    const response = await fetch('/api/mood', {
      method: 'POST',
      body: JSON.stringify({ score: 6, tags: ["work"] }),
    });
    
    expect(response.status).toBe(201);
    const data = await response.json();
    expect(data.score).toBe(6);
  });
});
```

### E2E 测试

**框架**: Playwright

**位置**: `e2e/{feature}.spec.ts`

**范围**: 核心用户流程

**命令**:
```bash
npx playwright test       # 运行测试
npx playwright test --ui  # UI 模式
```

**测试示例**:

```typescript
import { test, expect } from '@playwright/test';

test("完整心情记录流程", async ({ page }) => {
  await page.goto('/');
  
  // 1. 新增记录
  await page.fill('input[type="number"]', '6');
  await page.click('button[type="submit"]');
  
  // 2. 验证显示
  await expect(page.locator('text=Score: 6')).toBeVisible();
  
  // 3. 删除记录
  await page.click('button:has-text("删除")');
  await expect(page.locator('text=Score: 6')).not.toBeVisible();
});
```

---

## 开发偏好

### UI 偏好

1. **简洁清爽**
   - 避免复杂的视觉效果
   - 使用简单的颜色和形状
   - 保持界面整洁

2. **移动优先**
   - 优先考虑手机体验
   - 使用响应式设计
   - 触摸友好

3. **色彩克制**
   - 主要使用黑白灰
   - 点缀少量彩色（用于强调）
   - 避免花哨的配色

4. **排版清晰**
   - 使用足够的留白
   - 层次分明
   - 易于扫描

### 逻辑偏好

1. **函数式优先**
   - 优先使用纯函数
   - 避免副作用
   - 数据不可变

2. **单一职责**
   - 每个函数只做一件事
   - 函数名清晰表达意图
   - 避免上帝函数

3. **易于测试**
   - 依赖注入
   - 避免全局状态
   - Mock 友好

4. **错误友好**
   - 提供清晰的错误信息
   - 优雅降级
   - 记录错误日志

### 提交规范

**使用 Conventional Commits**:

```
feat: 添加数据导出功能
fix: 修复登录验证错误
docs: 更新 README
test: 添加单元测试
refactor: 重构 API 路由
style: 代码格式化
chore: 更新依赖
```

**提交信息示例**:

```
feat: add data export API (JSON & CSV)

- Support JSON and CSV formats
- Add query parameter: format=json|csv
- Add download headers for CSV
```

---

## 开发检查清单

### 新功能开发

- [ ] 设计数据模型（如果需要）
- [ ] 更新 Prisma schema
- [ ] 运行数据库迁移：`npx prisma migrate dev`
- [ ] 实现 API 路由
- [ ] 编写单元测试
- [ ] 实现 UI 组件
- [ ] 编写 E2E 测试（核心功能）
- [ ] 手动测试
- [ ] 更新 README（如果需要）
- [ ] 运行 lint：`npm run lint`
- [ ] 运行测试：`npm test`
- [ ] 提交代码

### Bug 修复

- [ ] 编写失败的测试用例
- [ ] 修复 Bug
- [ ] 确认测试通过
- [ ] 手动验证
- [ ] 提交代码

### 代码审查

- [ ] 代码遵循项目规范
- [ ] 没有 TypeScript 错误
- [ ] 没有 lint 错误
- [ ] 测试覆盖率达标
- [ ] 没有安全漏洞

---

## 常用命令

### 开发

```bash
npm run dev          # 启动开发服务器 (http://localhost:3000)
npm run build        # 构建生产版本
npm start            # 启动生产服务器
```

### 测试

```bash
npm test             # 运行单元测试
npm run test:watch   # 监听模式
npm run test:coverage # 生成覆盖率报告

npx playwright test       # 运行 E2E 测试
npx playwright test --ui  # UI 模式
```

### 代码质量

```bash
npm run lint         # 运行 ESLint
npm run typecheck    # 运行 TypeScript 类型检查
```

### 数据库

```bash
npx prisma studio       # 打开数据库 GUI
npx prisma migrate dev  # 运行迁移
npx prisma generate     # 生成客户端
npx prisma db push      # 直接推送 schema（不创建迁移）
```

---

## 已知问题和限制

1. **SQLite 限制**
   - 不支持并发写入
   - 单机使用足够
   - 不适合高并发场景

2. **暂无用户认证**
   - 所有数据公开
   - 后续可能添加

3. **暂无数据备份**
   - 需要手动备份 `dev.db`
   - 建议定期备份

4. **测试覆盖率**
   - 当前覆盖率约 70%
   - E2E 测试覆盖核心流程

---

## 未来计划

- [ ] 添加用户认证
- [ ] 数据可视化增强（更多图表类型）
- [ ] 数据同步（多设备）
- [ ] 移动应用（React Native）
- [ ] AI 辅助建议
- [ ] 数据备份功能
- [ ] 导入数据功能

---

## 技术债务

1. **API 测试覆盖不足**
   - 当前主要依赖手动测试
   - 需要补充更多集成测试

2. **错误处理不够优雅**
   - 部分错误信息不够友好
   - 需要统一错误处理机制

3. **性能优化空间**
   - 大数据量下的查询优化
   - 前端渲染性能优化

---

## 参考资料

- [Next.js 文档](https://nextjs.org/docs)
- [Prisma 文档](https://www.prisma.io/docs)
- [Tailwind CSS 文档](https://tailwindcss.com/docs)
- [Vitest 文档](https://vitest.dev)
- [Playwright 文档](https://playwright.dev)
- [Zod 文档](https://zod.dev)

---

## 更新日志

### v0.3.0 (2026-03-19)
- ✨ 数据导出功能（JSON/CSV）
- 🎨 移动端优化
- 📊 统计卡片

### v0.2.0 (2026-03-19)
- ✨ 编辑/删除功能
- 🔍 时间筛选
- 📊 标签统计
- ⚠️ 低分预警

### v0.1.0 (2026-03-19)
- 🎉 初始版本
- ✨ 基础心情记录功能
- 📈 趋势图表
