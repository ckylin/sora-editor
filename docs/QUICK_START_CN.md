# Sora Editor 快速参考卡

> 常用命令和代码片段的快速查阅表

## 🚀 基础命令

### 构建相关
```bash
# 构建调试 APK（最常用）
./gradlew assembleDebug

# 构建并安装到设备
./gradlew installDebug

# 完整构建（包括所有模块）
./gradlew build

# 清理所有编译输出
./gradlew clean

# 强制重新下载依赖
./gradlew --refresh-dependencies clean build
```

### 测试相关
```bash
# 运行所有单元测试
./gradlew test

# 运行特定模块的测试
./gradlew :editor:test

# 运行特定测试类
./gradlew :app:test --tests io.github.rosemoe.sora.app.tests.paged.PagedEditSessionTest

# 输出详细日志
./gradlew test -i
```

### 质量检查
```bash
# 运行 Lint 检查
./gradlew lint

# 特定模块的 Lint
./gradlew :editor:lint

# 完整检查（Lint + 测试）
./gradlew check
```

### Git 相关
```bash
# 克隆项目（包含子模块）
git clone --recurse-submodules https://github.com/Rosemoe/sora-editor.git

# 创建功能分支
git checkout -b feature/my-feature

# 提交代码
git commit -m "feat: add new feature"

# 推送到 fork
git push origin feature/my-feature

# 更新本地 main 分支
git fetch origin
git rebase origin/main
```

---

## 📱 编码快速参考

### 在 Activity 中使用编辑器

```kotlin
import io.github.rosemoe.sora.widget.Editor
import io.github.rosemoe.sora.lang.java.JavaLanguage

class MainActivity : AppCompatActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)

        val editor = findViewById<Editor>(R.id.editor)

        // 设置文本
        editor.setText("public class Hello {}")

        // 设置语言（启用语法高亮）
        editor.setLanguage(JavaLanguage())

        // 监听文本变化
        editor.subscribeEvent(ContentChangeEvent::class.java) { event ->
            Log.d("Editor", "Text changed")
        }

        // 监听选择变化
        editor.subscribeEvent(SelectionChangeEvent::class.java) { event ->
            Log.d("Editor", "Selection changed: ${event.selection}")
        }
    }
}
```

### 布局文件配置

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical">

    <!-- 编辑器 -->
    <io.github.rosemoe.sora.widget.Editor
        android:id="@+id/editor"
        android:layout_width="match_parent"
        android:layout_height="0dp"
        android:layout_weight="1" />

    <!-- 工具栏示例 -->
    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:orientation="horizontal">

        <Button
            android:id="@+id/btn_undo"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="撤销" />

        <Button
            android:id="@+id/btn_redo"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="重做" />
    </LinearLayout>
</LinearLayout>
```

### 常用编辑器操作

```kotlin
val editor = findViewById<Editor>(R.id.editor)

// 获取文本
val allText = editor.text.toString()
val selectedText = editor.selectedText
val line = editor.text.getLine(0)

// 设置文本
editor.setText("new content")

// 插入文本
editor.insertText("inserted text")

// 删除选中文本
editor.deleteSelection()

// 删除指定行
editor.deleteLines(0, 5)  // 删除第 0-5 行

// 替换文本
editor.replaceSelection("replacement")

// 撤销/重做
editor.undo()
editor.redo()

// 搜索和替换
editor.find("searchText")
editor.findNext()
editor.replace("searchText", "replaceText")

// 光标操作
editor.setSelection(line, column)  // 设置光标位置
editor.selectAll()  // 全选
editor.clearSelection()  // 取消选择

// 获取光标位置
val cursorLine = editor.cursorPosition.line
val cursorColumn = editor.cursorPosition.column
```

### 编写单元测试

```kotlin
import io.github.rosemoe.sora.text.Content
import org.junit.Test
import kotlin.test.assertEquals

class MyEditorTest {

    @Test
    fun testTextOperation() {
        val content = Content("Hello World")

        // 验证文本
        assertEquals("Hello World", content.toString())

        // 验证行数
        assertEquals(1, content.lineCount)
    }

