# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

**sora-editor** is a powerful, optimized code editor library for Android featuring incremental syntax highlighting, auto-completion with snippets, LSP integration, TreeSitter/TextMate support, and extensive editing features (undo/redo, search/replace, inlay hints, sticky scroll, etc.).

This is a **multi-module Gradle project** using Kotlin DSL with custom build logic conventions. The editor is distributed as libraries published to Maven Central, plus a sample Android app demonstrating features.

## Common Development Commands

### Building
- **Debug APK:** `./gradlew assembleDebug` - Builds the debug APK for the sample app
- **Release build:** `./gradlew assembleRelease` - Requires signing configuration
- **Clean build:** `./gradlew clean` - Clean all build artifacts
- **Build all modules:** `./gradlew build` - Compiles all modules

### Testing
- **Run all tests:** `./gradlew test` - Executes unit tests across all modules
- **Run tests for specific module:** `./gradlew :editor:test` - Run tests in the editor module only
- **Run specific test class:** `./gradlew :app:test --tests io.github.rosemoe.sora.app.tests.paged.PagedEditSessionTest`
- **Run with verbose output:** `./gradlew test -i`

### Linting & Code Quality
- **Lint editor module:** `./gradlew :editor:lint` - Android lint checks
- **Build with lint:** Most Gradle build tasks automatically include lint checks

### Publishing (Maintainers Only)
- **Publish to Maven Central:** `./gradlew publishAllPublicationsToMavenCentral` - Publishes libraries (requires secrets)
- **Individual module publish:** `./gradlew :editor:publishReleasePublicationToMavenCentralRepository`

### Development Workflow
- **Build and run app:** `./gradlew installDebug` - Installs the debug APK on a connected device
- **Monitor Gradle cache:** Caching is enabled in gradle.properties; use `--no-build-cache` to bypass if needed
- **Incremental builds:** Enabled by default; leverage when developing modules

## Project Structure & Architecture

### Module Organization

**Core Editor Module** (`editor/`)
- The heart of the project containing the editor UI component, document model, rendering, and text manipulation
- Main class: `Editor.kt` (defines the core editor view and API)
- Key packages:
  - `io.github.rosemoe.sora.widget.*` - Editor UI components and rendering
  - `io.github.rosemoe.sora.text.*` - Text buffer and document model
  - `io.github.rosemoe.sora.lang.*` - Language support interfaces and defaults
  - `io.github.rosemoe.sora.event.*` - Event system for editor lifecycle and user actions
  - `io.github.rosemoe.sora.graphics.*` - Rendering and visual components (inlay hints, text rows, etc.)

**Language Support Modules**
- `language-textmate/` - TextMate grammar support for tokenization and syntax highlighting
- `language-monarch/` - Monarch language packs for client-side highlighting
- `language-treesitter/` - TreeSitter parser integration for incremental parsing
- `language-java/` - Java-specific language implementation
- These modules plug into the editor core via language provider interfaces

**LSP Integration** (`editor-lsp/`)
- Language Server Protocol bridge for code intelligence (completion, diagnostics, hover, go-to-definition)
- Enables remote language server support

**Native Library** (`oniguruma-native/`)
- JNI wrapper for the Oniguruma regex engine
- Used by TextMate and other parsing components for robust regex matching
- Contains C++ source and build scripts for native compilation

**Sample App** (`app/`)
- Android application module demonstrating editor features
- Includes TextMate grammar assets and sample code files
- Wires together all language modules and LSP for a complete editor experience

**Build Infrastructure** (`build-logic/`)
- Custom Gradle plugin conventions applied across all modules
- Defines common Android/Kotlin compilation settings via `build-logic.root-project` plugin

### Architectural Patterns

**Plugin Architecture**: Language modules are loosely coupled to the editor core via standardized interfaces. New language support can be added by implementing language provider interfaces without modifying core editor code.

**Separation of Concerns**:
- **Editor core** handles rendering, text editing, UI, and document lifecycle
- **Language modules** provide syntax highlighting tokens, parsing, and grammars
- **LSP module** handles external language server communication
- **Native module** supplies high-performance regex for parsing backends

**Multi-Language Support**: TextMate, Monarch, and TreeSitter backends coexist, allowing multiple syntax highlighting strategies and incremental parsing for different languages.

## Build Configuration Details

