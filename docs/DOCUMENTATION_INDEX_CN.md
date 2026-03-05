# Sora Editor 开发者文档索引

> 完整的中文开发者文档导航

## 📖 文档概览

我们为 Android 新手开发者准备了完整的中文文档体系。请根据你的需求选择合适的文档阅读。

```
开始这里 ⭐
    ↓
[快速开始]
    ↓
[完整开发指南] ← 大多数开发者的首选
    ↓
[架构深度分析] ← 如果你想深入理解设计
    ↓
[贡献工作流] ← 准备提交 PR 时阅读
```

---

## 🚀 文档导航表

### 新手入门路径

如果你是 Android 新手，按照以下顺序阅读：

| # | 文档 | 阅读时间 | 目的 |
|---|------|---------|------|
| 1 | **QUICK_START_CN.md** | 15 分钟 | 了解基本命令和快速参考 |
| 2 | **DEVELOPER_GUIDE_CN.md** | 1-2 小时 | 学习项目结构和基础开发 |
| 3 | **ARCHITECTURE_CN.md** | 1-2 小时 | 理解编辑器的设计原理 |
| 4 | **CONTRIBUTION_WORKFLOW_CN.md** | 30 分钟 | 学习如何贡献代码 |

### 经验开发者快速路径

如果你有 Android 经验：

| 文档 | 用途 |
|------|------|
| QUICK_START_CN.md | 快速掌握命令 |
| ARCHITECTURE_CN.md | 了解设计架构 |
| CONTRIBUTION_WORKFLOW_CN.md | 贡献指南 |

### 查询和参考

需要查找特定信息时：

