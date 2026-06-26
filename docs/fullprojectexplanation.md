# MaaFramework 源码整体认知文档

本文档写给后续帮助开发 Maa 项目的 AI Agent。目标是建立对 `D:\projects\MaaSource\MaaFramework` 框架源码的整体认知，明确 MaaFramework 与当前模板项目 `D:\projects\MaaTest\MaaTest` 的边界。

重要边界：

- `D:\projects\MaaTest\MaaTest` 是当前模板项目，用于学习和后续开发 MaaPracticeBoilerplate 项目。
- `D:\projects\MaaSource\MaaFramework` 是 MaaFramework 框架源码，只用于参考、解释和理解底层机制。
- 除非用户明确要求“修改 MaaFramework 源码”或“为 MaaFramework 提交贡献”，否则不要修改 `D:\projects\MaaSource\MaaFramework`。

信息分类：

- 源码事实：来自 MaaFramework 实际源码路径。
- 文档说明：来自 MaaFramework README 或 docs。
- 模板事实：来自当前模板项目 maatest。
- 推断：根据命名、目录结构、接口关系或经验判断，必须标注“这是推断”。

## 1. MaaFramework 源码总体定位

### MaaFramework 是什么

文档说明：

- `README_en.md` 说明 MaaFramework 是一个基于图像识别技术的自动化黑盒测试框架。
- `include/MaaFramework/MaaAPI.h` 的 API 注释说明 MaaFramework 是用于在 Windows、Android 模拟器等环境中创建自动化测试的工具集。
- 根目录 `CMakeLists.txt` 显示项目名是 `MaaFw`，使用 CMake 组织构建。

### 它和 MaaPracticeBoilerplate 的关系

文档说明：

- MaaFramework 文档 `docs/en_us/1.1-QuickStarted.md` 明确提到 MaaPracticeBoilerplate 是项目模板，可用于纯 JSON 低代码或 JSON + 自定义逻辑扩展开发。
- 当前模板项目 `README.md` 标题是 `MaaPracticeBoilerplate`，并链接 MaaFramework。

模板事实：

- 当前模板项目 `assets/interface.json` 定义 ProjectInterface。
- 当前模板项目 `assets/resource/pipeline/my_task.json` 定义 pipeline 示例。
- 当前模板项目 `.github/workflows/install.yml` 会从 `MaaXYZ/MaaFramework` 下载发布包。
- 当前模板项目 `tools/install.py` 要求 `deps/bin` 存在，否则提示需要先下载 MaaFramework 到 `deps`。

### 它在当前开发流程中的作用

文档说明：

- MaaFramework 提供底层 API、资源加载、任务执行、图像识别、控制器、Agent、语言绑定等能力。
- MaaFramework 文档建议通过 `interface.json` 告诉通用 UI 资源在哪里、哪些任务可执行。

模板事实：

- 当前模板项目通过 `assets/interface.json` 声明 controller、resource、task、option。
- 当前模板项目通过 `assets/resource/pipeline/my_task.json` 声明任务节点。

这是推断：

- 当前模板项目开发时，通常改模板层的 `interface.json`、`assets/resource/`、`agent/`，框架层由 MaaFramework 发布包提供运行能力。

### 为什么当前不建议直接修改它

原因：

- MaaFramework 是底层框架本体，改动影响范围大。
- 当前用户目标是学习 MaaPracticeBoilerplate 模板项目，不是参与 MaaFramework 框架开发。
- 修改框架源码会让问题定位从“模板配置是否正确”扩大到“框架行为是否被改坏”。

### 后续 Agent 何时可以参考它

可以参考 MaaFramework 源码的情况：

- 解释 pipeline 如何解析。
- 解释任务如何执行。
- 解释识别、动作、控制器、资源加载的底层机制。
- 对照 schema 或文档确认字段含义。
- 排查模板项目无法解释的运行现象。

### 后续 Agent 何时不应该修改它

不应修改 MaaFramework 的情况：

