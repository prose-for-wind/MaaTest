# MaaPracticeBoilerplate 新手上手手册

这份手册是写给你看的，不是给 Agent 看的。目标很简单：帮你知道这个模板项目应该从哪里看、先改什么、怎么验证、哪些东西暂时不要碰。

当前模板项目路径是：

```text
D:\projects\MaaTest\MaaTest
```

MaaFramework 源码路径是：

```text
D:\projects\MaaSource\MaaFramework
```

注意：MaaFramework 源码只用来参考和理解底层机制，本手册不要求你修改它。

## 1. 先按这个顺序熟悉项目

### 第一步：确认 VS Code 打开的是正确目录

请确认 VS Code 打开的目录是：

```text
D:\projects\MaaTest\MaaTest
```

这个目录下应该能看到：

```text
README.md
assets/
agent/
tools/
deps/
docs/
.github/
.vscode/
maatools.config.mts
package.json
```

为什么要先确认这个：

当前外层还有一个 `D:\projects\MaaTest`，真正的模板项目在里面的 `MaaTest` 子目录。如果打开错了目录，Git 状态、VS Code 插件提示、文件路径都会让你困惑。

### 第二步：先读模板项目自己的开发说明

建议先看：

```text
README.md
docs/zh_cn/develop/how_to_develop.md
docs/zh_cn/develop/faq.md
docs/PROJECT_EXPLANATION.md
docs/QUESTIONS_AND_NOTES.md
```

其中最重要的是：

- `docs/zh_cn/develop/how_to_develop.md`：模板项目官方开发步骤。
- `docs/zh_cn/develop/faq.md`：常见环境问题，比如 Python、运行库、OCR 报错。

模板项目的 `how_to_develop.md` 明确说了几件事：

- 开发前先读 MaaFramework 的快速开始文档。
- 使用模板项目时，主要修改 `assets` 目录下的 `resource` 资源文件和 `interface.json`。
- OCR 资源需要放到 `assets/resource/model/ocr/`。
- 打包发布主要依靠 `.github/workflows/install.yml`。

### 第三步：理解 `assets/interface.json`

先打开：

```text
assets/interface.json
```

你可以先把它理解成“项目菜单和入口配置”。它告诉通用 UI：

- 项目叫什么。
- 有哪些控制方式，比如 Android 的 `Adb`、桌面端的 `Win32`。
- 资源文件在哪里。
- 有哪些任务可以执行。
- 每个任务从哪个 pipeline 节点开始。

MaaFramework 官方文档 `docs/zh_cn/3.3-ProjectInterfaceV2协议.md` 明确说明：`ProjectInterface` 是 MaaFramework 的标准化项目结构声明，目前包含 `interface.json` 一个文件；`task.entry` 是 pipeline 中起点 Node 的名称；`resource.path` 是资源加载路径，而且资源不只包括 pipeline，也包括 image 和 model。

当前项目里，`maatools.config.mts` 也明确写了：

```text
interfacePath: 'assets/interface.json'
```

所以你后续理解项目时，应该先从 `assets/interface.json` 开始。

### 第四步：理解资源目录 `assets/resource`

再打开：

```text
assets/resource/
```

当前能看到：

```text
assets/resource/pipeline/my_task.json
assets/resource/image/empty.png
```

MaaFramework 快速开始文档明确说明，模板项目的资源目录一般这样组织：

```text
resource/
├── image/          图片素材
├── model/ocr/      OCR 文字识别模型
└── pipeline/       任务流水线
```

当前项目还没有看到：

```text
assets/resource/model/ocr/
```

但模板项目 `how_to_develop.md` 明确要求下载 OCR 资源，并解压到：

```text
assets/resource/model/ocr/
```

解压后应包含：

```text
det.onnx
keys.txt
rec.onnx
```

同时文档也说明：OCR 资源不需要上传到代码仓库，`.gitignore` 已经忽略了这个目录，GitHub workflow 发布时会自动配置这些资源。

### 第五步：理解 pipeline 文件

打开：

```text
assets/resource/pipeline/my_task.json
```

当前内容很短：

