# MaaPracticeBoilerplate 项目解释文档

本文档面向新手，用来解释当前项目中已经能看到的结构和文件。没有在项目文件中找到明确说明的内容，会直接写“未找到明确说明”。

## 1. 项目是做什么的

从 `README.md`、`maatools.config.mts`、`assets/interface.json`、`assets/resource/pipeline/my_task.json` 和 `.github/workflows/` 可以看出：

- 当前项目名是 `MaaPracticeBoilerplate`。
- 它是一个 MaaFramework 项目模板。
- 它提供了 Maa 项目的基本目录、界面配置、任务 pipeline 示例、自定义 agent 示例、检查脚本和 GitHub Actions 流程。
- 这个仓库当前被用作测试性质项目，主要用于学习项目结构和环境配置。

注意：`README.md` 在当前环境中显示为乱码，无法完整确认原文含义。本文档只记录能从文件结构、配置字段、脚本和可读内容中确认的信息。

## 2. MaaPracticeBoilerplate 和 MaaFramework 的关系

明确能看到的信息：

- `README.md` 中引用了 `MaaFramework` 仓库链接。
- `.github/workflows/install.yml` 会从 `MaaXYZ/MaaFramework` 下载发布包。
- `tools/install.py` 要求先把 MaaFramework 下载到 `deps` 目录，否则会提示 `Please download the MaaFramework to "deps" first.`
- `deps/tools/*.schema.json` 是 MaaFramework 相关的 JSON schema，用于校验 interface 和 pipeline 配置。

新手理解：

- `MaaFramework` 是底层框架本体。
- `MaaPracticeBoilerplate` 是基于 MaaFramework 的项目模板，更适合作为自己 Maa 项目的起点。
- 对于普通项目开发，优先修改模板项目里的资源、配置、任务和 agent 示例，不要直接修改 MaaFramework 框架本体。

## 3. 项目主要目录结构

当前项目根目录是：

```text
D:\projects\MaaTest\MaaTest
```

主要目录和文件：

```text
.github/
.vscode/
agent/
assets/
deps/
docs/
tools/
README.md
maatools.config.mts
package.json
package-lock.json
pnpm-lock.yaml
LICENSE
```

## 4. 每个重要目录的作用

### `.github/`

GitHub 相关配置目录。

- `.github/workflows/check.yml`：检查项目资源和配置。
- `.github/workflows/install.yml`：下载 MaaFramework 和 MFAAvalonia，并打包安装产物。
- `.github/workflows/sync_schema_files.yml`：从 MaaFramework 同步 JSON schema 文件。
- `.github/workflows/mirrorchyan_release.yml`、`.github/workflows/mirrorchyan_release_note.yml`：MirrorChyan 发布相关流程，目前 workflow 里 `if: false`，默认不会执行。
- `.github/ISSUE_TEMPLATE/`：GitHub issue 模板。
- `.github/PULL_REQUEST_TEMPLATE.md`：GitHub pull request 模板。

### `.vscode/`

VS Code 推荐配置。

- `.vscode/extensions.json` 推荐安装：
  - `ms-python.black-formatter`
  - `nekosu.maa-support`
  - `davidanson.vscode-markdownlint`
- `.vscode/settings.json` 配置 JSON schema、JSONC 识别、Python 格式化、Markdown 格式化等。

### `agent/`

Python 自定义 agent 示例目录。

- `agent/main.py`：agent 启动入口。它要求运行参数中提供 `socket_id`。
- `agent/my_action.py`：自定义动作示例，注册名是 `my_action_111`。
- `agent/my_reco.py`：自定义识别示例，注册名是 `my_reco_222`。

注意：`assets/interface.json` 中 agent 配置目前是注释状态，所以是否实际启用 agent，需要后续根据运行方式再确认。

### `assets/`

项目资源和界面配置目录。

- `assets/interface.json`：项目界面、控制器、资源路径、任务入口等配置。
- `assets/resource/`：资源目录。
- `assets/resource/pipeline/my_task.json`：任务 pipeline 示例。
- `assets/resource/image/empty.png`：图片资源示例。

### `deps/`

依赖和 schema 目录。

- `deps/tools/pipeline.schema.json`：pipeline 校验规则。
- `deps/tools/interface.schema.json`：interface 校验规则。
- `deps/tools/interface_config.schema.json`：interface config 校验规则。
- `deps/tools/interface_import.schema.json`：interface import 校验规则。
- `deps/tools/custom.action.schema.json`、`deps/tools/custom.recognition.schema.json`：自定义 action / recognition 相关校验规则。