    @Test
    fun testMultilineText() {
        val content = Content("Line 1\nLine 2\nLine 3")

        // 验证行数
        assertEquals(3, content.lineCount)

        // 验证特定行
        assertEquals("Line 1", content.getLine(0))
        assertEquals("Line 2", content.getLine(1))
    }
}
```

---

## 📁 项目结构速查

### 模块位置

| 模块 | 路径 | 用途 |
|------|------|------|
| 核心编辑器 | `editor/` | 编辑器 UI、文本模型、事件系统 |
| TextMate 高亮 | `language-textmate/` | 基于 TextMate 的语法高亮 |
| Monarch 高亮 | `language-monarch/` | Monarch 格式的语法高亮 |
| TreeSitter | `language-treesitter/` | TreeSitter 增量解析 |
| Java 语言 | `language-java/` | Java 语言专用实现 |
| LSP 集成 | `editor-lsp/` | 语言服务器协议支持 |
| 本地正则 | `oniguruma-native/` | Oniguruma 正则引擎 JNI 封装 |
| 示例应用 | `app/` | 演示应用和测试用例 |
| 构建逻辑 | `build-logic/` | 自定义 Gradle 插件 |

### 关键类位置

| 类名 | 位置 | 功能 |
|------|------|------|
| `Editor` | `editor/src/main/kotlin/.../Editor.kt` | 编辑器主类 |
| `Content` | `editor/src/main/kotlin/.../text/Content.kt` | 文本缓冲区 |
| `Selection` | `editor/src/main/kotlin/.../text/Selection.kt` | 文本选择 |
| `Language` | `editor/src/main/kotlin/.../lang/Language.kt` | 语言接口 |
| `Lexer` | `editor/src/main/kotlin/.../lang/Lexer.kt` | 词法分析器 |
| `TextStyle` | `editor/src/main/kotlin/.../graphics/TextStyle.kt` | 文本样式 |

---

## 🔧 常见问题速解

### 问题：Gradle 同步失败

```bash
# 解决方案：清理缓存重试
./gradlew clean --refresh-dependencies
```

### 问题：编译找不到类

```bash
# 解决方案：确保包含了子模块
git submodule update --recursive
./gradlew sync
```

### 问题：测试失败

```bash
# 查看详细错误信息
./gradlew test -i

# 清理重新运行
./gradlew test --no-build-cache
```

### 问题：APK 构建太慢

```bash
# 使用 build cache
./gradlew assembleDebug --build-cache

# 并行构建（通常默认启用）
./gradlew assembleDebug --parallel
```

---

## 🎯 完整开发流程

### 1. 准备环境
```bash
# 克隆项目
git clone --recurse-submodules https://github.com/Rosemoe/sora-editor.git
cd sora-editor

# 在 Android Studio 打开项目
# File → Open → 选择项目目录

# 等待 Gradle 同步完成
```

### 2. 创建功能分支
```bash
git checkout -b feature/my-awesome-feature
```

### 3. 开发和测试
```bash
# 编写代码和测试
# 使用 Android Studio 编辑

# 运行测试
./gradlew test

# 构建调试版本
./gradlew assembleDebug

# 在设备上运行
./gradlew installDebug
```

### 4. 质量检查
```bash
# 运行所有检查
./gradlew check

# 修复任何问题
```

### 5. 提交和推送
```bash
# 提交代码
git add .
git commit -m "feat: add my awesome feature"

# 推送到 fork
git push origin feature/my-awesome-feature
```

### 6. 创建 Pull Request
- 访问 GitHub
- 点击 "New Pull Request"
- 填写 PR 描述
- 提交审核

---

## 📚 支持的语言

目前官方支持的语言：

- ✅ **Java** - 完整支持
- ✅ **Kotlin** - 通过 TextMate
- ✅ **Python** - 通过 TextMate
- ✅ **JavaScript/TypeScript** - 通过 TextMate
- ✅ **C/C++** - 通过 TextMate
- ✅ **HTML/XML** - 通过 TextMate
- ✅ **CSS/SCSS** - 通过 TextMate
- ✅ **SQL** - 通过 TextMate
- ✅ **Markdown** - 通过 TextMate

要添加新语言，请参考主文档中的"添加新的语言支持"章节。

---

## 🔗 快速链接

- **项目地址**：https://github.com/Rosemoe/sora-editor
- **官方文档**：https://project-sora.github.io/sora-editor-docs/
- **Issues**：https://github.com/Rosemoe/sora-editor/issues
- **Discussions**：https://github.com/Rosemoe/sora-editor/discussions
- **Maven Central**：https://central.sonatype.com/artifact/io.github.rosemoe/editor

---

## 💡 小贴士

1. **第一次构建会比较慢**：需要下载依赖，耐心等待
2. **使用 Gradle Daemon**：提高后续构建速度（通常默认启用）
3. **定期更新 Android Studio**：获得最新的工具支持
4. **使用 Profiler**：调试性能问题
5. **查看官方示例**：`app` 模块中有很多使用示例
6. **加入社区**：在 Issues 或 Discussions 中提问和交流

---

**更新时间**：2026-03-05
**版本**：1.0