### Gradle Setup
- **Kotlin DSL**: All build files use `.kts` format
- **JDK Version**: Java 17 (configured in build.gradle.kts)
- **Kotlin Version**: 2.3 with official code style
- **Caching**: Enabled globally in gradle.properties for faster builds
- **Parallel builds**: Enabled for decoupled projects

### Key Gradle Properties (`gradle.properties`)
```
org.gradle.jvmargs=-Xmx2048M              # JVM heap for daemon
org.gradle.caching=true                    # Build cache enabled
org.gradle.parallel=true                   # Parallel build enabled
android.useAndroidX=true                   # Use AndroidX libraries
android.enableR8.fullMode=true             # Full R8 optimization
```

### Maven Central Publishing
The project publishes libraries to Maven Central with:
- Group ID: `io.github.rosemoe`
- Artifact ID: `editor`, `editor-lsp`, `language-*`, etc.
- Publishing configured via `com.vanniktech.maven.publish` plugin
- Automatic snapshot publishing on main branch (CI/CD)

### Android Configuration
- **Compile SDK**: 35
- **Min SDK**: 21 (standard modules), 24 (high-API modules like editor-lsp)
- **Target SDK**: 35
- **Build Tools**: Latest compatible version

## CI/CD Pipeline

**Workflow File**: `.github/workflows/gradle.yml`

**Build Process**:
1. Cancels previous workflow runs
2. Checks out repository with submodules
3. Sets up JDK 17 (Temurin)
4. Runs `./gradlew assembleDebug` to build the debug APK
5. On success and main branch push: publishes to Maven Central (with signing)
6. Uploads debug APK as artifact

**Trigger**: Pushes to main (excluding renovate-* branches and markdown/docs-only changes), PRs, and manual workflow dispatch.

## Key Development Patterns

### Editor Extension Points
The editor core exposes extension points for:
- **Language registration**: Implement language provider interfaces to register custom syntax highlighting
- **LSP integration**: Connect to language servers via the editor-lsp module
- **Event listeners**: Subscribe to editor events (text changes, selection, etc.) via the event system
- **Custom rendering**: Use span renderers for custom text decoration (underlines, background colors, etc.)

### Text Model
- Document is represented by `Content.kt` - a immutable text buffer with efficient line-based indexing
- Changes tracked through `SequenceUpdateRange` and `StyleUpdateRange` for incremental updates
- Undo/redo stacks managed internally by the editor

### Styling System
- Syntax highlighting represented as spans with foreground/background colors and text styles
- Spans stored efficiently in `SpanPool.kt` to minimize memory overhead
- `TextStyle.kt` encodes color and style information (bold, italic, etc.)
- Color resolution via `SpanColorResolver` supports both constant colors and resolvable colors

### Inlay Hints & Decorations
- Line-based styling via `LineStyles.kt` (background, side icons, anchor text)
- Inlay hints rendered via `InlayHintRenderer` implementations
- External renderers for custom rendering (`SpanExternalRenderer`)

## Development Notes

### Submodules
The repository uses git submodules (referenced in `.gitmodules`). When cloning, use:
```bash
git clone --recurse-submodules https://github.com/Rosemoe/sora-editor.git
```

### Conventions
- **Code Style**: Official Kotlin style (configured in gradle.properties)
- **Source Compatibility**: Java 17
- **Package Structure**: `io.github.rosemoe.sora.*` for all modules
- **Kotlin Features**: Uses Kotlin 2.3 features; DSL-friendly for readers

### Performance Considerations
- Rendering is incremental: only visible regions are drawn
- Syntax highlighting uses partial updates (`StyleUpdateRange`) to avoid re-highlighting entire files
- Span pool reuses objects to reduce GC pressure
- Native Oniguruma binding for regex-heavy operations (TextMate tokenization)

### Testing
- Unit tests located in `src/test/java` within each module
- Sample test: `app/src/test/java/io/github/rosemoe/sora/app/tests/paged/PagedEditSessionTest.kt` - tests paged editing for long files
- Run tests via Gradle (see Commands section above)

## Documentation & Resources

- **Official Docs**: https://project-sora.github.io/sora-editor-docs/
- **Getting Started**: https://project-sora.github.io/sora-editor-docs/guide/getting-started
- **Reference**: https://project-sora.github.io/sora-editor-docs/reference/xml-attributes
- **Discussions**: Telegram group (https://t.me/rosemoe_code_editor) and QQ group (734652304)

## License

sora-editor is licensed under the GNU Lesser General Public License v2.1 (LGPL v2.1). See the LICENSE file for full details.
