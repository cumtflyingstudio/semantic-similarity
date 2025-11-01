# Semantic Similarity
> 文档分析：Claude Sonnet 4.5  
> 生成日期：2025-11-02

这个项目是一个基于 Java 的语义相似度计算与演示程序，包含词/短语/句子层面的相似度算法实现、若干统计与工具模块，以及一个基于 Swing 的演示 UI。它既可作为研究/学习相似度算法的参考实现，也包含可直接运行的演示程序。

## 项目简介

semantic-similarity 是一个将多种相似度计算方法组织为模块化实现的 Java 工程，主要功能包括：

- 词语、短语与句子相似度计算（字/词级相似、编辑距离、Dice、LCMC 等算法实现）。
- 基于统计与语义知识的相似度辅助模块（拼音、义原/同义资源、词典统计）。
- 简单的情感/倾向性训练与词向量倾向模块（训练工具、词倾向表示）。
- 提供 Swing GUI（`Start`）作为交互式演示，便于直观比较不同相似度算法的输出。

项目依赖与构建：基于 Maven 构建（Java 7/1.7），使用了 ansj 分词、Guava、logback 等常见库，项目的主 artifactId 为 `semantic-similarity`，版本 `1.0.0`（见 `pom.xml`）。

## 快速开始

先决条件：

- JDK 1.7 或更高（项目 pom 指定 source/target 为 1.7）。
- Maven（用于构建与运行测试）。

构建：

```bash
cd semantic-similarity
mvn -DskipTests package
```

打包后（使用 maven-assembly 插件）会在 `target/` 下生成一个包含依赖的可运行 jar，例如：

```
target/semantic-similarity-1.0.0-jar-with-dependencies.jar
```

运行演示 UI：

```bash
java -jar target/semantic-similarity-1.0.0-jar-with-dependencies.jar
```

或者在 IDE 中以主类 `zx.soft.ui.Start` 启动（该类会初始化全局字体并打开 Swing 界面，包含词/短语/句子/词法/义原树/情感分析等标签页）。

项目也包含一些启动脚本（`bin/start`、`bin/start.py`），但它们依赖于特定的目录布局与外部 lib 目录，优先使用 Maven 打包后的 jar 或 IDE 运行。

## 项目架构

工程以 Maven 模块布局组织，主要目录（简要）：

```
semantic-similarity/
├── pom.xml                    # Maven 构建文件，Java 1.7，assembly 打包
├── bin/                       # 若干本地/历史启动脚本（可选）
├── dict/                      # 词典与情感/倾向词资源
├── src/main/java/zx/soft/
│   ├── classification/        # 朴素贝叶斯等分类/训练相关（Training, WordTendency）
│   ├── similarity/            # 相似度实现（词/短语/句子/文本等）
│   │   ├── phrase/            # 短语相似度入口（PhraseSimilarity）
│   │   ├── sentence/          # 句子相似度、分词代理（SentenceSimilarity, SegmentProxy）
│   │   ├── text/              # 文本级别的相似度（DiceSimilarity 等）
│   │   └── word/              # 词语相似度实现与资源（pinyin, hownet, cilin 等）
│   ├── tendency/              # 词倾向/情感分析相关模块
│   └── ui/                    # Swing 演示界面（Start、各 UI 面板）
└── src/main/resources/        # 配置与数据（about.html、拼音映射等）
```

### 核心功能/目录

#### similarity（相似度）

此目录是项目的核心，涵盖多层次相似度计算：

- `word`：基于字符/拼音/义原/词典的词汇级相似度实现。  
- `phrase`：短语层面的相似算法封装，支持将词相似度组合用于短语比对。  
- `sentence`：句子相似度及分词代理（`SegmentProxy`），包含句子分词、编辑距离等方法。  
- `text`：文本级别指标（如 Dice similarity）与组合策略。

代表类：`WordSimilarity`, `PhraseSimilarity`, `SentenceSimilarity`, `DiceSimilarity`。

#### statistic（统计）

用于构建字/词出现统计与词典支持（`DictStatistic`, `LCMC` 等），在计算基于统计的相似度指标时被调用。

#### tendency（情感/倾向）

提供基于词典与训练的词倾向分析，包含训练入口与词倾向表示（`Training`, `WordTendency`）。

#### ui（演示界面）

Swing 界面集合，主要入口 `zx.soft.ui.Start`，将项目能力以交互式标签页的形式暴露（词/短语/句子/义原树/情感分析等）。

#### util（工具集）

常用工具类：文件读写、编辑距离实现、拼音映射、XML 工具、日志配置读取等（例如 `FileUtils`, `EditDistance`, `PinyinUtils`）。

### 工程化配置

- 构建工具：Maven，`pom.xml` 声明了依赖与插件（maven-compiler-plugin、assembly-plugin 用于生成带依赖的可运行 jar）。
- Java 版本：1.7（pom 中 `source`/`target` = 1.7）。
- 主要依赖：ansj（分词）、guava、logback、commons-lang3 等。

建议在 CI 中使用 `mvn -DskipTests package` 或添加单元测试步骤 `mvn test` 来保证稳定性。

## 开发说明

如何开始开发：

1. 克隆仓库并在 JDK 1.7/1.8 下打开（IDEA/ Eclipse）。
2. 通过 Maven 导入依赖：`mvn -DskipTests package`。
3. 在 IDE 中以 `zx.soft.ui.Start` 启动主类查看 UI；或运行生成的 jar。 

代码风格与注意事项：

- 源码使用标准 Java 约定（包路径 `zx.soft.*`），字符编码 UTF-8。  
- 演示 UI 使用 Swing，部分字体在不同操作系统可能需要调整（`Start.InitGlobalFont` 中设置为 Microsoft YaHei，用于避免 Linux/Ubuntu 的中文显示问题）。

如果需要扩展算法：请在 `src/main/java/zx/soft/similarity` 下新增模块或在现有接口（如 `Similaritable`）上实现新算法，并补充对应的单元测试。