```json
{
    "MyTask1": {},
    "MyTask2": {},
    "MyTask3": {},
    "MyTask4": {
        "recognition": "Custom",
        "custom_recognition": "my_reco_222",
        "action": "Custom",
        "custom_action": "my_action_111"
    },
    "OcrTask": {}
}
```

这里的每个顶层名字都是一个节点，比如 `MyTask1`、`MyTask4`。

MaaFramework 的 pipeline 文档明确说明：任务流水线用 JSON 描述，由若干节点组成。节点常见核心字段包括：

- `recognition`：识别方式，例如 OCR、TemplateMatch、Custom。
- `action`：动作，例如 Click、Swipe、Custom。
- `next`：后续节点列表。
- `on_error`：失败后进入的节点列表。

当前模板项目中，`MyTask1`、`MyTask2`、`MyTask3` 都是空节点，适合你先用来理解“入口名字如何对应”。`MyTask4` 是自定义识别和自定义动作示例。

### 第六步：把 `interface.json` 和 pipeline 对起来看

在 `assets/interface.json` 里找 `task`。

你会看到任务的 `entry` 指向：

```text
MyTask1
MyTask2
MyTask3
MyTask4
```

再到：

```text
assets/resource/pipeline/my_task.json
```

你能找到同名节点。

这就是你理解任务流程的第一条线：

```text
assets/interface.json 的 task.entry
        ↓
assets/resource/pipeline/*.json 里的同名节点
```

### 第七步：暂时只了解 `agent/`

当前项目有：

```text
agent/main.py
agent/my_reco.py
agent/my_action.py
```

其中：

- `agent/my_reco.py` 注册了 `my_reco_222`。
- `agent/my_action.py` 注册了 `my_action_111`。
- `assets/resource/pipeline/my_task.json` 的 `MyTask4` 正好引用了这两个名字。

MaaFramework 的 `Custom&Agent` 文档明确说明：

- Custom 用来扩展内置识别算法和动作类型无法满足的需求。
- AgentServer 用来注册并执行自定义识别与自定义动作代码。
- AgentClient 是 UI 的一部分，负责与 AgentServer 通信。

不过，当前 `assets/interface.json` 里的 agent 配置是注释掉的：

```jsonc
// "agent": {
//     "child_exec": "python",
//     "child_args": [
//         "./agent/main.py"
//     ]
// },
```

所以现在先不要急着改 `agent/`。等你能看懂普通 pipeline 后，再研究 Custom 和 Agent。

### 第八步：先做检查，不急着打包

当前项目没有在 `package.json` 里提供明确的 `npm run xxx` 启动命令。

当前项目明确能找到的检查方式来自：

```text
.github/workflows/check.yml
```

里面的检查命令是：

```powershell
npm ci
npx @nekosu/maa-tools check
python -m pip install --upgrade pip
python -m pip install --upgrade maafw --pre
python -m pip install jsonschema==4.26.0 referencing==0.37.0
python tools/validate_schema.py --schema-dir deps/tools --resource-dirs assets/resource --exclude-dirs assets/resource/announcement --interface-files assets/interface.json
```

你本地可以先从这两个检查开始：

```powershell
npx @nekosu/maa-tools check
python tools/validate_schema.py --schema-dir deps/tools --resource-dirs assets/resource --exclude-dirs assets/resource/announcement --interface-files assets/interface.json
```

如果提示缺少 Node 或 Python 依赖，再按报错补：

```powershell
npm ci
python -m pip install jsonschema==4.26.0 referencing==0.37.0
```

如果遇到 Python 命令没有反应，模板项目 FAQ 明确提到：Win10 / Win11 可能自带的是 Python 安装器，不是真正可用的 Python，需要安装真实 Python 并处理环境变量。

### 第九步：记录报错

如果运行失败，不要只写“报错了”。请记录到：

```text
docs/QUESTIONS_AND_NOTES.md
```

建议记录：

```text
日期：
执行命令：
执行目录：
报错原文：
我当时想做什么：
可能原因：
最小修复方案：
结果：
```

模板项目 FAQ 也明确建议：提问时要具体指出困惑的文档章节、分享项目文件中的具体内容、提供完整报错信息。

### 第十步：第一次只做最小修改

第一次不要改复杂逻辑。建议只改：

```text
assets/interface.json
```

可以改这些低风险内容：

