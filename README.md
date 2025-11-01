
# semantic-similarity

一个用于中文词语、短语、句子以及篇章层面语义相似度计算的开源框架，包含多个相似度算法实现、统计工具、词典支持及图形界面演示。

> 文档分析：Claude Sonnet 4.5
> 生成日期：2025-11-02

## 项目简介

该项目实现并集成了多种中文相似度计算方法，覆盖字/词/短语/句子/篇章不同层级，包含：字符串编辑距离、字符/词粒度相似度、基于词典（同义词词林、知网/HowNet）的方法、统计相似度（LCMC、Dice 等）以及若干工程化组件（词典管理、分词代理、UI 演示）。项目以 Java/Maven 构建，附带测试用例与部分资源文件（拼音表、词典片段等）。

## 快速开始

系统依赖：
- JDK 8+（推荐使用与项目兼容的 Java 版本）
- Maven（用于构建与运行测试）

构建与测试：

```bash
# 在项目根目录运行
mvn -U -DskipTests=false clean test package
```

运行演示 UI（示例）：

```bash
# 运行 main 类：zx.soft.ui.Start
# 可使用 IDE 直接运行或通过 mvn exec 插件（若已配置）
```

## 项目架构

源码位于：`src/main/java`，测试位于：`src/test/java`，资源和数据位于：`src/main/resources` 与 `dict/` 目录。

### 主要目录 / 核心功能
- `src/main/java/zx/soft/similarity`：相似度核心实现与工厂类。
- `src/main/java/zx/soft/classification`：分类器（如朴素贝叶斯）及相关数据结构（Feature、Instance、Variable）。
- `src/main/java/zx/soft/similarity/sentence`：句子级相似度相关（分词代理、句子相似度实现、编辑距离子模块）。
- `src/main/java/zx/soft/similarity/word`：词语层相似度（字/拼音/Hownet 支持）。
- `src/main/java/zx/soft/similarity/statistic`：统计方法（LCMC、词典统计等）。
- `src/main/java/zx/soft/tendency`：情感/倾向性分析模块（训练与词向量/词倾向实现）。
- `src/main/java/zx/soft/ui`：简单 GUI，用于交互式演示与测试（Start、SentenceSimilarityUI、等）。
- `dict/`：词典与情感词表，用于实验与算法支持。
- `bin/`：脚本与启动包装（`start`, `start.py`）。

### 关键目录/核心功能1分析（similarity）
similarity 包实现了短语、句子到篇章级别的相似度计算。实现包含：
- 编辑距离与其变种（`EditDistance`、`editdistance/*`）。
- 基于字符/词的相似度（`CharBasedSimilarity`、`WordSimilarity`）。
- 统计相似度（`DiceSimilarity`、`LCMC`）。

这些实现各自偏向不同场景：编辑距离适用于字符串级别比较，词/字层相似适用于中文语义比较，统计方法适合语料驱动的相似度估计。

### 关键目录/核心功能2分析（资源与词典）
`dict/` 下包含情感词典、程度词等多种文本资源，`src/main/resources/data` 包含拼音映射等辅助数据。项目对中文处理存在较强的资源依赖，运行时需保证这些文件编码正确（UTF-8/GBK 视具体文件而定）。

### 关键目录/核心功能3分析（UI 与 工程化）
`ui` 包提供 Swing（或其他）演示界面，可用于交互式比较与调试。`bin/` 中的启动脚本便于在不同环境下快速运行。工程使用 Maven 管理依赖与构建，`pom.xml` 是主要的工程配置入口。

### 工程化配置
- 构建工具：Maven（`pom.xml`）
- 日志：logback 配置位于 `src/main/resources/logback.xml`。
- 测试：项目包含 JUnit 测试（`src/test/java`），建议在修改后运行 `mvn test` 验证。

## 开发说明

- 代码风格：遵循 Java 常规约定，包路径以 `zx.soft` 为根。
- 本地开发：推荐使用 IDEA（添加 `.idea/` 忽略规则在 `.gitignore` 中）。
- 运行建议：中文文件处理注意编码（IDE/终端需配置 UTF-8）。
- 贡献：提交 PR 前请确保单元测试通过并写明变更目的与测试用例。