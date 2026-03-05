# Sora Editor 贡献者工作流指南

> 完整的、分步骤的贡献流程，让你快速上手

## 📋 目录

1. [准备工作](#准备工作)
2. [完整贡献流程](#完整贡献流程)
3. [不同类型贡献](#不同类型贡献)
4. [常见问题排查](#常见问题排查)
5. [与维护者沟通](#与维护者沟通)

---

## 准备工作

### 第一步：Fork 项目

1. 访问 https://github.com/Rosemoe/sora-editor
2. 点击右上角的 **Fork** 按钮
3. 选择要 Fork 到的账户（通常是你自己的账户）

```
GitHub 页面：
┌─────────────────────────────────────┐
│  Rosemoe/sora-editor                │
│                                     │
│  [Star] [Fork] [Watch] [Sponsor]    │
│                        ↑ 点这里     │
└─────────────────────────────────────┘

↓

└─→ 你的账户/sora-editor (fork 副本)
```

### 第二步：克隆你的 Fork

```bash
# 使用你的 fork URL（不是原项目）
git clone --recurse-submodules https://github.com/YOUR_USERNAME/sora-editor.git

cd sora-editor

# 添加上游仓库引用（用于同步最新代码）
git remote add upstream https://github.com/Rosemoe/sora-editor.git

# 验证
git remote -v
# 应该看到：
# origin    https://github.com/YOUR_USERNAME/sora-editor.git (fetch)
# origin    https://github.com/YOUR_USERNAME/sora-editor.git (push)
# upstream  https://github.com/Rosemoe/sora-editor.git (fetch)
# upstream  https://github.com/Rosemoe/sora-editor.git (push)
```

### 第三步：配置本地开发环境

```bash
# 在项目根目录配置 Git
git config user.name "Your Name"
git config user.email "your.email@example.com"

# 配置 git hook（可选但推荐）
# 这会在提交前自动运行检查
```

---

## 完整贡献流程

### 🔄 标准工作流

#### Phase 1: 准备新功能分支

```bash
# 1. 确保本地 main 分支是最新的
git fetch upstream
git checkout main
git rebase upstream/main

# 2. 创建功能分支（分支名应该有意义）
git checkout -b feature/add-vim-mode
# 或
git checkout -b fix/selection-bug
# 或
git checkout -b docs/update-readme
```

**分支命名规范**：
- `feature/*` - 新功能
- `fix/*` - 错误修复
- `docs/*` - 文档更新
- `refactor/*` - 代码重构
- `test/*` - 测试相关
- `chore/*` - 维护性工作

#### Phase 2: 开发新功能

```bash
# 在分支上进行开发
# 使用 Android Studio 编辑代码

# 定期提交（小粒度提交便于审查）
git add .
git commit -m "feat: implement vim normal mode keybindings"

# 如果需要更新分支（同步上游最新代码）
git fetch upstream
git rebase upstream/main
```

**提交信息规范**：

```
格式：<type>(<scope>): <subject>

type 可以是：
  feat:       新功能
  fix:        错误修复
  docs:       文档
  style:      代码格式（不影响功能）
  refactor:   重构代码
  test:       测试
  perf:       性能优化
  chore:      构建/工程化任务

scope（可选）：
  editor, lang, lsp, app, etc.

subject（必需）：
  - 用祈使句形式
  - 不要以大写字母开头
  - 不要以句号结尾
  - 字数不超过 50

示例：
  feat(lang): add support for Rust syntax highlighting
  fix(editor): resolve cursor position bug in large files
  docs: update development guide with examples
  test(lsp): add unit tests for completion provider
```

**好的提交消息示例**：
```
feat(editor): implement sticky header for method definitions

- Use TreeSitter to identify method boundaries
- Only show header when scrolling past method start
- Support customizable header styling
```

**不好的提交消息示例**：
```
Fix bug
Update code
Various improvements
```

#### Phase 3: 本地验证

在提交 PR 前，运行完整的验证流程：

```bash
# 1. 清理旧的编译产物
./gradlew clean

# 2. 运行所有单元测试
./gradlew test

# 3. 运行 Lint 检查
./gradlew lint

# 4. 构建示例应用
./gradlew :app:assembleDebug

# 5. 可选：在模拟器或设备上测试
./gradlew installDebug
```

**验证清单**：
- ✅ 所有测试通过
- ✅ 没有 Lint 警告
- ✅ APK 构建成功
- ✅ 功能在设备上正常工作
- ✅ 没有引入新的编译警告

#### Phase 4: 推送到 Fork

```bash
# 推送本地分支到 fork（使用 -u 设置上游跟踪）
git push -u origin feature/add-vim-mode
```

如果已经推过一次，后续可以直接：
```bash
git push
```

#### Phase 5: 创建 Pull Request

1. **在 GitHub 上**
   - 访问 https://github.com/YOUR_USERNAME/sora-editor
   - 应该会看到一个横幅提示你有新推送
   - 点击 **Compare & pull request** 按钮

2. **或手动创建**
   - 点击 **Pull requests** 标签
   - 点击 **New pull request** 按钮
   - 选择 `base: main` (Rosemoe/sora-editor) 和 `compare: feature/xxx` (你的 fork)
   - 点击 **Create pull request**

3. **填写 PR 详情**

使用以下模板：

```markdown
## 描述
简要说明这个 PR 的目的和改动。

一句话总结：实现 Vim 模式的法线命令支持（支持 h/j/k/l/w/b 等）。

## 改动类型
- [x] 新功能
- [ ] 错误修复
- [ ] 性能优化
- [ ] 文档更新
- [ ] 其他

## 相关问题
关闭 #123（如果有的话）
参考 #456

## 设计决策
解释你为什么选择这种实现方式，而不是其他方式。

### 为什么不用 X 方案？
X 方案的问题是...

## 测试
- [x] 已在 OnePlus 8T (Android 12) 上测试
- [x] 已在 Emulator (API 30) 上测试
- [x] 已添加单元测试
- [x] 所有现有测试仍然通过

## 性能影响
- 不会增加包体积
- 启动时间无变化
- 内存占用无增加

## 注意事项
- 本 PR 要求更新文档
- 可能影响向后兼容性

## 截图或录屏（如涉及 UI 改动）
[粘贴截图或上传录屏]

## 检查清单
- [x] 我已经阅读了贡献指南
- [x] 我的代码遵循项目的代码风格
- [x] 我已经运行了本地测试
- [x] 我添加了必要的测试
- [x] 我更新了相关文档
```

#### Phase 6: 代码审查

项目维护者会审查你的代码。可能会：

- 📝 **请求改动**：可能需要修改代码
  ```
  维护者评论：
  > 这里应该使用 suspend 函数而不是阻塞调用
  > 请查看 xxx 的实现以参考最佳实践
  ```

  你的操作：
  ```bash
  # 修改代码
  # 重新提交
  git add .
  git commit -m "refactor: use coroutine instead of blocking call"
  git push
  # PR 会自动更新
  ```

- ✅ **批准**：你的代码已经准备好合并了

#### Phase 7: 合并

维护者会将你的 PR 合并到主分支。一旦合并：

```bash
# 清理本地分支
git checkout main
git pull upstream main
git branch -d feature/add-vim-mode
```

---

## 不同类型贡献

### 🐛 修复 Bug

#### 任务：修复光标选择 Bug

**步骤**：

1. **找到 Issue**
   - GitHub Issues 中找到描述清楚的 bug
   - 确保没有人已经在修复

2. **创建分支**
   ```bash
   git checkout -b fix/cursor-selection-bug
   ```

3. **重现 Bug**
   - 在本地复现问题
   - 编写失败的测试用例
   ```kotlin
   @Test
   fun testSelectionWithCtrlShiftArrow() {
       // 这个测试应该失败（因为 bug 还没修）
       val editor = createEditor()
       editor.setText("Hello World")
       editor.cursorPosition = Position(0, 0)

       // 模拟 Ctrl+Shift+Right（选择单词）
       editor.selectWord()

       assertEquals(5, editor.selection.right)  // 应该选中 "Hello"
   }
   ```

4. **修复代码**
   ```kotlin
   // editor/src/main/kotlin/.../widget/selection/SelectionHandler.kt
   fun selectWord() {
       // 修复逻辑
   }
   ```

5. **验证测试通过**
   ```bash
   ./gradlew test --tests SelectionHandlerTest.testSelectionWithCtrlShiftArrow
   ```

6. **提交 PR**
   ```bash
   git commit -m "fix(editor): correct cursor selection with Ctrl+Shift+Arrow"
   ```

### ✨ 添加新功能

#### 任务：添加括号匹配高亮

**步骤**：

1. **设计阶段**
   - 在 Issue 中讨论设计
   - 获得维护者的反馈

2. **创建分支**
   ```bash
   git checkout -b feature/bracket-matching-highlight
   ```

3. **实现功能**
   ```kotlin
   // editor/src/main/kotlin/.../graphics/BracketMatcher.kt
   class BracketMatcher {
       fun findMatchingBracket(line: Int, column: Int): Pair<Int, Int>? {
           // 实现
       }

       fun highlightBracketPair(canvas: Canvas, pair: Pair<Int, Int>) {
           // 绘制
       }
   }
   ```

4. **添加测试**
   ```kotlin
   @Test
   fun testFindMatchingBracket() {
       val matcher = BracketMatcher()
       val content = Content("fun foo() { }")
       val match = matcher.findMatchingBracket(content, 0, 8)
       assertEquals(Pair(10, 11), match)  // 找到匹配的 }
   }
   ```

5. **更新文档**
   - 如果是公共 API，添加 KDoc 注释
   ```kotlin
   /**
    * 查找匹配的括号对
    *
    * @param line 起始行号
    * @param column 起始列号
    * @return 匹配括号的 (行, 列) 对，如果找不到返回 null
    */
   fun findMatchingBracket(line: Int, column: Int): Pair<Int, Int>?
   ```

6. **性能测试**
   - 在大文件上测试性能
   - 确保不会导致卡顿

7. **提交 PR**
   ```bash
   git commit -m "feat(editor): add bracket pair highlighting"
   ```

### 📚 改进文档

#### 任务：更新开发指南

**步骤**：

1. **创建分支**
   ```bash
   git checkout -b docs/expand-language-guide
   ```

2. **编辑文档**
   - 使用 Markdown 编写
   - 添加示例代码
   - 包含清晰的说明

3. **验证格式**
   ```bash
   # 在本地预览 Markdown
   # 使用 VS Code 或其他 Markdown 预览工具
   ```

4. **提交**
   ```bash
   git commit -m "docs: expand language implementation guide with examples"
   ```

### ♻️ 代码重构

#### 任务：优化 Lexer 性能

**步骤**：

1. **创建分支**
   ```bash
   git checkout -b refactor/optimize-lexer
   ```

2. **分析性能**
   - 使用 Android Profiler
   - 找到瓶颈
   - 制定改进计划

3. **实现优化**
   - 修改代码
   - 添加缓存
   - 减少对象创建

4. **基准测试**
   ```bash
   # 在优化前后运行测试，比较性能
   # 记录结果
   ```

5. **验证不破坏功能**
   ```bash
   ./gradlew test
   ```

6. **提交**
   ```bash
   git commit -m "perf(lang): optimize lexer tokenization speed by 30%"
   ```

---

## 常见问题排查

### ❌ 问题 1: 提交时出现 "Cannot find class"

**原因**：代码引用了不存在的类

**解决**：
```bash
# 1. 清理构建
./gradlew clean

# 2. 刷新依赖
./gradlew --refresh-dependencies

# 3. 重新构建
./gradlew build
```

### ❌ 问题 2: PR 有冲突

**原因**：上游分支已经改变了你修改的文件

**解决**：
```bash
# 1. 更新本地 main
git fetch upstream
git checkout main
git rebase upstream/main

# 2. 变基你的功能分支
git checkout feature/my-feature
git rebase main

# 3. 解决冲突
# Android Studio 会显示冲突文件
# 手动编辑、选择正确版本

# 4. 完成变基
git add .
git rebase --continue

# 5. 推送（可能需要强制推送）
git push --force-with-lease
```

### ❌ 问题 3: 提交消息不符合规范

**原因**：提交消息格式不对

**修改最后一个提交**：
```bash
git commit --amend -m "feat(editor): correct title format"
git push --force-with-lease
```

### ❌ 问题 4: 忘记了分支上的部分改动

**恢复方案**：
```bash
# 查看之前的版本
git log --oneline

# 恢复到某个提交
git reset --soft <commit-hash>

# 修改并重新提交
git add .
git commit -m "feat: include forgotten changes"
```

### ❌ 问题 5: 如何参与现有 PR 的开发？

**步骤**：
```bash
# 1. 获取别人的 PR 代码
git fetch upstream pull/PR_NUMBER/head:pr-branch

# 2. 切换到该分支
git checkout pr-branch

# 3. 在该分支上继续开发
# ...

# 4. 推送回 PR
git push upstream pr-branch:PR_BRANCH_NAME
```

---

## 与维护者沟通

### 💬 讨论设计方案

如果你要实现一个大功能，建议先讨论设计：

1. **创建讨论**
   - GitHub Discussions 标签
   - 或在相关 Issue 中评论

2. **描述方案**
   ```markdown
   我想实现 LSP 集成。计划如下：

   ## 设计
   1. 创建 LSPClient 类，负责与语言服务器通信
   2. 在 Editor 中添加 attach/detach 方法
   3. 支持补全、诊断、跳转定义

   ## 可能的挑战
   - 处理异步通信和响应
   - 与现有高亮系统的集成

   ## 问题
   - 应该在哪个线程运行 LSP 通信？
   - 如何处理诊断显示的更新？
   ```

3. **获得反馈**
   - 维护者会评论建议
   - 可能会指出更好的方案
   - 或提供参考实现

### 📧 如何提问？

**Good Question（能得到好回答）**：
```
标题：如何在编辑器中添加自定义语言支持？

内容：
我想为我的 DSL 添加语法高亮。

我看到了 Language 接口和 JavaLanguage 实现。
我的理解是应该：

1. 创建 MyDSLLanguage : Language
2. 实现 MyDSLLexer
3. 在编辑器中调用 setLanguage(MyDSLLanguage())

这个方向对吗？还有其他需要注意的地方吗？

[附上示例代码]
```

**Bad Question（不易得到帮助）**：
```
标题：怎样添加语言？

内容：我想添加语言支持但不知道怎么做。
谢谢。
```

### 🎯 提问的最佳实践

1. **提供上下文**
   - 你想做什么
   - 为什么要这样做
   - 你已经尝试了什么

2. **包含代码示例**
   - 显示你的实现尝试
   - 或相关的现有代码片段

3. **描述遇到的错误**
   - 完整的错误信息
   - 堆栈跟踪
   - 步骤复现

4. **查询已有的 Issue**
   - 使用关键字搜索
   - 避免重复提问

---

## 总结：快速参考

### 一分钟快速流程

```bash
# 1. Fork + Clone
git clone --recurse-submodules https://github.com/YOUR_USERNAME/sora-editor.git
cd sora-editor
git remote add upstream https://github.com/Rosemoe/sora-editor.git

# 2. 创建分支
git checkout -b feature/my-feature

# 3. 开发和测试
./gradlew clean build test lint

# 4. 提交
git add .
git commit -m "feat: implement my awesome feature"

# 5. 推送
git push -u origin feature/my-feature

# 6. 在 GitHub 上创建 PR
```

### 完整检查清单

- [ ] Fork 了项目
- [ ] Clone 了 fork
- [ ] 添加了 upstream remote
- [ ] 创建了功能分支
- [ ] 代码能够编译
- [ ] 所有测试通过
- [ ] Lint 检查通过
- [ ] 提交消息符合规范
- [ ] 推送到 fork
- [ ] 创建了 PR
- [ ] PR 描述清晰完整
- [ ] 等待审查
- [ ] 根据反馈进行改进

---

**更新时间**：2026-03-05
**贡献者感谢**：感谢所有为 Sora Editor 做出贡献的开发者！