- 用户只是想改模板项目功能。
- 用户只是想新增 pipeline 节点、图片资源、OCR 资源或 agent 逻辑。
- 用户只是想跑通当前模板项目。
- 用户没有明确说要修改框架源码或给 MaaFramework 提 PR。

## 2. 源码目录结构总览

### 根目录

路径：

```text
D:\projects\MaaSource\MaaFramework
```

主要文件：

```text
README.md
README_en.md
CMakeLists.txt
CMakePresets.json
LICENSE.md
AGENTS.md
```

作用：

- `README_en.md` / `README.md`：项目介绍、文档入口、社区项目列表。
- `CMakeLists.txt`：顶层构建配置，定义控制器、绑定、测试、样例等构建选项。
- `CMakePresets.json`：CMake preset 配置。

是否属于框架核心：

- `CMakeLists.txt` 属于构建核心。
- README 属于文档入口。

是否建议新手阅读：

- 建议读 `README_en.md`、`docs/en_us/1.1-QuickStarted.md`。
- 不建议新手一开始修改 `CMakeLists.txt`。

和模板项目开发的关系：

- 了解框架定位和构建选项即可，模板功能开发通常不改这里。

### `include/`

路径：

```text
include/
```

主要子目录：

```text
include/MaaFramework/
include/MaaControlUnit/
include/MaaToolkit/
include/MaaAgentClient/
include/MaaAgentServer/
```

作用：

- 对外公开 API 头文件。
- `include/MaaFramework/MaaAPI.h` 聚合 MaaFramework 主要 API。
- `include/MaaFramework/Instance/MaaTasker.h` 提供任务提交、等待、状态查询等 API。
- `include/MaaFramework/Instance/MaaResource.h` 提供资源加载、pipeline override、自定义识别/动作注册等 API。
- `include/MaaFramework/Instance/MaaController.h` 提供 ADB、Win32、macOS、Dbg、Replay、Record、PlayCover、WlRoots、KWin、Gamepad 等控制器创建和操作 API。

是否属于框架核心：

- 是，对外接口核心。

是否建议新手阅读：

- 建议后续 Agent 阅读。
- 普通模板新手只需知道这些是底层接口，不需要一开始深入。

是否建议新手修改：

- 不建议。

和模板项目开发的关系：

- 模板项目通常通过通用 UI、Python binding 或 MaaFramework 发布包间接使用这些 API。

### `source/`

路径：

```text
source/
```

主要子目录：

```text
source/MaaFramework/
source/MaaToolkit/
source/MaaPiCli/
source/binding/
source/Common/
source/MaaUtils/
source/MaaAdbControlUnit/
source/MaaWin32ControlUnit/
source/MaaMacOSControlUnit/
source/MaaAndroidNativeControlUnit/
source/MaaCustomControlUnit/
source/MaaDbgControlUnit/
source/MaaReplayControlUnit/
source/MaaRecordControlUnit/
source/MaaAgentServer/
source/MaaAgentClient/
```

作用：

- 框架主体实现、工具、命令行 Client、绑定层、公共代码、平台控制单元和 Agent 实现。

是否属于框架核心：

- `source/MaaFramework/` 是核心。
- 多个 `*ControlUnit` 是平台控制能力核心。
- `source/binding/` 是语言绑定层。
- `source/MaaPiCli/` 是官方命令行 ProjectInterface Client。

是否建议新手阅读：

- 后续 Agent 可按问题阅读。
- 新手不建议从这里开始。

是否建议新手修改：

- 不建议。

### `source/MaaFramework/`

路径：

```text
source/MaaFramework/
```

主要子目录：

```text
API/
Base/
Controller/
Global/
Resource/
Task/
Tasker/
Vision/
```

作用：

- `API/`：C API 实现，例如 `source/MaaFramework/API/MaaFramework.cpp`。
- `Controller/`：控制器代理，例如 `ControllerAgent`。
- `Resource/`：资源加载、pipeline 解析、OCR/ONNX/图片资源管理。
- `Task/`：任务节点、识别任务、动作任务、pipeline 任务。
- `Tasker/`：任务调度和运行入口。
- `Vision/`：图像识别算法实现。
- `Global/`：全局选项、插件管理。