| 问题 | 文档 | 位置 |
|------|------|------|
| 项目结构是什么？ | DEVELOPER_GUIDE_CN.md | [项目结构](#项目结构) |
| 如何构建项目？ | QUICK_START_CN.md | [基础命令](#基础命令) |
| 编辑器工作原理？ | ARCHITECTURE_CN.md | [整体架构](#整体架构) |
| 如何修复 Bug？ | DEVELOPER_GUIDE_CN.md | [常见开发任务](#常见开发任务) |
| 如何提交 PR？ | CONTRIBUTION_WORKFLOW_CN.md | [完整贡献流程](#完整贡献流程) |
| 如何添加新语言？ | DEVELOPER_GUIDE_CN.md | [任务 1](#任务1添加新的语言支持) |
| 遇到问题如何解决？ | CONTRIBUTION_WORKFLOW_CN.md | [常见问题排查](#常见问题排查) |

---

## 📚 各文档详细说明

### 📄 1. QUICK_START_CN.md
**快速参考卡**

**适合人群**：所有开发者

**包含内容**：
- 常用 Gradle 命令速查
- 编码快速参考（代码片段）
- 项目结构速查表
- 常见问题快速解
- 完整开发流程概览

**何时使用**：
- 需要快速查找命令
- 需要代码示例
- 快速问题解决

**阅读时间**：15-30 分钟

**关键章节**：
- 🚀 基础命令
- 📱 编码快速参考
- 🔧 常见问题速解

### 📄 2. DEVELOPER_GUIDE_CN.md
**完整开发者指南**

**适合人群**：想要贡献代码的开发者

**包含内容**：
- 项目概览和技术栈
- 环境准备和项目克隆
- 详细的项目结构说明
- 快速开始（构建、运行、测试）
- 核心模块详解（Editor、Language、LSP、Native）
- 常见开发任务（示例和代码）
- 单元测试指南
- 贡献指南基础

**何时使用**：
- 初次接触项目
- 想要理解项目各部分
- 需要学习如何开发功能

**阅读时间**：1-2 小时

**关键章节**：
- ✅ 环境准备
- 📁 项目结构
- 📌 核心模块详解
- 🔨 常见开发任务

### 📄 3. ARCHITECTURE_CN.md
**架构深度分析**

**适合人群**：想要深入理解设计的开发者

**包含内容**：
- 整体架构分层设计
- 模块依赖关系
- 核心设计模式（观察者、策略、工厂等）
- 数据流分析
- 关键组件详解
- 渲染流程
- 性能优化策略

**何时使用**：
- 需要理解编辑器为什么这样设计
- 想要实现复杂功能
- 需要性能优化
- 参与架构改进

**阅读时间**：1-2 小时

**关键章节**：
- 🏗️ 整体架构
- 🎯 核心设计模式
- 📊 数据流
- 🖌️ 渲染流程
- ⚡ 性能优化

### 📄 4. CONTRIBUTION_WORKFLOW_CN.md
**贡献者工作流指南**

**适合人群**：准备贡献代码的开发者

**包含内容**：
- Fork 和克隆步骤
- 完整贡献流程（7 个 Phase）
- 不同类型贡献指南（修复 Bug、添加功能、改进文档等）
- 常见问题排查
- 与维护者沟通技巧
- 快速参考检查清单

**何时使用**：
- 准备创建 Pull Request
- 不确定工作流程
- 遇到 Git 问题
- 想学习最佳实践

**阅读时间**：30-45 分钟

**关键章节**：
- 🔄 完整贡献流程
- 🐛 不同类型贡献
- ❌ 常见问题排查
- 💬 与维护者沟通

---

## 🎯 学习路线图

### 路线 1️⃣：我想快速上手编辑器

```
Step 1: 读 QUICK_START_CN.md（15 分钟）
        └─ 了解基本命令和项目结构

Step 2: 读 DEVELOPER_GUIDE_CN.md 的快速开始部分（30 分钟）
        └─ 学会构建、运行、测试

Step 3: 在示例应用中修改代码（1 小时）
        └─ 了解实际使用

完成时间：约 2 小时
```

### 路线 2️⃣：我想添加新功能

```
Step 1: 读 QUICK_START_CN.md（15 分钟）

Step 2: 读 DEVELOPER_GUIDE_CN.md 的相关章节（1 小时）
        └─ 核心模块、常见任务

Step 3: 读 ARCHITECTURE_CN.md（1 小时）
        └─ 理解设计，找到合适的修改点

Step 4: 编写代码和测试（2-4 小时）
        └─ 实现功能

Step 5: 读 CONTRIBUTION_WORKFLOW_CN.md（30 分钟）
        └─ 学习如何提交 PR

Step 6: 创建和审查 PR（1-2 小时）
        └─ 与维护者沟通

完成时间：约 6-9 小时
```

### 路线 3️⃣：我想深入理解编辑器设计

```
Step 1: 读 DEVELOPER_GUIDE_CN.md 完整版（2 小时）

Step 2: 读 ARCHITECTURE_CN.md 完整版（2 小时）
        └─ 重点理解：
           - 分层设计
           - 设计模式
           - 数据流

Step 3: 阅读源代码（2-4 小时）
        └─ 重点看：
           - Editor.kt（主类）
           - Content.kt（文本模型）
           - Lexer（词法分析）
           - 渲染逻辑

Step 4: 在 Profiler 中运行代码（1 小时）
        └─ 实际观察性能

Step 5: 修改代码实验（2-4 小时）
        └─ 尝试改进性能或功能

完成时间：约 9-13 小时
```

---

## 💡 文档使用技巧

### 1. 快速搜索

在浏览器中打开 Markdown 文件后，使用 `Ctrl+F`（或 `Cmd+F`）搜索：

**常用关键词**：
- `编译失败` - 构建问题
- `测试` - 测试相关
- `性能` - 性能优化
- `Git` - 版本控制
- `缺失` - 依赖问题
- `示例` - 代码示例

### 2. 使用目录快速导航

所有文档都有 **目录（Table of Contents）**，点击可快速跳转。

### 3. 相关链接

文档之间有相互链接，例如：
- "详见 ARCHITECTURE_CN.md [渲染流程](#渲染流程)"

### 4. 代码示例复制

所有代码块都可以直接复制，贴到你的 IDE 中使用。

---

## 🔗 相关官方资源

| 资源 | 链接 | 用途 |
|------|------|------|
| **官方主页** | https://project-sora.github.io/sora-editor-docs/ | 项目概览 |
| **快速开始** | https://project-sora.github.io/sora-editor-docs/guide/getting-started | 官方快速开始 |
| **API 文档** | https://project-sora.github.io/sora-editor-docs/reference/ | API 详细文档 |
| **GitHub Repo** | https://github.com/Rosemoe/sora-editor | 源代码 |
| **Issues** | https://github.com/Rosemoe/sora-editor/issues | 问题跟踪 |
| **Discussions** | https://github.com/Rosemoe/sora-editor/discussions | 社区讨论 |
| **Maven Central** | https://central.sonatype.com/artifact/io.github.rosemoe/editor | 依赖库 |

---

## ❓ 常见问题

### Q: 我应该从哪个文档开始？

**A**: 如果你是 Android 新手，按顺序读：
1. QUICK_START_CN.md（15 分钟了解基础）
2. DEVELOPER_GUIDE_CN.md（学习具体细节）
3. 其他文档（按需查阅）

### Q: 文档多久更新一次？

**A**:
- 随着代码改变而更新
- 重大功能完成后更新
- 社区反馈后改进

### Q: 我发现文档有错误，应该怎么办？

**A**:
- 在 GitHub Issues 中报告
- 或直接提交 PR 修复
- 提前感谢你的贡献！

### Q: 能否提供英文版本？

**A**:
- 这份文档是中文版本
- 英文版本可能即将推出
- 社区欢迎翻译贡献

### Q: 文档提到的代码在哪里？

**A**: 所有提及的源文件位置都用 `file_path:line_number` 格式标记，例如：
- `editor:src/main/kotlin/.../Editor.kt:100`

在 Android Studio 中，使用 `Ctrl+G` 快速跳转到指定行。

---

## 📞 获取帮助

### 如果你卡住了

1. **查看文档**
   - 先在文档中搜索关键词
   - 查看"常见问题"部分

2. **查看 GitHub Issues**
   - 可能已经有人遇到相同问题
   - 搜索关键词

3. **提问**
   - 在 GitHub Discussions 中提问
   - 加入社区群组（Telegram/QQ）

4. **寻求帮助**
   - 提问时附带完整错误信息
   - 说明你已经尝试过的解决方案

### 联系方式

- **GitHub Issues**: https://github.com/Rosemoe/sora-editor/issues
- **GitHub Discussions**: https://github.com/Rosemoe/sora-editor/discussions
- **Telegram 群**: https://t.me/rosemoe_code_editor
- **QQ 群**: 734652304

---

## 📋 文档维护信息

| 项 | 信息 |
|---|------|
| **文档版本** | 1.0 |
| **更新时间** | 2026-03-05 |
| **维护者** | Sora Editor 社区 |
| **语言** | 中文 |
| **适用版本** | v0.x.x+ |
| **许可证** | LGPL v2.1 |

---

## 🎓 进阶资源

### 想要学更多？

- **Kotlin 官方文档**: https://kotlinlang.org/docs/
- **Android 官方文档**: https://developer.android.com/docs
- **Gradle 构建指南**: https://docs.gradle.org/
- **Git 完全指南**: https://git-scm.com/book/en/v2

### 相关项目

- **TreeSitter**: 增量解析器 https://tree-sitter.github.io/
- **TextMate**: 语法定义标准 https://macromates.com/textmate/manual/
- **Oniguruma**: 正则引擎 https://github.com/kkos/oniguruma
- **LSP**: 语言服务协议 https://microsoft.github.io/language-server-protocol/

---

## 🙏 致谢

感谢以下人员和社区：

- **Rosemoe**: 项目创始人和主要维护者
- **社区贡献者**: 众多代码贡献者
- **文档贡献者**: 帮助完善中文文档的开发者

---

## 📝 文档改进建议

你有改进建议吗？

1. 找到相关文档
2. 点击"编辑"（如果在 GitHub 上）
3. 提交你的建议
4. 或在 Issues 中反馈

你的反馈对我们很重要！

---

**开始阅读**: [👉 快速开始指南](QUICK_START_CN.md)

**下一步**: [👉 完整开发指南](DEVELOPER_GUIDE_CN.md)

---

祝你开发愉快！🚀

*本文档体系为 Android 新手开发者精心设计，旨在降低学习成本，加快贡献速度。*