- `name`
- `description`
- `contact`
- `welcome`
- 某个任务的显示名称

改完后运行检查：

```powershell
npx @nekosu/maa-tools check
python tools/validate_schema.py --schema-dir deps/tools --resource-dirs assets/resource --exclude-dirs assets/resource/announcement --interface-files assets/interface.json
```

如果检查通过，再考虑下一步改 pipeline。

## 2. 第一次适合改什么，后面再改什么

### 第一阶段：只改说明和显示信息

适合修改：

```text
docs/QUESTIONS_AND_NOTES.md
docs/PROJECT_EXPLANATION.md
docs/manual.md
assets/interface.json
```

适合做的事：

- 记录问题。
- 修改项目名和说明。
- 修改任务在 UI 中显示的名称。

验证方式：

```powershell
npx @nekosu/maa-tools check
python tools/validate_schema.py --schema-dir deps/tools --resource-dirs assets/resource --exclude-dirs assets/resource/announcement --interface-files assets/interface.json
```

风险：

- `assets/interface.json` 是 JSONC。括号、逗号、引号写错会导致校验失败。
- 当前部分中文文件在普通 PowerShell 输出里可能乱码，编辑时要注意文件编码。

### 第二阶段：改 pipeline

适合修改：

```text
assets/resource/pipeline/my_task.json
```

适合做的事：

- 给空节点增加简单字段。
- 学习 `recognition`、`action`、`next` 的关系。
- 把一个任务从“空节点”改成有明确识别和动作的节点。

先不要做的事：

- 一上来就写很长的任务链。
- 一上来就写复杂 `on_error`。
- 一上来就写 Custom。

验证方式：

```powershell
npx @nekosu/maa-tools check
python tools/validate_schema.py --schema-dir deps/tools --resource-dirs assets/resource --exclude-dirs assets/resource/announcement --interface-files assets/interface.json
```

风险：

- `interface.json` 的 `task.entry` 必须能在 pipeline 中找到同名节点。
- pipeline 节点名改了，引用它的地方也要一起改。

### 第三阶段：新增或替换资源

适合修改：

```text
assets/resource/image/
assets/resource/model/ocr/
```

MaaFramework 快速开始文档明确说明：

- `image/` 主要放模板匹配、特征检测等图片素材。
- 图片会按 pipeline 中的 `template` 等字段读取。
- 图片建议基于无损原图缩放到 720p 后再裁剪。
- `model/ocr/` 放 OCR 模型文件。

模板项目 `how_to_develop.md` 明确说明 OCR 资源放在：

```text
assets/resource/model/ocr/
```

且应包含：

```text
det.onnx
keys.txt
rec.onnx
```

验证方式：

- 先跑 schema 和 maa-tools 检查。
- 如果是图片识别效果，需要使用开发工具调试。MaaFramework 快速开始文档推荐 VS Code 插件、MaaPipelineEditor、MFA 工具箱、图片裁剪及 ROI 获取工具。

当前未找到明确说明：

- 当前模板项目里 `assets/resource/image/empty.png` 被哪个节点引用。

### 第四阶段：再研究 Custom 和 Agent

适合修改：

```text
agent/my_reco.py
agent/my_action.py
agent/main.py
assets/interface.json
assets/resource/pipeline/my_task.json
```

你应该先理解这条关系：

```text
pipeline 节点 MyTask4
        ↓
custom_recognition: my_reco_222
custom_action: my_action_111
        ↓
agent/my_reco.py
agent/my_action.py
```

MaaFramework 文档明确说明：当内置识别算法或动作类型无法满足需求时，可以使用 Custom；AgentServer 用于注册和执行自定义识别与动作代码。

风险：

- 当前 `assets/interface.json` 中 agent 配置是注释状态。
- Agent 需要 UI / Client 配合启动和通信。
- 如果 Python 环境或 `maafw` 包没有配置好，会先遇到环境问题。

当前未找到明确说明：

- 当前模板项目在本地如何完整启动 agent 并执行 `MyTask4`。

### 暂时不要改的内容

新手阶段暂时不要改：

```text
tools/install.py
tools/configure.py
tools/validate_schema.py
.github/workflows/*.yml
deps/tools/*.schema.json
package-lock.json
pnpm-lock.yaml
.gitmodules
D:\projects\MaaSource\MaaFramework
```