从 `tools/install.py` 看，完整安装还需要 `deps/bin` 和 `deps/share/MaaAgentBinary` 等 MaaFramework 产物。当前仓库中未看到 `deps/bin`。

### `docs/`

文档目录。

- `docs/zh_cn/develop/how_to_develop.md`：开发说明文档。
- `docs/zh_cn/develop/faq.md`：常见问题。
- `docs/zh_cn/develop/custom_configure.md`：自定义配置说明。
- `docs/zh_cn/develop/pull_request_guidelines.md`：PR 规范。
- `docs/PROJECT_EXPLANATION.md`：当前新手项目解释文档。
- `docs/QUESTIONS_AND_NOTES.md`：当前问题与笔记文档。

### `tools/`

辅助脚本目录。

- `tools/install.py`：复制 MaaFramework 依赖、资源、README、LICENSE、agent 到 `install/`。
- `tools/configure.py`：配置 OCR 模型资源。
- `tools/validate_schema.py`：校验 JSON / JSONC 文件是否符合 schema。
- `tools/requirements.txt`：Python 脚本依赖，目前包含 `json-with-comments`。

## 5. 重要文件说明

### `README.md`

项目说明文件。当前环境中读取时出现乱码，因此未能完整确认原文内容。

能确认的信息：

- 标题是 `MaaPracticeBoilerplate`。
- 引用了 MaaFramework。
- 包含指向 `docs/zh_cn/develop/how_to_develop.md`、`faq.md`、`pull_request_guidelines.md` 的链接。

### `maatools.config.mts`

`@nekosu/maa-tools` 的配置文件。

明确内容：

- `maaVersion: 'latest'`
- `interfacePath: 'assets/interface.json'`
- `cwd: import.meta.dirname`

这说明 maa-tools 检查时会以项目根目录为工作目录，并使用 `assets/interface.json` 作为 interface 配置。

### `package.json`

Node 依赖配置。

当前只有 `devDependencies`：

- `@nekosu/maa-tools`
- `@types/node`
- `prettier-plugin-multiline-arrays`

未找到明确的 `scripts` 字段，所以没有看到 `npm run xxx` 这种明确脚本命令。

### `assets/interface.json`

项目 interface 配置文件。它定义了：

- `interface_version`
- 项目名称、描述、联系方式、许可证、欢迎语
- GitHub 地址和版本号
- 控制器配置，包括 `Adb` 和 `Win32`
- 资源路径 `./resource`
- 任务列表，每个任务对应一个 `entry`
- 任务选项
- 可选 agent 配置示例，但当前被注释

注意：文件中的中文注释和字符串在当前环境输出里有乱码，不能完整确认原文。

### `assets/resource/pipeline/my_task.json`

任务 pipeline 示例文件。

当前包含：

- `MyTask1`
- `MyTask2`
- `MyTask3`
- `MyTask4`
- `OcrTask`

其中 `MyTask4` 明确使用：

- `recognition: "Custom"`
- `custom_recognition: "my_reco_222"`
- `action: "Custom"`
- `custom_action: "my_action_111"`

这与 `agent/my_reco.py` 和 `agent/my_action.py` 中注册的名称对应。

## 6. pipeline 文件在哪里

当前能看到的 pipeline 文件：

```text
assets/resource/pipeline/my_task.json
```

pipeline schema 文件：

```text
deps/tools/pipeline.schema.json
```

## 7. resource / 资源文件在哪里

资源目录：

```text
assets/resource/
```

当前资源示例：

```text
assets/resource/pipeline/my_task.json
assets/resource/image/empty.png
```

`assets/interface.json` 中的资源路径写的是：

```json
"path": [
    "./resource"
]
```

结合 `tools/install.py` 可以看出，安装时会把 `assets/resource` 复制到 `install/resource`。

## 8. 配置文件在哪里

主要配置文件：

- `assets/interface.json`：Maa 项目 interface 配置。
- `maatools.config.mts`：maa-tools 配置。
- `.vscode/settings.json`：VS Code 编辑器配置。
- `.vscode/extensions.json`：VS Code 推荐插件。
- `package.json`：Node 开发依赖。
- `tools/requirements.txt`：Python 工具依赖。
- `.github/workflows/*.yml`：GitHub Actions pipeline 配置。

## 9. 程序如何运行或测试

明确找到的检查方式：

```powershell
npm ci
npx @nekosu/maa-tools check
```

这些命令来自 `.github/workflows/check.yml`。

明确找到的 schema 校验方式：