是否属于框架核心：

- 是。

是否建议新手阅读：

- 后续 Agent 需要解释底层行为时阅读。
- 模板新手不建议先读。

是否建议新手修改：

- 不建议，除非用户明确要参与框架开发。

### `docs/`

路径：

```text
docs/
```

主要子目录：

```text
docs/en_us/
docs/zh_cn/
docs/static/
```

重点文件：

```text
docs/en_us/1.1-QuickStarted.md
docs/en_us/1.3-Custom&Agent.md
docs/en_us/2.1-Integration.md
docs/en_us/2.2-IntegratedInterfaceOverview.md
docs/en_us/2.3-CallbackProtocol.md
docs/en_us/2.4-ControlMethods.md
docs/en_us/3.1-PipelineProtocol.md
docs/en_us/3.3-ProjectInterfaceV2.md
docs/en_us/4.1-BuildGuide.md
```

作用：

- 解释快速开始、集成方式、API、回调、控制方式、pipeline 协议、ProjectInterface 协议、构建方式等。

是否属于框架核心：

- 文档，不是运行核心，但对理解框架很重要。

是否建议新手阅读：

- 建议阅读快速开始、pipeline 协议、ProjectInterface 协议。

是否建议新手修改：

- 当前不建议。

### `tools/`

路径：

```text
tools/
```

重点文件：

```text
tools/pipeline.schema.json
tools/interface.schema.json
tools/interface_config.schema.json
tools/interface_import.schema.json
tools/custom.action.schema.json
tools/custom.recognition.schema.json
tools/migrate_pipeline_v5.py
tools/ImageCropper/
tools/Doxygen/
```

作用：

- JSON schema。
- pipeline 迁移脚本。
- 图片裁剪和 ROI 工具。
- Doxygen 文档工具。

是否属于框架核心：

- 辅助工具和校验规则，不是运行核心。

是否建议新手阅读：

- 可以参考 schema 和 ImageCropper 说明。

是否建议新手修改：

- 不建议。

和模板项目开发的关系：

- 当前模板项目 `deps/tools/*.schema.json` 与这些 schema 对应。

### `sample/`

路径：

```text
sample/
```

重点文件：

```text
sample/interface.json
sample/resource/pipeline/sample.json
sample/python/demo1.py
sample/python/demo3_agent.py
sample/nodejs/main.ts
sample/cpp/main.cpp
sample/csharp/
```

作用：

- 提供 ProjectInterface、pipeline、Python、NodeJS、C++、C# 示例。

是否属于框架核心：

- 不是核心，是示例。

是否建议新手阅读：

- 建议后续 Agent 和新手按需参考。

是否建议新手修改：

- 不建议在 MaaFramework 源码内修改；可参考后在模板项目中实现。

### `test/`

路径：

```text
test/
```

重点目录：

```text
test/python/
test/nodejs/
test/pipeline/
test/schema/
test/agent/
test/macos_test/
test/win32_test/
```

作用：

- 测试 Python/NodeJS 绑定、pipeline、schema、agent、平台能力等。

是否属于框架核心：

- 测试代码，不是运行核心。

是否建议新手阅读：

- 后续 Agent 排查框架行为时可参考。

是否建议新手修改：

- 当前不建议。

### `3rdparty/`

路径：

```text
3rdparty/
```

作用：

- 第三方依赖和相关 CMake 配置。

是否属于框架核心：

- 属于构建依赖层。

是否建议新手阅读：

- 不建议作为入门阅读。

是否建议新手修改：

- 不建议。

### `cmake/`

路径：

```text
cmake/
```

作用：

- CMake package 配置模板，例如 `MaaFrameworkConfig.cmake.in`、`MaaAgentServerConfig.cmake.in`。

是否属于框架核心：

- 构建和安装配置核心。

是否建议新手阅读：

- 只在处理构建集成问题时阅读。

是否建议新手修改：

- 不建议。

### `.github/workflows/`

路径：

