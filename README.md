# HHH Workflow Kit

精简的 Java Spring Boot 企业级开发工作流系统。

## 特点

- ✅ **精简** - 只有 4 个核心文件
- ✅ **入门门槛低** - 清晰的规则和示例
- ✅ **Java Spring Boot 专用** - 针对技术栈优化
- ✅ **企业级** - 适合团队协作

## 技术栈

- Java 17+
- Spring Boot 3.x
- MyBatis
- Redis
- MySQL

## 核心工作流

```
理解 → 计划 → 实现 → 验证
```

每个开发任务都遵循这个 4 步流程。

## 文件结构

```
.claude/
├── CLAUDE.md              # 主配置入口
└── rules/
    ├── workflow.md        # 工作流规则（4步流程 + TDD）
    ├── java-spring.md     # Java Spring Boot 编码规范
    └── team-guide.md      # 团队协作指南（反模式 + 检查清单）
```

## 使用方式

### 1. 自动生效

将此项目放在你的 Java Spring Boot 项目根目录，Claude Code 会自动加载规则。

### 2. 开发流程

```
1. 理解需求 → 明确要做什么
2. 制定计划 → 分步骤规划
3. TDD 实现 → 先写测试，再写代码
4. 验证结果 → 运行测试，确保正确
```

### 3. 关键原则

- ❌ 不要跳过计划直接编码
- ❌ 不要跳过测试
- ✅ 遵循 TDD（测试驱动开发）
- ✅ 保持改动最小化
- ✅ 重大变更前获得确认

## 规则说明

### workflow.md
- 4 步工作流程
- TDD 强制要求
- Git 提交规范

### java-spring.md
- 代码风格和命名规范
- Spring Boot 最佳实践
- MyBatis/Redis/MySQL 使用规范
- 错误处理和日志规范
- 安全和测试要求

### team-guide.md
- 常见错误和反模式
- 危险区域警示
- 代码审查清单
- 团队协作指南

## 快速开始

1. 克隆此项目到你的 Java Spring Boot 项目根目录
2. 使用 Claude Code 开始开发
3. AI 会自动遵循这些规则

## 示例

**用户**: 我要添加一个用户注册功能

**AI 会**:
1. 理解需求（确认注册流程、字段、验证规则）
2. 制定计划（Controller → Service → Mapper → 测试）
3. TDD 实现（先写测试，再写代码）
4. 验证结果（运行测试，确保覆盖率 ≥ 80%）

## 参考来源

基于 [everything-claude-code](https://github.com/affaan-m/everything-claude-code) 提取精华，专为 Java Spring Boot 企业级项目优化。

## License

MIT