```powershell
python -m pip install jsonschema==4.26.0 referencing==0.37.0
python tools/validate_schema.py --schema-dir deps/tools --resource-dirs assets/resource --exclude-dirs assets/resource/announcement --interface-files assets/interface.json
```

这些命令来自 `.github/workflows/check.yml`。

明确找到的安装打包方式：

```powershell
cd tools
python -m pip install -r ./requirements.txt
python ./install.py <version> <os> <arch>
```

示例参数来自 `tools/install.py`：

```powershell
python ./install.py v1.0.0 win x86_64
```

限制：

- `tools/install.py` 明确要求 `deps/bin` 存在。
- 当前仓库中未看到 `deps/bin`，所以直接运行安装脚本大概率会提示需要先下载 MaaFramework 到 `deps`。
- 项目没有在 `package.json` 中提供明确的 `npm run` 命令。
- 如何启动完整图形界面或最终应用：未找到明确说明。

## 10. 如果当前项目没有明确运行说明

如何启动完整 Maa 应用：未找到明确说明。

如何在本地手动下载并放置 MaaFramework 到 `deps` 的完整步骤：未找到明确说明。

如何启用 `agent/main.py` 并把它接入实际运行流程：当前只看到 `assets/interface.json` 中被注释的 agent 示例，未找到明确说明。

## 11. 新手应该按什么顺序阅读文件

建议阅读顺序：

1. `README.md`：先了解项目定位。注意当前本地显示有乱码。
2. `docs/PROJECT_EXPLANATION.md`：阅读当前新手解释文档。
3. `assets/interface.json`：了解界面、资源、任务入口在哪里配置。
4. `assets/resource/pipeline/my_task.json`：了解任务 pipeline 如何命名和连接。
5. `agent/my_action.py` 和 `agent/my_reco.py`：了解自定义动作和自定义识别示例。
6. `agent/main.py`：了解 agent 如何启动。
7. `maatools.config.mts`：了解 maa-tools 检查入口。
8. `.github/workflows/check.yml`：了解自动检查做了什么。
9. `tools/validate_schema.py` 和 `tools/install.py`：需要运行检查或打包时再看。
10. `docs/zh_cn/develop/*.md`：需要进一步学习 MaaPracticeBoilerplate 开发方式时再看。

## 12. 哪些文件后续最可能修改

作为新手做最小实验，后续最可能修改：

- `assets/interface.json`：改项目名称、描述、任务显示名、任务入口等。
- `assets/resource/pipeline/my_task.json`：改任务 pipeline。
- `assets/resource/image/`：添加或替换识别用图片资源。
- `agent/my_action.py`：修改自定义动作示例。
- `agent/my_reco.py`：修改自定义识别示例。
- `docs/QUESTIONS_AND_NOTES.md`：记录问题、报错和结论。
- `docs/PROJECT_EXPLANATION.md`：随着理解加深补充项目解释。

## 13. 哪些文件暂时不要乱改

新手阶段建议暂时不要随意修改：

- `deps/tools/*.schema.json`：这些是校验规则，来自 MaaFramework。
- `.github/workflows/*.yml`：这些会影响 GitHub Actions 流程。
- `tools/validate_schema.py`：这是校验脚本，除非明确知道要改什么。
- `tools/install.py`：这是安装打包脚本，改错会影响产物生成。
- `package-lock.json`、`pnpm-lock.yaml`：依赖锁文件，不要手动编辑。
- `.gitignore`、`.gitattributes`、`.gitmodules`：Git 行为相关文件，不要随意改。
- MaaFramework 框架本体：当前项目中未看到完整框架源码，不应直接修改框架本体。

## 14. 后续可以尝试的最小修改

建议最小修改从文案或示例任务开始：

1. 在 `assets/interface.json` 中把项目 `name` 从模板名改成自己的测试项目名。
2. 在 `assets/interface.json` 中修改一个任务显示名。
3. 在 `assets/resource/pipeline/my_task.json` 中只改一个简单任务节点，先不要增加复杂识别逻辑。
4. 运行检查命令确认配置没有写坏：

```powershell
npm ci
npx @nekosu/maa-tools check
python -m pip install jsonschema==4.26.0 referencing==0.37.0
python tools/validate_schema.py --schema-dir deps/tools --resource-dirs assets/resource --exclude-dirs assets/resource/announcement --interface-files assets/interface.json
```

如果检查报错，把命令、目录、报错原文、可能原因、最小修复方案记录到 `docs/QUESTIONS_AND_NOTES.md`。