```text
.github/workflows/
```

当前看到的 workflow：

```text
build.yml
deploy-manual.yml
deploy-website.yml
format.yml
meta.yml
mirrorchyan.yml
mirrorchyan_release_note.yml
release.yml
test.yml
```

作用：

- 构建、测试、发布、格式检查、文档部署等 GitHub Actions 流程。

是否属于框架核心：

- 工程流程层，不是运行核心。

是否建议新手阅读：

- 需要了解框架 CI 时阅读。

是否建议新手修改：

- 当前不建议。

## 3. 框架核心模块理解

### 图像识别相关模块

源码事实：

```text
source/MaaFramework/Vision/
source/MaaFramework/Task/Component/Recognizer.h
source/MaaFramework/Task/Component/Recognizer.cpp
```

`source/MaaFramework/Vision/` 中看到：

```text
TemplateMatcher
FeatureMatcher
ColorMatcher
OCRer
NeuralNetworkClassifier
NeuralNetworkDetector
VisionBase
VisionTypes
```

`Recognizer.h` 中有：

```text
template_match
feature_match
color_match
ocr
nn_classify
nn_detect
and_
or_
custom_recognize
```

结论：

- 图像识别算法主体在 `Vision/`。
- pipeline 执行时的识别分发在 `Task/Component/Recognizer.*`。

### 控制器 / 设备连接相关模块

源码事实：

对外 API：

```text
include/MaaFramework/Instance/MaaController.h
```

控制器代理：

```text
source/MaaFramework/Controller/ControllerAgent.h
source/MaaFramework/Controller/ControllerAgent.cpp
```

平台控制单元：

```text
source/MaaAdbControlUnit/
source/MaaWin32ControlUnit/
source/MaaMacOSControlUnit/
source/MaaAndroidNativeControlUnit/
source/MaaPlayCoverControlUnit/
source/MaaGamepadControlUnit/
source/MaaWlRootsControlUnit/
source/MaaKWinControlUnit/
source/MaaDbgControlUnit/
source/MaaReplayControlUnit/
source/MaaRecordControlUnit/
source/MaaCustomControlUnit/
```

结论：

- `MaaController.h` 暴露控制器创建和操作 API。
- `ControllerAgent` 统一封装 click、swipe、screencap、shell、start_app、stop_app 等控制动作。
- 各平台 `*ControlUnit` 负责具体平台能力。

### 任务执行 / pipeline / 调度相关模块

源码事实：

```text
include/MaaFramework/Instance/MaaTasker.h
source/MaaFramework/Tasker/Tasker.h
source/MaaFramework/Tasker/Tasker.cpp
source/MaaFramework/Task/PipelineTask.h
source/MaaFramework/Task/PipelineTask.cpp
source/MaaFramework/Task/RecognitionTask.h
source/MaaFramework/Task/ActionTask.h
source/MaaFramework/Task/TaskBase.h
source/MaaFramework/Task/Context.h
```

`MaaTasker.h` 暴露：

```text
MaaTaskerPostTask
MaaTaskerPostRecognition
MaaTaskerPostAction
MaaTaskerWait
MaaTaskerStatus
MaaTaskerOverridePipeline
```

结论：

- `Tasker` 是任务提交和调度入口。
- `PipelineTask` 负责完整 pipeline 任务运行。
- `RecognitionTask` 和 `ActionTask` 支持单独识别或动作任务。

### 资源加载相关模块

源码事实：

```text
include/MaaFramework/Instance/MaaResource.h
source/MaaFramework/Resource/ResourceMgr.h
source/MaaFramework/Resource/ResourceMgr.cpp
source/MaaFramework/Resource/PipelineResMgr.h
source/MaaFramework/Resource/PipelineResMgr.cpp
source/MaaFramework/Resource/PipelineParser.h
source/MaaFramework/Resource/PipelineParser.cpp
source/MaaFramework/Resource/TemplateResMgr.h
source/MaaFramework/Resource/OCRResMgr.h
source/MaaFramework/Resource/ONNXResMgr.h
source/MaaFramework/Resource/DefaultPipelineMgr.h
```

