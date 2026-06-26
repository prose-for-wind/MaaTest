# 问题与笔记

本文档用于记录学习 MaaPracticeBoilerplate、MaaFramework、GitHub、VS Code 和 Codex 过程中的关键问题、结论、报错和最小修复方案。

当前项目是测试性质项目，不额外创建 `DEVELOPMENT_LOG`、`ENVIRONMENT_NOTES`、`TODO` 等长期流程文档。

## Q001：应该下载 MaaFramework 还是 MaaPracticeBoilerplate？

结论：

作为新手，如果目标是开发自己的 Maa 项目，应该从 MaaPracticeBoilerplate 开始，而不是直接修改 MaaFramework 框架本体。

原因：

MaaFramework 是底层框架本体，适合研究框架原理或参与框架开发。

MaaPracticeBoilerplate 是作者提供的项目模板，更适合作为自己的项目起点。

## Q002：下载 MaaPracticeBoilerplate 后不知道从哪里开始怎么办？

结论：

当前项目是测试性质项目，不需要复杂的长期维护流程。

目前只保留：

- `AGENTS.md`
- `docs/PROJECT_EXPLANATION.md`
- `docs/QUESTIONS_AND_NOTES.md`

不创建 skill，不创建过多流程文档。

后续动作：

1. 先让 Codex 阅读项目结构
2. 生成项目解释文档
3. 记录关键问题
4. 跑通环境
5. 再尝试一个最小修改

## 当前项目初步结论

- 实际项目目录是 `D:\projects\MaaTest\MaaTest`。
- `assets/interface.json` 是 interface 配置入口。
- `assets/resource/pipeline/my_task.json` 是当前能看到的 pipeline 示例。
- `agent/` 目录包含自定义 action 和 recognition 示例。
- `.github/workflows/check.yml` 中能看到自动检查命令。
- `.github/workflows/install.yml` 中能看到下载 MaaFramework 并打包 install 目录的流程。
- 当前仓库中未看到 `deps/bin`，直接运行 `tools/install.py` 可能会提示需要先下载 MaaFramework 到 `deps`。
- 当前环境读取部分中文文件时出现乱码，尤其是 `README.md`、`assets/interface.json` 的中文注释和部分中文字符串，所以这些内容不能完整确认原文。

## 报错记录模板

后续遇到报错时，按下面格式记录。

```text
日期：
执行命令：
执行目录：
报错原文：
可能原因：
最小修复方案：
结果：
```
