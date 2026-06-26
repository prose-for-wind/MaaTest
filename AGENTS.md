# AGENTS.md

## 项目背景

当前项目是 MaaPracticeBoilerplate 的测试性质项目，主要用于熟悉 GitHub、VS Code、Codex 插件、MaaPracticeBoilerplate 项目结构，以及 MaaFramework 相关项目的基础配置和运行方式。

项目使用者是 GitHub、VS Code、Codex、MaaFramework 相关项目的新手。后续解释应尽量使用中文，面向新手说明，不要默认使用者已经熟悉工程术语。

## 协作要求

- 先解释项目，再修改代码。
- 修改前说明准备修改哪些文件，以及为什么需要修改。
- 不要创建 `.codex/skills/`。
- 不要创建 Codex skill。
- 不要创建过多流程记录文档。
- 不要大规模修改代码。
- 不要重构项目。
- 不要修改 MaaFramework 框架本体。
- 不要在没有说明原因的情况下修改业务文件。
- 如果项目信息不明确，请明确写“未找到明确说明”，不要猜测。

## 文档维护范围

当前项目主要维护以下说明文档：

- `docs/manual.md`
- `docs/fullprojectexplanation.md`
- `docs/environment_preparation.md`

不要额外创建 `DEVELOPMENT_LOG`、`ENVIRONMENT_NOTES`、`TODO` 等长期流程文档，除非使用者后续明确要求。

## 环境准备说明维护任务

后续在本项目中尝试环境配置、资源下载、运行检查或首次启动时，请把可复用的准备步骤维护到 `docs/environment_preparation.md`。这个说明文档的目标是：下一次直接使用 MaaPracticeBoilerplate 模板时，可以复制或参考这份文档，完成前期准备工作。

记录时应尽量面向新手，使用中文，避免写成长期维护日志。重点记录：

- 需要安装或确认的软件，例如 Git、VS Code、Python、Node.js、Maa 相关工具等。
- 需要下载的资源，例如 OCR 模型、模板资源包或 MaaFramework 发布包等。
- 资源应该放到哪个目录。
- 实际执行过的命令。
- 命令的执行目录。
- 成功后的判断方式。
- 失败时的报错原文、可能原因和最小修复方案。
- 哪些内容来自项目文件或 MaaFramework 文档，哪些内容仍是“未找到明确说明”。

## 报错记录要求

遇到环境配置、资源下载、运行检查或首次启动相关报错时，请优先记录到 `docs/environment_preparation.md`，至少包含：

- 执行命令
- 执行目录
- 报错原文
- 可能原因
- 最小修复方案