`MaaResource.h` 暴露：

```text
MaaResourcePostBundle
MaaResourcePostOcrModel
MaaResourcePostPipeline
MaaResourcePostImage
MaaResourceOverridePipeline
MaaResourceOverrideImage
MaaResourceRegisterCustomRecognition
MaaResourceRegisterCustomAction
```

结论：

- `ResourceMgr` 是资源管理入口。
- `PipelineResMgr` 读取 pipeline 文件。
- `PipelineParser` 解析节点、识别、动作、next、wait 等字段。
- `TemplateResMgr`、`OCRResMgr`、`ONNXResMgr` 分别处理图片、OCR、ONNX 相关资源。

### API / include 对外接口

源码事实：

```text
include/MaaFramework/MaaAPI.h
include/MaaFramework/Instance/MaaTasker.h
include/MaaFramework/Instance/MaaResource.h
include/MaaFramework/Instance/MaaController.h
include/MaaFramework/Instance/MaaContext.h
include/MaaFramework/Global/MaaGlobal.h
include/MaaFramework/Utility/
```

实现：

```text
source/MaaFramework/API/MaaFramework.cpp
```

结论：

- 外部项目主要通过 `include/MaaFramework` 使用框架。
- API 实现会创建具体的 `Tasker`、`ResourceMgr`、`ControllerAgent`。

### 日志、工具、公共基础模块

源码事实：

```text
source/Common/
source/MaaUtils/
source/include/Common/
source/include/Utils/EventDispatcher.hpp
```

搜索结果显示多个文件使用：

```text
#include "MaaUtils/Logger.h"
LogError
LogWarn
LogInfo
LogDebug
```

结论：

- 日志接口主要来自 `MaaUtils/Logger.h`。
- 事件通知相关代码可看 `source/include/Utils/EventDispatcher.hpp`。
- 公共类型可看 `source/include/Common/MaaTypes.h` 和 `include/MaaFramework/MaaDef.h`。

### 绑定层或多语言接口

源码事实：

```text
source/binding/Python/
source/binding/NodeJS/
```

文档说明：

- `docs/en_us/2.1-Integration.md` 说明 C++、Python、NodeJS、CSharp、Golang、Java、Rust 等集成状态。
- 其中 Python 和 NodeJS 源码在当前仓库内：
  - `source/binding/Python`
  - `source/binding/NodeJS`

当前源码中还看到：

```text
source/binding/Python/maa/
source/binding/Python/maa/agent/agent_server.py
source/binding/NodeJS/src/apis/
```

结论：

- Python / NodeJS binding 是当前仓库内的实际源码。
- CSharp、Golang、Java、Rust 文档指向外部仓库，当前未在本地源码中完整看到。

## 4. 框架和模板的关系

### 模板项目通常通过什么方式使用 MaaFramework

文档说明：

- MaaFramework 快速开始文档提供三种方式：
  - 纯 JSON 低代码。
  - JSON + 自定义逻辑扩展，推荐。
  - 全代码开发。
- 文档明确提到 MaaPracticeBoilerplate 可作为模板起点。

模板事实：

- 当前模板项目包含 `assets/interface.json`。
- 当前模板项目包含 `assets/resource/pipeline/my_task.json`。
- 当前模板项目包含 `agent/my_reco.py`、`agent/my_action.py`、`agent/main.py`。
- 当前模板项目 `.github/workflows/install.yml` 下载 MaaFramework 发布包。

这是推断：

- 当前模板项目属于“JSON + 自定义逻辑扩展”方向的最小模板，因为它同时包含 pipeline 和 Python agent 示例。

### 模板 pipeline / resource / 配置可能如何被 MaaFramework 读取和执行

模板事实：

- `assets/interface.json` 的 `resource.path` 指向 `./resource`。
- `assets/interface.json` 的 `task.entry` 指向 pipeline 节点。
- `assets/resource/pipeline/my_task.json` 定义节点。

框架参考：

