# Sora Editor 开发者指南（中文版）

> 本指南为想要贡献 Sora Editor 项目的 Android 新手开发者而编写。如果你是经验丰富的 Android 开发者，可以选择性阅读。

## 📋 目录

1. [项目概览](#项目概览)
2. [环境准备](#环境准备)
3. [项目结构](#项目结构)
4. [快速开始](#快速开始)
5. [核心模块详解](#核心模块详解)
6. [常见开发任务](#常见开发任务)
7. [测试指南](#测试指南)
8. [贡献指南](#贡献指南)

---

## 项目概览

### 什么是 Sora Editor？

Sora Editor 是一个功能强大的 **Android 文本编辑库**，提供：

- 📝 高效的文本编辑和文档管理
- 🎨 实时语法高亮（支持多种后端）
- ⚡ 增量语法分析（TreeSitter/TextMate）
- 🔧 LSP（语言服务协议）集成支持
- 🎯 代码补全、诊断、跳转定义等智能功能
- 🖥️ 撤销/重做、搜索替换、行号、Inlay Hints 等完整编辑功能

### 项目的核心价值

这是一个**生产级别的编辑库**，不仅可以在 Android App 中使用，还可以作为 SDK 集成到其他项目中。它已发布到 Maven Central，供全球 Android 开发者使用。

### 技术栈

| 技术 | 版本 | 用途 |
|------|------|------|
| Kotlin | 2.3 | 主要开发语言 |
| Java | 17 | 最低编译要求 |
| Gradle | 最新 | 构建系统（Kotlin DSL） |
| Android SDK | 35 | 编译版本 |
| Android Min SDK | 21 | 最低支持版本 |

---

## 环境准备

### 必要软件

在开始贡献前，请确保已安装：

| 工具 | 最低版本 | 安装链接 |
|------|---------|---------|
| **Android Studio** | 2024.1+ | https://developer.android.com/studio |
| **Java JDK** | 17+ | https://adoptium.net/ 或 OpenJDK 17 |
| **Git** | 最新 | https://git-scm.com |
| **Gradle** | 由 gradlew 管理 | 项目内置 |

### 验证环境

```bash
# 检查 Java 版本（必须是 17 或更高）
java -version

# 检查 Git 版本
git --version

# 打开 Android Studio，检查 SDK 版本
```

### 克隆项目

```bash
# 包含所有子模块的完整克隆
git clone --recurse-submodules https://github.com/Rosemoe/sora-editor.git

# 进入项目目录
cd sora-editor
```

> ⚠️ **重要**：务必使用 `--recurse-submodules` 参数，否则某些依赖文件无法正确下载。

### 在 Android Studio 中打开项目

1. 启动 Android Studio
2. 选择 **File → Open**
3. 导航到 `sora-editor` 目录并打开
4. 等待 Gradle 同步完成（第一次可能需要 5-10 分钟）

> **建议**：同步期间，系统会自动下载所有依赖。如果卡住，可以在 `gradle.properties` 中配置代理。

---

## 项目结构

### 顶级目录树

```
sora-editor/
├── .github/                    # GitHub Actions CI/CD 配置
│   └── workflows/
│       └── gradle.yml          # 自动构建和发布流程
│
├── build-logic/                # 自定义 Gradle 插件和构建约定
│   └── src/main/kotlin/        # 构建脚本（共享给所有模块）
│
├── editor/                     # 📌 核心编辑器模块（最重要！）
│   ├── src/main/kotlin/        # 编辑器实现代码
│   └── src/test/kotlin/        # 单元测试
│
├── language-textmate/          # TextMate 语法高亮支持
│   └── src/main/kotlin/
│
├── language-monarch/           # Monarch 语言包支持
│   └── src/main/kotlin/
│
├── language-treesitter/        # TreeSitter 解析器支持
│   └── src/main/kotlin/
│
├── language-java/              # Java 语言特定实现
│   └── src/main/kotlin/
│
├── editor-lsp/                 # LSP（语言服务协议）集成
│   └── src/main/kotlin/
│
├── oniguruma-native/           # 本地正则引擎（JNI）
│   ├── src/main/cpp/           # C++ 源代码
│   └── src/main/kotlin/        # Kotlin 包装
│
├── app/                        # 📌 示例应用（学习参考）
│   ├── src/main/kotlin/        # Android 应用代码
│   ├── src/main/res/           # UI 资源（布局、图片等）
│   └── src/test/kotlin/        # 应用层测试
│
├── gradle/                     # Gradle 包装脚本
├── .gitignore                  # Git 忽略配置
├── build.gradle.kts            # 顶级 Gradle 配置
├── settings.gradle.kts         # 模块定义
├── gradle.properties           # 全局编译参数
├── CLAUDE.md                   # Claude Code 开发指南
├── DEVELOPER_GUIDE_CN.md       # 本文件
└── LICENSE                     # LGPL v2.1 许可证
```

### 模块关系图

```
app（示例应用）
  ├─> editor（核心库）
  ├─> language-textmate（语法支持）
  ├─> language-monarch（语法支持）
  ├─> language-treesitter（语法支持）
  ├─> language-java（Java语言）
  └─> editor-lsp（LSP 支持）
        └─> editor（核心库）

editor（核心库）
  └─> oniguruma-native（正则引擎）
```

---

## 快速开始

### 1️⃣ 构建调试 APK

```bash
# 构建调试版本（最常用）
./gradlew assembleDebug

# 输出位置：app/build/outputs/apk/debug/app-debug.apk
```

> **预计时间**：首次 5-10 分钟（取决于网络和机器性能），后续增量构建 30 秒左右。

### 2️⃣ 在设备上运行

```bash
# 一键构建 + 安装到连接的设备或模拟器
./gradlew installDebug

# 也可以手动通过 Android Studio：
# Run → Run 'app' (Shift + F10)
```

### 3️⃣ 运行测试

```bash
# 运行全部单元测试
./gradlew test

# 只运行编辑器模块的测试
./gradlew :editor:test

# 运行特定的测试类
./gradlew :app:test --tests io.github.rosemoe.sora.app.tests.paged.PagedEditSessionTest
```

### 4️⃣ 代码质量检查

```bash
# 运行 Android Lint 检查（检查常见编码问题）
./gradlew lint

# 指定模块的 Lint
./gradlew :editor:lint
```

### 5️⃣ 清理构建

```bash
# 清理所有编译输出，释放磁盘空间
./gradlew clean

# 然后重新构建
./gradlew build
```

---

## 核心模块详解

### 📌 1. Editor 模块（核心库）

这是项目的**心脏**，包含所有编辑器的基础功能。

#### 位置
```
editor/src/main/kotlin/io/github/rosemoe/sora/
```

#### 主要子包

| 包名 | 功能 | 关键类 |
|------|------|--------|
| **widget** | 编辑器 UI 和渲染 | `Editor.kt`、`EditorView.kt`、`LineNumberPanel` |
| **text** | 文本缓冲区和文档模型 | `Content.kt`、`TextLine.kt`、`Cursor.kt` |
| **lang** | 语言支持接口 | `Language.kt`、`Lexer.kt`、`CodeAttribute.kt` |
| **event** | 事件系统 | `SelectionChangeEvent.kt`、`ContentChangeEvent.kt` |
| **graphics** | 渲染和装饰 | `InlayHint.kt`、`Span.kt`、`TextStyle.kt` |

#### 核心概念

**1. Content（文本缓冲区）**
```kotlin
// 表示编辑器中的所有文本
class Content {
    // 获取指定行的文本
    fun getLine(line: Int): CharSequence

    // 获取总行数
    val lineCount: Int

    // 替换文本范围
    fun replace(startLine: Int, startCol: Int, endLine: Int, endCol: Int, replacement: String)
}
```

**2. Selection（选择）**
```kotlin
class Selection {
    // 起始位置（行、列）
    var left: Int
    var leftColumn: Int

    // 结束位置
    var right: Int
    var rightColumn: Int

    // 是否有选区
    val isSelected: Boolean
}
```

**3. Language（语言支持）**
```kotlin
interface Language {
    // 获取词法分析器
    val lexer: Lexer

    // 获取语言名称
    val name: String
}
```

#### 常见任务

**获取编辑器中的文本**
```kotlin
val editor: Editor = findViewById(R.id.editor)

// 获取全部文本
val allText = editor.text.toString()

// 获取选中文本
val selectedText = editor.selectedText

// 获取指定行的文本
val line = editor.text.getLine(0)
```

**修改文本**
```kotlin
// 在光标位置插入文本
editor.insertText("Hello World")

// 替换选中文本
editor.replaceSelection("new text")

// 删除一行
editor.deleteLines(line, line)
```

**响应文本变化**
```kotlin
editor.subscribeEvent(ContentChangeEvent::class.java) { event ->
    // 文本已改变
    Log.d("Editor", "Text changed: ${event.changedText}")
}
```

### 🎨 2. Language 模块（语法高亮）

这些模块提供不同的**语法高亮后端**。

#### Language-TextMate

- **用途**：基于 TextMate 语法定义进行词法分析
- **特点**：支持 `.tmLanguage` 文件，生态丰富
- **位置**：`language-textmate/`

```kotlin
// 使用 TextMate 高亮 Java 代码
val language = JavaLanguage()  // 或其他 TextMate 支持的语言
editor.setLanguage(language)
```

#### Language-Monarch

- **用途**：轻量级客户端语言定义
- **特点**：JSON 配置，易于自定义
- **位置**：`language-monarch/`

#### Language-TreeSitter

- **用途**：增量解析和精确的语法分析
- **特点**：支持大文件、增量更新、精确的语法树
- **位置**：`language-treesitter/`

```kotlin
// 使用 TreeSitter 进行解析
val language = TreeSitterLanguage(/* language 参数 */)
editor.setLanguage(language)
```

#### Language-Java

- **用途**：Java 专用语言实现
- **特点**：优化的 Java 语法分析、补全支持
- **位置**：`language-java/`

### 🔧 3. LSP 模块（智能功能）

集成**语言服务器协议**，提供代码补全、诊断、跳转定义等功能。

#### 位置
```
editor-lsp/src/main/kotlin/io/github/rosemoe/sora/lsp/
```

#### 主要功能

| 功能 | 描述 |
|------|------|
| **Completion** | 代码补全建议 |
| **Diagnostics** | 错误/警告诊断 |
| **Hover** | 悬浮提示 |
| **Definition** | 跳转到定义 |
| **References** | 查找所有引用 |

#### 基础用法

```kotlin
// 连接到语言服务器
val lspClient = LSPClient(serverUri)
editor.attachLanguageServer(lspClient)

// 获取补全建议
lspClient.completion(file, line, column) { completions ->
    // 显示补全列表
}

// 显示诊断
lspClient.diagnostic(file) { diagnostics ->
    editor.showDiagnostics(diagnostics)
}
```

### ⚡ 4. Native 模块（正则引擎）

JNI 封装的高性能正则引擎。

#### 位置
```
oniguruma-native/
├── src/main/cpp/           # C++ 源代码
└── src/main/kotlin/        # Kotlin 接口
```

#### 用途

- 为 TextMate 等语法分析提供高性能正则匹配
- 用户一般**不直接使用**，由语法高亮模块内部调用

---

## 常见开发任务

### 任务 1️⃣：添加新的语言支持

**目标**：让编辑器支持新的编程语言（如 Go、Rust 等）

#### 步骤

**1. 创建新语言模块**
```bash
# 在项目根目录创建新模块
mkdir language-go
```

**2. 配置模块 `language-go/build.gradle.kts`**
```kotlin
plugins {
    id("android-library")
    id("kotlin-android")
}

android {
    namespace = "io.github.rosemoe.sora.language.go"
    compileSdk = 35

    defaultConfig {
        minSdk = 21
        targetSdk = 35
    }
}

dependencies {
    api(project(":editor"))
    // 其他依赖
}
```

**3. 实现 Language 接口**
```kotlin
// language-go/src/main/kotlin/io/github/rosemoe/sora/language/go/GoLanguage.kt
package io.github.rosemoe.sora.language.go

import io.github.rosemoe.sora.lang.Language
import io.github.rosemoe.sora.lang.Lexer

class GoLanguage : Language {
    override val name: String = "Go"

    override val lexer: Lexer = GoLexer()
}

class GoLexer : Lexer {
    // 实现词法分析逻辑
}
```

**4. 在 `settings.gradle.kts` 中注册模块**
```kotlin
include(":language-go")
```

**5. 在示例应用中使用**
```kotlin
// app/src/main/kotlin/MainActivity.kt
val goLanguage = GoLanguage()
editor.setLanguage(goLanguage)
```

### 任务 2️⃣：修复编辑器中的 Bug

**示例**：修复某个文本选择问题

#### 步骤

**1. 定位 Bug**
```bash
# 使用 Android Studio 的搜索功能查找相关代码
# 快捷键：Ctrl + Shift + F（全局搜索）
```

**2. 阅读代码**
```kotlin
// 假设 Bug 在 Selection.kt 中
// editor/src/main/kotlin/io/github/rosemoe/sora/text/Selection.kt
class Selection {
    var left: Int = 0
    var right: Int = 0

    fun normalize() {
        // Bug 可能在这里
        if (left > right) {
            val temp = left
            left = right
            right = temp
        }
    }
}
```

**3. 编写测试证明 Bug**
```kotlin
// editor/src/test/kotlin/io/github/rosemoe/sora/text/SelectionTest.kt
@Test
fun testNormalize() {
    val selection = Selection()
    selection.left = 10
    selection.right = 5
    selection.normalize()

    assertEquals(5, selection.left)
    assertEquals(10, selection.right)
}
```

**4. 修复代码**
```kotlin
fun normalize() {
    if (left > right) {
        val temp = left
        left = right
        right = temp
    }
}
```

**5. 运行测试验证**
```bash
./gradlew :editor:test --tests SelectionTest
```

### 任务 3️⃣：优化编辑器性能

**示例**：减少大文件编辑时的滞后感

#### 步骤

**1. 使用 Android Profiler 分析性能**
- 在 Android Studio 中运行应用
- 点击 **Profiler** 标签页
- 观察 CPU、内存、帧率数据

**2. 定位性能瓶颈**
```kotlin
// 例如，渲染函数可能过于频繁调用
class Editor {
    fun updateDisplay() {
        // ⚠️ 如果频繁调用，可能导致卡顿
        invalidate()
    }
}
```

**3. 应用优化策略**
```kotlin
// 使用防抖处理，减少渲染次数
private var updateDisplayScheduled = false

fun updateDisplay() {
    if (updateDisplayScheduled) return

    updateDisplayScheduled = true
    post {
        // 执行渲染
        invalidate()
        updateDisplayScheduled = false
    }
}
```

**4. 测试验证**
```bash
# 使用 Profiler 重新测试性能
# 观察 FPS 是否提升
```

### 任务 4️⃣：添加新的编辑器功能

**示例**：添加"括号匹配高亮"功能

#### 步骤

**1. 分析需求**
- 当光标在括号前/后时，高亮匹配的括号
- 支持 `()`、`[]`、`{}`、`<>`

**2. 设计实现**
```kotlin
// editor/src/main/kotlin/io/github/rosemoe/sora/graphics/BracketMatcher.kt
class BracketMatcher {
    fun findMatchingBracket(content: Content, line: Int, column: Int): Pair<Int, Int>? {
        val char = content.getLine(line).getOrNull(column) ?: return null

        return when (char) {
            '(' -> findClosingBracket(content, line, column, '(', ')')
            ')' -> findOpeningBracket(content, line, column, ')', '(')
            '[' -> findClosingBracket(content, line, column, '[', ']')
            // ... 其他括号
            else -> null
        }
    }

    private fun findClosingBracket(
        content: Content,
        startLine: Int,
        startColumn: Int,
        open: Char,
        close: Char
    ): Pair<Int, Int>? {
        // 从 startLine:startColumn 向后查找匹配的 close 括号
        // 返回 (line, column) 对
    }
}
```

**3. 集成到编辑器**
```kotlin
// 在编辑器中使用括号匹配
class Editor {
    private val bracketMatcher = BracketMatcher()

    override fun onCursorMove() {
        val cursor = cursorPosition
        val match = bracketMatcher.findMatchingBracket(
            content, cursor.line, cursor.column
        )

        if (match != null) {
            highlightBracketPair(cursor, match)
        }
    }
}
```

**4. 编写测试**
```kotlin
@Test
fun testFindMatchingBracket() {
    val content = Content("fun foo() { }")
    val matcher = BracketMatcher()

    val match = matcher.findMatchingBracket(content, 0, 8) // 光标在 ( 上
    assertEquals(10, match?.second)  // 应该找到 )
}
```

---

## 测试指南

### 单元测试基础

#### 测试文件位置

```
editor/src/test/kotlin/io/github/rosemoe/sora/...
app/src/test/kotlin/io/github/rosemoe/sora/app/...
```

#### 编写简单的测试

```kotlin
// editor/src/test/kotlin/io/github/rosemoe/sora/text/ContentTest.kt
package io.github.rosemoe.sora.text

import org.junit.Test
import kotlin.test.assertEquals

class ContentTest {

    @Test
    fun testGetLine() {
        val content = Content("Hello\nWorld")

        // 验证第一行
        assertEquals("Hello", content.getLine(0))
        // 验证第二行
        assertEquals("World", content.getLine(1))
    }

    @Test
    fun testReplaceText() {
        val content = Content("Hello World")

        // 替换 "Hello" 为 "Hi"
        content.replace(0, 0, 0, 5, "Hi")

        assertEquals("Hi World", content.toString())
    }
}
```

#### 运行测试

```bash
# 运行所有测试
./gradlew test

# 运行特定测试类
./gradlew test --tests ContentTest

# 运行特定测试方法
./gradlew test --tests ContentTest.testGetLine
```

### Android 集成测试

对于需要 Android 环境的测试（如 UI 相关），使用 `androidTest`：

```kotlin
// app/src/androidTest/kotlin/io/github/rosemoe/sora/app/EditorUITest.kt
@RunWith(AndroidJUnit4::class)
class EditorUITest {

    @get:Rule
    val activityRule = ActivityScenarioRule(MainActivity::class.java)

    @Test
    fun testEditorLoads() {
        val editor = onView(withId(R.id.editor))
        editor.check(matches(isDisplayed()))
    }
}
```

运行集成测试：
```bash
./gradlew connectedAndroidTest
```

### 常用断言

```kotlin
import kotlin.test.*

// 相等性检查
assertEquals(expected, actual)
assertNotEquals(expected, actual)

// 布尔检查
assertTrue(condition)
assertFalse(condition)

// 对象检查
assertNull(value)
assertNotNull(value)

// 异常检查
assertThrows<ExceptionType> {
    // 应该抛出异常的代码
}
```

---

## 贡献指南

### 贡献前的准备

1. **Fork 项目**到你的 GitHub 账户
2. **Clone** 你的 fork：
   ```bash
   git clone https://github.com/YOUR_USERNAME/sora-editor.git
   cd sora-editor
   ```
3. **创建功能分支**：
   ```bash
   git checkout -b feature/my-feature
   ```

### 开发工作流

#### 1. 编码规范

- **Kotlin 风格**：遵循官方 Kotlin 风格指南
- **命名**：驼峰命名法（camelCase），常量使用大写加下划线（CONSTANT_NAME）
- **注释**：为公共 API 和复杂逻辑添加注释

```kotlin
/**
 * 计算两个点之间的距离
 *
 * @param p1 第一个点
 * @param p2 第二个点
 * @return 两点间的欧氏距离
 */
fun distance(p1: Point, p2: Point): Double {
    val dx = p2.x - p1.x
    val dy = p2.y - p1.y
    return sqrt(dx * dx + dy * dy)
}
```

#### 2. 提交信息规范

使用清晰的提交信息：

```bash
# 好的提交信息
git commit -m "feat: add bracket matching highlight support"
git commit -m "fix: resolve issue with cursor position in large files"
git commit -m "docs: update developer guide"

# 不好的提交信息
git commit -m "update"
git commit -m "fix bug"
```

**提交类型前缀**：
- `feat:` - 新功能
- `fix:` - 错误修复
- `docs:` - 文档更新
- `style:` - 代码格式调整（不影响功能）
- `refactor:` - 代码重构
- `test:` - 添加或修改测试
- `perf:` - 性能优化

#### 3. 提交前检查清单

在提交 PR 前，请运行：

```bash
# 1. 清理和重新构建
./gradlew clean build

# 2. 运行所有测试
./gradlew test

# 3. 运行 Lint 检查
./gradlew lint

# 4. 验证示例应用是否能正常运行
./gradlew :app:assembleDebug
```

### 创建 Pull Request

#### PR 前的准备

```bash
# 获取最新的主分支代码
git fetch origin
git rebase origin/main

# 推送到你的 fork
git push origin feature/my-feature
```

#### PR 描述模板

在 GitHub 创建 PR 时，使用以下格式描述：

```markdown
## 描述
简要描述这个 PR 的目的。

## 改动类型
- [ ] Bug 修复
- [ ] 新功能
- [ ] 性能优化
- [ ] 文档更新
- [ ] 其他

## 相关问题
关闭 #123（如果有的话）

## 测试
- [ ] 已在本地验证
- [ ] 已运行单元测试
- [ ] 已运行集成测试
- [ ] 已运行 Lint 检查

## 截图（如果涉及 UI 改动）
[添加截图]

## 其他信息
[任何其他相关信息]
```

### 代码审查

项目维护者会审查你的 PR。可能需要的改进：

1. **代码质量**：是否遵循风格指南？
2. **功能完整性**：是否实现了所有要求的功能？
3. **测试覆盖**：是否有相应的测试？
4. **文档**：是否有必要的文档说明？
5. **性能**：是否有性能问题？

---

## 常见问题 (FAQ)

### Q1: 构建时出现 "Unable to find a matching variant"

**原因**：Gradle 依赖版本不兼容

**解决**：
```bash
# 删除本地缓存
rm -rf ~/.gradle/caches

# 重新同步
./gradlew clean sync
```

### Q2: 编译失败，提示 "Cannot find class"

**原因**：缺少依赖或未正确同步

**解决**：
```bash
# 1. 确保克隆时包含子模块
git clone --recurse-submodules https://github.com/Rosemoe/sora-editor.git

# 2. 更新子模块
git submodule update --recursive

# 3. 重新同步 Gradle
./gradlew --refresh-dependencies
```

### Q3: 如何在自己的 App 中使用 Sora Editor？

**步骤**：

1. **添加依赖** (`build.gradle.kts`)
```kotlin
dependencies {
    implementation("io.github.rosemoe:editor:0.x.x")  // 检查最新版本
}
```

2. **在布局中添加编辑器** (`activity_main.xml`)
```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent">

    <io.github.rosemoe.sora.widget.Editor
        android:id="@+id/editor"
        android:layout_width="match_parent"
        android:layout_height="match_parent" />
</LinearLayout>
```

3. **在代码中使用** (`MainActivity.kt`)
```kotlin
import io.github.rosemoe.sora.widget.Editor
import io.github.rosemoe.sora.lang.java.JavaLanguage

class MainActivity : AppCompatActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)

        val editor = findViewById<Editor>(R.id.editor)
        editor.setText("public class Hello {}")
        editor.setLanguage(JavaLanguage())
    }
}
```

### Q4: 如何调试编辑器问题？

**方法**：

1. **在 Android Studio 中设置断点**
   - 点击代码行号左边
   - 运行 app 时会在断点处暂停

2. **使用 Logcat 查看日志**
   - Android Studio 底部 → Logcat
   - 过滤：`Tag: "sora"` 或 `"editor"`

3. **使用调试器**
   - 在变量上右键 → Evaluate Expression
   - 检查对象的当前状态

---

## 学习资源

### 官方文档

- **项目主页**：https://project-sora.github.io/sora-editor-docs/
- **快速开始**：https://project-sora.github.io/sora-editor-docs/guide/getting-started
- **API 文档**：https://project-sora.github.io/sora-editor-docs/reference/

### Android 开发基础

- **Android 官方文档**：https://developer.android.com/docs
- **Kotlin 官方文档**：https://kotlinlang.org/docs/
- **Gradle 文档**：https://docs.gradle.org/

### 社区交流

- **Telegram 群组**：https://t.me/rosemoe_code_editor
- **QQ 群**：734652304
- **GitHub Issues**：https://github.com/Rosemoe/sora-editor/issues

---

## 项目许可证

Sora Editor 采用 **LGPL v2.1** 许可证。详见 [LICENSE](../LICENSE) 文件。

在贡献代码前，请确保了解并同意此许可证的条款。

---

## 致谢

感谢所有为 Sora Editor 做出贡献的开发者！

祝你开发愉快！🚀

---

**最后更新**：2026-03-05
**维护者**：Rosemoe 和社区贡献者
**联系方式**：GitHub Issues 或官方社区