原因：

- `tools/` 影响安装、OCR 配置、schema 校验。
- `.github/workflows/` 影响 GitHub Actions 打包和检查。
- `deps/tools/*.schema.json` 是 schema 校验规则。
- 锁文件影响依赖版本。
- MaaFramework 是框架源码，不是当前模板项目的开发对象。

## 3. 当前任务流程应该怎么理解

先记住一句话：

```text
interface.json 负责告诉 UI 有哪些任务；pipeline JSON 负责描述任务怎么执行。
```

### `interface.json` 里的任务

路径：

```text
assets/interface.json
```

当前能看到 4 个任务入口：

```text
MyTask1
MyTask2
MyTask3
MyTask4
```

MaaFramework ProjectInterface V2 文档明确说明：`task.entry` 是 pipeline 中起点 Node 的名称。

所以读任务时，你应该这样读：

1. 在 `assets/interface.json` 找 `task`。
2. 看某个任务的 `entry`。
3. 到 `assets/resource/pipeline/my_task.json` 找同名节点。

### pipeline 节点怎么执行

MaaFramework pipeline 文档明确说明：

- 任务通过指定入口节点启动。
- 当前节点执行动作后，会去识别 `next` 列表里的节点。
- `next` 是按顺序检测的，识别到一个就执行这个节点的动作。
- 如果 `next` 一直没有命中，直到超时，会进入 `on_error`。
- 如果 `next` 为空，任务流程会结束。
- 外部调用 `post_stop` 或执行 `StopTask` 也会终止任务。

这对你现在最重要的理解是：

```text
节点不是按文件顺序自动一个个执行。
真正决定后续走向的是 entry、next、on_error。
```

当前模板的 `my_task.json` 很简单，还没有 `next`，所以它更像一个“骨架示例”。

### `MyTask4` 为什么特殊

当前 `MyTask4` 是：

```json
"MyTask4": {
    "recognition": "Custom",
    "custom_recognition": "my_reco_222",
    "action": "Custom",
    "custom_action": "my_action_111"
}
```

它对应：

```text
agent/my_reco.py      注册 my_reco_222
agent/my_action.py    注册 my_action_111
```

MaaFramework 的 Custom & Agent 文档明确说明：Custom 用于内置识别和动作无法满足需求时；AgentServer 用于注册并执行自定义识别与动作。

所以你可以把 `MyTask4` 理解成：模板提供的“自定义逻辑示例入口”。

但在你还没跑通普通 pipeline 前，不建议先从它开始。

### 修改任务流程时先看什么

按顺序看：

```text
assets/interface.json
assets/resource/pipeline/my_task.json
docs/zh_cn/develop/how_to_develop.md
D:\projects\MaaSource\MaaFramework\docs\zh_cn\3.1-任务流水线协议.md
D:\projects\MaaSource\MaaFramework\docs\zh_cn\3.3-ProjectInterfaceV2协议.md
```

不要先去改 MaaFramework 源码。

## 4. 当前资源文件应该怎么理解

### 资源目录在哪里

当前模板项目资源目录是：

```text
assets/resource/
```

`assets/interface.json` 中配置的资源路径是：

```json
"path": [
    "./resource"
]
```

ProjectInterface V2 文档明确说明：`resource.path` 是相对于 `interface.json` 所在目录的路径；资源不仅包括 pipeline，也包括 image 和 model，所以不要直接把路径写到 `pipeline` 目录。

### pipeline 资源

当前 pipeline 资源：

```text
assets/resource/pipeline/my_task.json
```

MaaFramework 快速开始文档明确说明：`pipeline` 中的文件包含主要脚本执行逻辑，会递归读取目录中的 JSON 文件。

所以后续你可以先改这个文件。熟悉后，也可以在 `assets/resource/pipeline/` 下增加新的 JSON 文件。

### 图片资源

当前图片资源：

```text
assets/resource/image/empty.png
```

MaaFramework 快速开始文档明确说明：`image` 中的文件主要用于 pipeline 所需的模板匹配图片、特征检测图片等，会按 pipeline 中的 `template` 等字段读取。

当前未找到明确说明：