- `docs/en_us/3.3-ProjectInterfaceV2.md` 说明 `resource.path` 是资源加载路径，`task.entry` 是 pipeline 起始节点。
- `docs/en_us/1.1-QuickStarted.md` 说明资源目录下 `pipeline/` 文件包含主脚本执行逻辑，图片资源根据 pipeline 字段读取。
- `source/MaaFramework/Resource/ResourceMgr.*`、`PipelineResMgr.*`、`PipelineParser.*` 是资源和 pipeline 解析相关源码。
- `source/MaaFramework/Tasker/Tasker.*`、`Task/PipelineTask.*` 是任务执行相关源码。

这是推断：

- 通用 UI 或 MaaPiCli 先解析 `interface.json`，再根据用户选择加载 `resource.path`，然后根据 `task.entry` 调用框架任务执行。

### 哪些内容属于模板项目层

模板项目层：

```text
assets/interface.json
assets/resource/pipeline/*.json
assets/resource/image/*
assets/resource/model/ocr/*
agent/*.py
tools/install.py
tools/validate_schema.py
maatools.config.mts
```

说明：

- 这些是当前 MaaPracticeBoilerplate 模板项目内的配置、资源和辅助脚本。

### 哪些内容属于 MaaFramework 框架层

框架层：

```text
D:\projects\MaaSource\MaaFramework\include
D:\projects\MaaSource\MaaFramework\source
D:\projects\MaaSource\MaaFramework\tools
D:\projects\MaaSource\MaaFramework\docs
D:\projects\MaaSource\MaaFramework\sample
```

说明：

- 这些是框架源码、文档、schema、示例和工具。

### 后续开发优先修改模板层

规则：

- 改任务：优先改 `assets/resource/pipeline/`。
- 改资源：优先改 `assets/resource/image/`、`assets/resource/model/ocr/`。
- 改显示和任务入口：优先改 `assets/interface.json`。
- 改自定义识别/动作：优先改 `agent/`。
- 不要为了实现模板任务直接改 MaaFramework。

## 5. 后续 Agent 使用源码参考的规则

后续 Agent 必须遵守：

- 如果用户要做模板项目功能开发，应优先阅读当前项目 `D:\projects\MaaTest\MaaTest`，而不是直接改 MaaFramework。
- MaaFramework 源码只作为底层机制参考。
- 需要解释底层行为时，可以阅读 `D:\projects\MaaSource\MaaFramework` 源码。
- 需要修改模板 pipeline / resource 时，不应修改 MaaFramework。
- 只有当用户明确提出“修改框架源码”或“为 MaaFramework 提交贡献”时，才考虑修改 `D:\projects\MaaSource\MaaFramework`。
- 修改前必须说明依据来自哪些文件路径。
- 如果是推断，必须明确标注“这是推断”。
- 不得把推断当成事实。

## 6. 常见问题的源码参考入口

### pipeline 是如何被解析的

优先查看：

```text
docs/en_us/3.1-PipelineProtocol.md
source/MaaFramework/Resource/PipelineResMgr.h
source/MaaFramework/Resource/PipelineResMgr.cpp
source/MaaFramework/Resource/PipelineParser.h
source/MaaFramework/Resource/PipelineParser.cpp
tools/pipeline.schema.json
```

已确认：

- `PipelineParser` 有 `parse_node`、`parse_recognition`、`parse_action`、`parse_next` 等接口。
- `PipelineResMgr` 负责加载 pipeline 文件和目录。

### 任务是如何被执行的

优先查看：

```text
include/MaaFramework/Instance/MaaTasker.h
source/MaaFramework/Tasker/Tasker.h
source/MaaFramework/Tasker/Tasker.cpp
source/MaaFramework/Task/PipelineTask.h
source/MaaFramework/Task/PipelineTask.cpp
source/MaaFramework/Task/TaskBase.h
source/MaaFramework/Task/Context.h
```

已确认：

- `MaaTaskerPostTask` 是对外提交任务 API。
- `Tasker` 内部管理任务 runner。
- `PipelineTask` 负责 pipeline 任务运行。

