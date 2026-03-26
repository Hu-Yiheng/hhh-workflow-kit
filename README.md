# HHH Workflow Kit

精简的 Java Spring Boot 企业级开发工作流插件，基于 [everything-claude-code](https://github.com/affaan-m/everything-claude-code) 精华提取。

## 特点

- ✅ **专注 Java** - 针对 Spring Boot + MyBatis + Redis + MySQL 技术栈优化
- ✅ **精简实用** - 只保留后端开发最核心的能力
- ✅ **开箱即用** - 安装即可使用，无需额外配置
- ✅ **企业级** - 适合团队协作的工作流和规范

## 技术栈支持

- Java 17+
- Spring Boot 3.x
- MyBatis
- Redis
- MySQL

## 安装

### 方法 1：直接从 GitHub 安装（推荐）

```bash
/plugin install Hu-Yiheng/hhh-workflow-kit
```

### 方法 2：通过插件市场安装

```bash
# 1. 添加插件市场
/plugin marketplace add Hu-Yiheng/hhh-workflow-kit

# 2. 安装插件
/plugin install hhh-workflow-kit@hhh-workflow-kit
```

## 核心功能

### 📋 命令（Commands）

| 命令 | 说明 |
|------|------|
| `/plan` | 制定实现计划 - 在编码前先规划步骤 |
| `/verify` | 验证实现结果 - 确保代码符合要求 |
| `/gradle-build` | 修复 Gradle 构建错误 |
| `/multi-backend` | 多后端服务协调 |

### 🤖 智能代理（Agents）

| 代理 | 说明 |
|------|------|
| `java-reviewer` | Java 代码审查专家 |
| `java-build-resolver` | Java 构建问题解决专家 |

### 📚 技能库（Skills）

| 技能 | 说明 |
|------|------|
| `search-first` | 搜索优先 - 改代码前先搜调用链 |
| `springboot-patterns` | Spring Boot 最佳实践模式 |
| `springboot-verification` | Spring Boot 验证规范 |
| `springboot-security` | Spring Boot 安全规范 |
| `springboot-tdd` | Spring Boot TDD 实践 |
| `java-coding-standards` | Java 编码标准 |
| `api-design` | API 设计规范 |
| `backend-patterns` | 后端架构模式 |
| `database-migrations` | 数据库迁移规范 |
| `postgres-patterns` | PostgreSQL 使用模式 |

### 📖 规则体系（Rules）

- **Common Rules** - 通用开发规范（代码风格、Git 工作流、测试、安全等）
- **Java Rules** - Java 专项规范（编码风格、测试、模式、Hooks、安全）

## 使用方式

### 1. 核心工作流

```
理解需求 → 制定计划 → TDD 实现 → 验证结果
```

### 2. 典型开发流程

```bash
# 1. 理解需求
"我要添加用户注册功能"

# 2. 制定计划
/plan

# 3. TDD 实现
# AI 会先写测试，再写实现代码

# 4. 验证结果
/verify
```

### 3. 使用智能代理

代理会在需要时自动调用，也可以手动指定：

```bash
# 代码审查
"请 java-reviewer 审查这段代码"

# 构建问题修复
"请 java-build-resolver 修复构建错误"
```

### 4. 利用技能库

技能会根据上下文自动激活，提供专业指导：

- 修改代码前，`search-first` 会提醒先搜索调用链
- 写 Spring Boot 代码时，`springboot-patterns` 会提供最佳实践
- 安全相关代码，`springboot-security` 会进行安全检查

## 关键原则

### ✅ 必须遵守

- **先计划后编码** - 使用 `/plan` 制定实现计划
- **TDD 开发** - 先写测试，再写实现
- **搜索优先** - 改代码前先搜索影响范围
- **最小改动** - 保持改动范围最小化
- **验证完整** - 使用 `/verify` 确保实现正确

### ❌ 禁止行为

- 跳过计划直接编码
- 跳过测试
- 不搜索就修改共享代码
- 硬编码敏感信息
- 静默扩大需求范围

## 示例场景

### 场景 1：添加新功能

```
用户: 添加用户注册功能

AI 执行流程:
1. 理解需求 - 确认注册流程、字段、验证规则
2. /plan - 制定实现计划（Controller → Service → Mapper → 测试）
3. TDD 实现 - 先写测试，再写代码
4. /verify - 验证测试覆盖率 ≥ 80%，功能正确
```

### 场景 2：修复构建错误

```
用户: Gradle 构建失败了

AI 执行流程:
1. 调用 java-build-resolver 代理
2. 分析错误日志
3. 定位问题根源
4. 提供修复方案
5. 验证构建成功
```

### 场景 3：代码审查

```
用户: 审查这段代码

AI 执行流程:
1. 调用 java-reviewer 代理
2. 检查代码风格、安全、性能
3. 提供改进建议
4. 标注严重程度（CRITICAL/HIGH/MEDIUM/LOW）
```

## 项目结构

```
hhh-workflow-kit/
├── .claude-plugin/
│   ├── plugin.json           # 插件配置
│   └── marketplace.json      # 市场配置
├── commands/                 # 命令目录
│   ├── plan.md
│   ├── verify.md
│   ├── gradle-build.md
│   └── multi-backend.md
├── agents/                   # 代理目录
│   ├── java-reviewer.md
│   └── java-build-resolver.md
├── skills/                   # 技能目录
│   ├── search-first/
│   ├── springboot-patterns/
│   ├── java-coding-standards/
│   └── ...
├── rules/                    # 规则目录
│   ├── common/              # 通用规则
│   └── java/                # Java 规则
├── AGENTS.md                # 项目总入口
└── README.md
```

## 更新插件

```bash
# 更新到最新版本
/plugin update hhh-workflow-kit
```

## 卸载插件

```bash
/plugin uninstall hhh-workflow-kit
```

## 参考来源

本插件基于 [everything-claude-code](https://github.com/affaan-m/everything-claude-code) 提取精华，专注于 Java Spring Boot 企业级开发场景。

## 贡献

欢迎提交 Issue 和 Pull Request。

## License

MIT