- `empty.png` 在当前 pipeline 中被哪个节点使用。

所以现在不要删除它，先保留。

### OCR 资源

模板项目 `how_to_develop.md` 明确要求下载 OCR 资源到：

```text
assets/resource/model/ocr/
```

需要包含：

```text
det.onnx
keys.txt
rec.onnx
```

FAQ 也明确提到：如果 OCR 一直没有识别结果，并报错 `Failed to load det or rec`、`ocrer_ is null`，通常就是前面 OCR 资源步骤没有正确完成。

当前项目目录里未找到明确的 OCR 模型文件。

### 替换或新增资源时怎么做

建议顺序：

1. 图片放到 `assets/resource/image/`。
2. OCR 模型放到 `assets/resource/model/ocr/`。
3. pipeline 放到 `assets/resource/pipeline/`。
4. 在 pipeline 里引用图片或配置 OCR。
5. 运行检查。

不要直接改：

```text
deps/tools/*.schema.json
```

这些是校验规则，不是你的业务资源。

## 5. 运行、调试、打包怎么理解

### 本地运行

MaaFramework 快速开始文档明确说明：通用 UI 通过 `interface.json` 知道资源文件放在哪里、有哪些任务可以执行，然后帮你跑起来。

当前模板项目没有找到明确的本地“一键启动完整 UI”命令。

不过 MaaFramework 快速开始文档和模板项目文档都提到开发工具，包括：

- VS Code 插件 `Maa Pipeline Support`
- MaaPipelineEditor
- MaaDebugger
- MFAAvalonia / MFA 工具箱
- MaaPiCli 模式执行

当前 `.vscode/extensions.json` 也推荐安装：

```text
nekosu.maa-support
ms-python.black-formatter
davidanson.vscode-markdownlint
```

### 调试

MaaFramework 快速开始文档明确说明，多数工具会生成：

```text
config/maa_option.json
```

其中常见调试选项包括：

- `logging`：保存日志，生成 `debug/maafw.log`。
- `save_draw`：保存图像识别可视化结果。
- `stdout_level`：控制台日志等级。
- `save_on_error`：任务失败时保存当前截图。

模板项目 FAQ 也提到，提问时最好带上：

```text
debug/maa.log
```

### 打包

模板项目 `how_to_develop.md` 明确说明：本模板附带 GitHub Actions 工作流配置文件：

```text
.github/workflows/install.yml
```

CI 检测到 tag 会自动打包和发布，默认会将 MFAAvalonia 与项目一同打包和发版。

它给出的发布示例是：

```bash
git tag v1.0.0
git push origin v1.0.0
```

但你现在是学习阶段，不建议急着打包发布。先把项目结构、资源、pipeline 看懂。

## 6. 你现在最适合做的最小练习

建议做这个练习：

1. 打开 `assets/interface.json`。
2. 找到项目 `name` 或 `description`。
3. 改一个很小的文字。
4. 运行检查。
5. 把运行命令和结果记录到 `docs/QUESTIONS_AND_NOTES.md`。

然后再做第二个练习：

1. 打开 `assets/interface.json`。
2. 找到 `task` 中某个任务的显示名称。
3. 改一个任务名称。
4. 确认 `entry` 不要改。
5. 再运行检查。

第三个练习再考虑 pipeline：

1. 打开 `assets/resource/pipeline/my_task.json`。
2. 先只观察 `MyTask1`、`MyTask4` 的差别。
3. 阅读 MaaFramework 的 `3.1-任务流水线协议.md`。
4. 不急着加复杂节点。

## 7. 目前还没有明确答案的事

下面这些内容，我在当前项目文件和已读文档中没有找到明确说明：

- 当前模板项目在本地如何一键启动完整通用 UI。
- 当前项目的 `agent/main.py` 如何在本地被当前 UI / Client 自动启动。
- `assets/resource/image/empty.png` 当前被哪个 pipeline 节点引用。
- 当前项目是否已经包含 OCR 模型文件。

这些不是你现在必须解决的问题。你现在最应该做的是先看懂：

```text
assets/interface.json
assets/resource/pipeline/my_task.json
assets/resource/
```

等这三块看懂后，再去跑工具、调试图片识别、研究 Agent。