### 图像识别是如何调用的

优先查看：

```text
source/MaaFramework/Task/Component/Recognizer.h
source/MaaFramework/Task/Component/Recognizer.cpp
source/MaaFramework/Vision/
```

已确认：

- `Recognizer` 按识别类型分发到 TemplateMatch、FeatureMatch、ColorMatch、OCR、神经网络和 Custom 等。

### 资源文件是如何加载的

优先查看：

```text
include/MaaFramework/Instance/MaaResource.h
source/MaaFramework/Resource/ResourceMgr.h
source/MaaFramework/Resource/ResourceMgr.cpp
source/MaaFramework/Resource/TemplateResMgr.h
source/MaaFramework/Resource/OCRResMgr.h
source/MaaFramework/Resource/ONNXResMgr.h
source/MaaFramework/Resource/DefaultPipelineMgr.h
```

已确认：

- `MaaResourcePostBundle`、`MaaResourcePostPipeline`、`MaaResourcePostImage`、`MaaResourcePostOcrModel` 是资源加载相关 API。

### API 是如何暴露给模板或外部项目的

优先查看：

```text
include/MaaFramework/MaaAPI.h
include/MaaFramework/Instance/
source/MaaFramework/API/MaaFramework.cpp
docs/en_us/2.1-Integration.md
docs/en_us/2.2-IntegratedInterfaceOverview.md
```

已确认：

- `MaaAPI.h` 聚合 `MaaTasker`、`MaaResource`、`MaaController`、`MaaContext` 等接口。
- `MaaFramework.cpp` 是 C API 实现入口之一。

### 日志如何输出

优先查看：

```text
source/MaaFramework/API/MaaFramework.cpp
source/MaaFramework/Resource/ResourceMgr.cpp
source/MaaFramework/Task/Component/Recognizer.cpp
source/include/Utils/EventDispatcher.hpp
```

已确认：

- 多个源码文件包含 `MaaUtils/Logger.h`。
- 源码中大量使用 `LogError`、`LogWarn`、`LogInfo`、`LogDebug`。

未找到明确说明：

- 本次未完整展开 `source/MaaUtils/Logger.h` 的实现细节。

### 错误如何处理

优先查看：

```text
source/MaaFramework/API/MaaFramework.cpp
source/MaaFramework/Resource/ResourceMgr.cpp
source/MaaFramework/Resource/PipelineResMgr.cpp
source/MaaFramework/Task/Component/Recognizer.cpp
docs/en_us/2.3-CallbackProtocol.md
```

已确认：

- API 层遇到空指针、平台不支持、控制单元创建失败等情况会 `LogError` 并返回空指针或失败。
- `CallbackProtocol` 文档包含 Pipeline Node 消息和错误处理建议。

### 平台或设备连接如何实现

优先查看：

```text
include/MaaFramework/Instance/MaaController.h
source/MaaFramework/Controller/ControllerAgent.h
source/MaaAdbControlUnit/
source/MaaWin32ControlUnit/
source/MaaMacOSControlUnit/
source/MaaAndroidNativeControlUnit/
source\MaaPlayCoverControlUnit/
source\MaaWlRootsControlUnit/
source\MaaKWinControlUnit/
source\MaaGamepadControlUnit/
docs/en_us/2.4-ControlMethods.md
```

已确认：

- `MaaController.h` 暴露多种平台控制器创建函数。
- `ControllerAgent` 封装具体动作提交和截图缓存。

### Python / 其他语言绑定如何工作

优先查看：

```text
docs/en_us/2.1-Integration.md
source/binding/Python/
source/binding/Python/maa/
source/binding/Python/maa/agent/agent_server.py
source/binding/NodeJS/
source/binding/NodeJS/src/apis/
sample/python/
sample/nodejs/
```

已确认：

- Python binding README 写了安装命令 `python -m pip install maafw`。
- NodeJS binding 在 `source/binding/NodeJS`。
- 文档说明 CSharp、Golang、Rust 等也有集成方式，但部分在外部仓库。

### 构建系统如何组织

优先查看：

```text
CMakeLists.txt
CMakePresets.json
source/CMakeLists.txt
source/MaaFramework/CMakeLists.txt
cmake/
.github/workflows/build.yml
.github/workflows/test.yml
.github/workflows/release.yml
docs/en_us/4.1-BuildGuide.md
```

已确认：

- 顶层 `CMakeLists.txt` 定义了控制器、绑定、Agent、样例、测试等构建选项。
- `source/MaaFramework/CMakeLists.txt` 构建 `MaaFramework` 共享库，并链接 OpenCV、ONNXRuntime、fastdeploy_ppocr 等。

## 7. 信息可信度规则

后续 Agent 必须区分：

### 来自 MaaFramework 源码的事实

示例：

- `source/MaaFramework/Resource/PipelineParser.h` 存在 `parse_node`、`parse_recognition`、`parse_action`。
- `include/MaaFramework/Instance/MaaTasker.h` 暴露 `MaaTaskerPostTask`。
- `include/MaaFramework/Instance/MaaResource.h` 暴露 `MaaResourcePostPipeline`。
- `source/MaaFramework/Vision/` 包含 OCR、模板匹配、特征匹配、颜色匹配等实现文件。

### 来自 MaaFramework README / docs 的说明

示例：

- `README_en.md` 说明 MaaFramework 是基于图像识别的自动化黑盒测试框架。
- `docs/en_us/1.1-QuickStarted.md` 说明三种开发方式。
- `docs/en_us/3.1-PipelineProtocol.md` 说明 pipeline 节点和执行逻辑。
- `docs/en_us/3.3-ProjectInterfaceV2.md` 说明 `interface.json`、`resource.path`、`task.entry`。

### 来自模板项目 maatest 的事实

示例：

- `assets/interface.json` 中 `interface_version` 是 2。
- `assets/interface.json` 中资源路径是 `./resource`。
- `assets/resource/pipeline/my_task.json` 中定义了 `MyTask1` 到 `MyTask4`。
- `agent/my_action.py` 注册了 `my_action_111`。
- `agent/my_reco.py` 注册了 `my_reco_222`。

### Agent 根据命名或经验做出的推断

必须标注“这是推断”。

示例：

- “模板项目执行任务时由 UI / Client 解析 `interface.json` 并调用 MaaFramework”是推断，除非结合具体运行日志或 Client 源码证明。
- “某个图片会被某个 pipeline 节点使用”必须有 `template` 字段或源码引用，否则只能写未找到明确说明。

## 8. 未确认事项

本次阅读后仍未完全确认：

- 当前模板项目如何在本地启动完整通用 UI：未找到明确说明。
- 当前模板项目的 `agent/main.py` 在本地如何由 UI / Client 自动启动：模板中只看到注释示例，未找到明确说明。
- 当前模板项目 `assets/resource/image/empty.png` 被哪个节点引用：未找到明确说明。
- 当前模板项目是否已经包含 OCR 模型：未找到明确说明。
- MaaFramework 日志系统 `MaaUtils/Logger.h` 的完整实现细节：本次未展开阅读。
- MaaFramework 中 ProjectInterface 与具体通用 UI 的完整调用链：已看到 `source/MaaPiCli` 作为命令行 Client，但当前模板项目没有明确说明本地使用哪个 Client。
- CSharp、Golang、Rust 等绑定的完整源码：文档指向外部仓库，当前本地源码未完整包含。

## 9. 对后续 Agent 的最小工作建议

当用户要开发当前模板项目时：

1. 先读 `D:\projects\MaaTest\MaaTest\assets\interface.json`。
2. 再读 `D:\projects\MaaTest\MaaTest\assets\resource\pipeline\my_task.json`。
3. 如果涉及自定义逻辑，再读 `D:\projects\MaaTest\MaaTest\agent\*.py`。
4. 只有解释底层机制时，再读 `D:\projects\MaaSource\MaaFramework` 对应源码。
5. 不要修改 MaaFramework，除非用户明确要求。
