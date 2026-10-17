<p align="center">
  <picture>
    <source media="(prefers-color-scheme: dark)" srcset="docs/logo/dark.svg">
    <source media="(prefers-color-scheme: light)" srcset="docs/logo/light.svg">
    <img alt="HyperFrames" src="docs/logo/light.svg" width="300">
  </picture>
</p>

<p align="center">
  <a href="https://www.npmjs.com/package/hyperframes"><img src="https://img.shields.io/npm/v/hyperframes.svg?style=flat" alt="npm version"></a>
  <a href="https://www.npmjs.com/package/hyperframes"><img src="https://img.shields.io/npm/dm/hyperframes.svg?style=flat" alt="npm downloads"></a>
  <a href="LICENSE"><img src="https://img.shields.io/badge/license-Apache%202.0-blue.svg" alt="License"></a>
  <a href="https://nodejs.org"><img src="https://img.shields.io/badge/node-%3E%3D22-brightgreen" alt="Node.js"></a>
  <a href="https://discord.gg/EbK98HBPdk"><img src="https://img.shields.io/badge/Discord-Join-5865F2?logo=discord&logoColor=white" alt="Discord"></a>
</p>

<p align="center"><b>写 HTML，渲染视频。为智能体而生。</b></p>

<p align="center">
  <a href="https://www.hyperframes.dev/studio">Studio</a> |
  <a href="https://hyperframes.heygen.com/quickstart">Quickstart</a> |
  <a href="https://hyperframes.heygen.com/showcase">Showcase</a> |
  <a href="https://www.hyperframes.dev/">Playground</a> |
  <a href="https://hyperframes.heygen.com/catalog/blocks/data-chart">Catalog</a> |
  <a href="https://hyperframes.heygen.com/introduction">Docs</a> |
  <a href="https://discord.gg/EbK98HBPdk">Discord</a>
</p>

<p align="center">
  <img src="https://github.com/user-attachments/assets/f6ff9fae-f33d-4f68-bd54-f3ed4ba6473b" alt="HyperFrames app opening from the Dock: native Mac and Linux app now available" width="800">
</p>

HyperFrames 是一个开源框架，可将 HTML、CSS、媒体和可定位（seekable）动画转换为确定性的 MP4 视频。你可以在本地用 CLI 使用它，也可以通过 AI 编程助手的 Skills 使用，或作为托管创作工作流背后的渲染核心。

## 快速上手

### 使用 AI 编程助手

针对 Claude Code，安装带版本号的插件：

```bash
claude plugin marketplace add heygen-com/hyperframes
claude plugin install hyperframes@hyperframes
```

在 `/plugin` → **Marketplaces** 中为 **hyperframes** 市场开启自动更新，然后使用 `/hyperframes:hyperframes`。关于 Copilot、VS Code、Cursor、Gemini CLI、更新与迁移，请参阅[插件指南](docs/guides/plugins.mdx)。

独立 Skills（包括 OpenCode）请使用：

```bash
npx skills add heygen-com/hyperframes
```

> 选择器打开时默认什么都不选——你只需要 **核心 Skills** 一组：`/hyperframes` 路由器会按需安装每个创作工作流。智能体和非交互式运行请改用 `npx hyperframes skills update`——它只安装核心集合，而 `skills add --all` 会安装全部 21 个已发布 Skills。仓库内部的 6 个 Skills（位于 `.claude/skills` / `.agents/skills` 下）默认不包含。如需完整已发布集合，请使用 `npx hyperframes skills`。
>
> `skills add` 会解析 skills.sh 注册表 blob，可能比 `main` 分支落后数小时。`npx hyperframes skills update` 直接从当前 `main` 安装，因此需要最新版 Skill 时请用它。

试试这样的提示词：

> 使用 `/hyperframes`，创建一个 10 秒的产品介绍视频：带淡入标题、背景视频和轻柔的背景音乐。

这些 Skills 教会智能体 HyperFrames 的生产流程：规划视频、编写合法的 HTML、接入可定位动画、添加媒体、Lint 检查、预览与渲染。它们适用于 Claude Code、Codex、Cursor、Gemini CLI、IBM Bob 以及其他支持 Skills 的编程助手。

## Skills

HyperFrames 提供 21 个 Skills，供智能体按需加载。请先读 `/hyperframes`——它是路由器和能力地图；它会为任何“帮我做一个……”请求挑选工作流——视频、演示文稿或组合移植——并指向下面的领域 Skills。

独立安装默认使用**核心集合**——路由器会按需安装每个创作工作流。`npx hyperframes skills update` 可从任意位置精确安装核心集合；交互式选择器（`npx skills add heygen-com/hyperframes`）会将其列为“Core Skills”组，默认不预选。选择器仅支持交互式——非交互式或智能体运行若不带 `--skill` 会安装全部 21 个。如需有意安装完整已发布集合，请使用 `npx skills add heygen-com/hyperframes --all`（跳过选择器），或用 `npx skills add heygen-com/hyperframes --skill <name>` 只安装某一个（裸名称，不要带前导 `/`）。

此后独立安装会保持精简：`npx hyperframes init` 会保持**核心集合**最新（路由器、`hyperframes-*` 领域 Skills 和 `media-use`——再加上已安装的任何内容；`/figma` 保持按需加载），且不会悄悄扩展部分安装；创作工作流**按需**安装——路由器在进入某个工作流之前会先运行 `npx hyperframes skills update <workflow>`。不会在你背后重新拉取完整集合。

### 插件包

插件打包完整的 Skill 目录，并使用各自智能体的更新管理器。`bun run package:agent-plugin` 会构建已提交的可移植 ZIP、源码元数据和 SHA-256 校验和。详见[维护者清单](docs/contributing/agent-plugins.mdx)。

### 上传到 Codex

从已提交的 `HEAD` 版本的清单、品牌资源和 Skills 构建可直接上传的 Codex 插件归档：

```bash
bun run package:codex-plugin
```

这会生成 `dist/hyperframes-plugin.zip`（根目录为 `hyperframes/`），若归档超过 Codex 100 MB 上传上限则会失败。

### 路由器

| Skill          | 适用场景                                                                                                                                                                                                                                                                 |
| -------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `/hyperframes` | **请先读**：任何制作 / 创建 / 编辑 / 动画 / 渲染视频、动画或动态图形的请求。领域 Skills 的能力地图；在每个创作简报开始前确认意图的意图层；以及下面创作工作流的意图路由器。 |

### 创作工作流

| Skill                      | 适用场景                                                                                                                                                                                                                     |
| -------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `/product-launch-video`    | 任何**网站**——为某个产品做营销 / 发布 / 推广（来自其 URL、简报或脚本），或以该网站自身视觉为特色的站点巡览 / 展示 / 社交短片。最长约 3 分钟（最佳时长 30–90 秒）。              |
| `/faceless-explainer`      | **讲解某个主题 / 概念**，来自任意文本——没有产品、没有 URL、没有网站截图；所有视觉均由 LLM 生成（排版 / 抽象 / 图表 / 数据可视化）。                                                    |
| `/pr-to-video`             | 一个 **GitHub Pull Request**（PR URL、`owner/repo#N` 引用，或“这个 PR”）→ 更新日志 / 功能发布 / 修复 / 重构讲解，通过 `gh` CLI 读取。                                                                         |
| `/embedded-captions`       | 为现有口播视频添加**字幕 / 隐藏字幕**（原 footage 不动）——逐字轨道、主体背后的嵌入式高潮字幕，或纯电影感嵌入。                                                          |
| `/talking-head-recut`      | 为现有口播 / 访谈 / 播客视频打包**设计感图形叠加层**——下三分之一字幕条、数据标注、动态标题、引言、侧边面板、画中画。                                              |
| `/motion-graphics`         | 短小、**无旁白、设计主导的动态图形**（约 10 秒以内）——动态文字、数据 / 图表冲击、Logo 闪现、下三分之一、动态推文 / 标题。MP4 或透明叠加层。                                            |
| `/music-to-video`          | 一条**音乐轨道**（音频文件、从视频中提取的音频，或根据情绪简报生成的音乐）→ 一条**节拍同步**的视频——歌词、幻灯片或动感宣传片；音乐驱动节奏。                                            |
| `/slideshow`               | 一个**演示文稿 / 路演 Deck / 交互式 Deck**——离散幻灯片、片段揭示、分支、热点导航、演讲者模式。输出是可导航的 Deck，而非渲染视频。                                     |
| `/general-video`           | **其他一切**——更长或多场景作品、品牌 / 精华混剪、标题卡、静态循环、自由形式组合。与输入和时长无关的兜底方案，也是陪伴模式（与完整工具箱共同创作）的所在地。 |
| `/remotion-to-hyperframes` | **将现有 Remotion**（React）组合的源码**移植**到 HyperFrames HTML。单向迁移，不是创作。                                                                                                          |

### 领域 Skills（按需加载）

创作工作流组合调用的原子能力——需要某一层能力时拉取对应的 Skill。

| Skill                    | 覆盖内容                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| ------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `/hyperframes-core`      | 组合契约——`data-*` 计时属性、`class="clip"`、轨道、子组合、变量、框架自有媒体播放、确定性规则。                                                                                                                                                                                                                                                                                                                                                                                       |
| `/hyperframes-animation` | 全部动画知识——原子运动规则、场景蓝图、转场、运行时适配器（GSAP / Lottie / Three.js / Anime.js / CSS / WAAPI / TypeGPU）。                                                                                                                                                                                                                                                                                                                        |
| `/hyperframes-keyframes` | 跨运行时的可定位关键帧编写——GSAP 时间线、CSS 关键帧、Anime.js、WAAPI、FLIP、路径、遮罩、SVG 变形 / 描边、3D 纵深——另有 `hyperframes keyframes` 诊断工具，用于检查已渲染的运动。                                                                                                                                                                                                                                                                          |
| `/hyperframes-creative`  | 非动画创意指导——`frame.md` / `design.md`、配色、排版、旁白、节拍规划、音频响应式视觉、组合模式。                                                                                                                                                                                                                                                                                                                           |
| `/media-use`             | 媒体操作系统（宿主 App 自带的音乐和音效工具优先）——将任何媒体需求（BGM、SFX、图片、图标、Logo、语音、色彩分级、LUT）解析为已冻结的本地文件或可直接粘贴的代码块 + 台账记录；目录缺失时通过 TTS / 音乐 / 图像模型生成；转录、字幕、抠图，并在项目间复用资源。统一音频引擎 + 清单追踪。                                                                |
| `/hyperframes-cli`       | CLI 开发循环——`init`、`lint`、`check`、`snapshot`、`preview`、`render`、`publish`、`doctor`，另有 HeyGen 托管云渲染（`cloud render`）和 AWS Lambda 渲染（`lambda deploy / render / progress`）。                                                                                                                                                                                                                                                                    |
| `/hyperframes-audio`     | 混音已放入组合的音频——旁白避让（仅在人声占据的频段压低音乐床，支持静态或动态，含电平匹配）、效果链（EQ、压缩器、限制器、噪声门、饱和、延迟、混响、合唱、相位、比特破碎）、音量或任意效果参数的自动化包络，以及子混音总线（`<hf-audio-group>`），为多条轨道共用一条效果链、推子和自动化时钟。音频来源请用 `/media-use`。 |
| `/hyperframes-registry`  | 通过 `hyperframes catalog` / `hyperframes add` 搜索、安装并接入注册表 Block 与组件。在手动搭建任何具名外观、效果、处理或转场之前先加载它。编写新的 Block 或组件以贡献给上游。                                                                                                                                                                                                                       |
| `/figma`                 | 导入 Figma 资源、Token、组件和故事板章节 → 重建的动态效果（帧被当作状态读取，而非幻灯片）（REST/CLI），另有 Motion 动画（MCP）和着色器（MCP 源码 / 原生导出）导入组合。                                                                                                                                                                                                                                                     |

视觉设计交付工作流请参阅 [Claude Design 指南](https://hyperframes.heygen.com/guides/claude-design)和 [Open Design 指南](https://hyperframes.heygen.com/guides/open-design)。

### 手动使用 CLI

```bash
npx hyperframes init my-video
cd my-video
npx hyperframes preview      # 在浏览器中预览，支持热重载
npx hyperframes render       # 渲染为 MP4
```

**环境要求：** Node.js 22+、FFmpeg

## 你能做什么

缺灵感？浏览 [Showcase](https://hyperframes.heygen.com/showcase)，看看已完成的视频——可观看、可阅读、可运行、可改编。

- 产品发布视频与功能公告
- PR 讲解，带动画代码差异、旁白与字幕
- 数据可视化、图表竞速与地图动画
- 带动态字幕、叠加层与音乐的社交视频
- 文档转视频、PDF 转视频与站点巡览讲解
- 可用于自动化内容流水线的可复用动态图形

## Frame.md

**frame.md——你的设计系统，为视频而生。**

每个品牌都有 `design.md`。没有一个是为镜头写的。`frame.md` 是缺失的翻译层：它接收你的 Web 语境设计规范，并将其为画面反转——相同的 Token、相同的规则，但重写后 AI 智能体无需猜测尺寸、无需触碰 Web chrome 即可编排宣传视频。

输出是你的整个工具链都能读取的 `DESIGN.md` 超集。原子保持神圣。组合保持自由。数字来自脚本。

<table>
  <tr>
    <td width="50%" align="center">
      <a href="https://www.hyperframes.dev/design/biennale-yellow"><img src="https://static.heygen.ai/hyperframes-oss/docs/images/design-templates/biennale-yellow.png" alt="Biennale Yellow" width="100%"></a>
      <br><b><a href="https://www.hyperframes.dev/design/biennale-yellow">Biennale Yellow</a></b>
    </td>
    <td width="50%" align="center">
      <a href="https://www.hyperframes.dev/design/blockframe"><img src="https://static.heygen.ai/hyperframes-oss/docs/images/design-templates/blockframe.png" alt="BlockFrame" width="100%"></a>
      <br><b><a href="https://www.hyperframes.dev/design/blockframe">BlockFrame</a></b>
    </td>
  </tr>
  <tr>
    <td width="50%" align="center">
      <a href="https://www.hyperframes.dev/design/blue-professional"><img src="https://static.heygen.ai/hyperframes-oss/docs/images/design-templates/blue-professional.png" alt="Blue Professional" width="100%"></a>
      <br><b><a href="https://www.hyperframes.dev/design/blue-professional">Blue Professional</a></b>
    </td>
    <td width="50%" align="center">
      <a href="https://www.hyperframes.dev/design/bold-poster"><img src="https://static.heygen.ai/hyperframes-oss/docs/images/design-templates/bold-poster.png" alt="Bold Poster" width="100%"></a>
      <br><b><a href="https://www.hyperframes.dev/design/bold-poster">Bold Poster</a></b>
    </td>
  </tr>
  <tr>
    <td width="50%" align="center">
      <a href="https://www.hyperframes.dev/design/broadside"><img src="https://static.heygen.ai/hyperframes-oss/docs/images/design-templates/broadside.png" alt="Broadside" width="100%"></a>
      <br><b><a href="https://www.hyperframes.dev/design/broadside">Broadside</a></b>
    </td>
    <td width="50%" align="center">
      <a href="https://www.hyperframes.dev/design/capsule"><img src="https://static.heygen.ai/hyperframes-oss/docs/images/design-templates/capsule.png" alt="Capsule" width="100%"></a>
      <br><b><a href="https://www.hyperframes.dev/design/capsule">Capsule</a></b>
    </td>
  </tr>
  <tr>
    <td width="50%" align="center">
      <a href="https://www.hyperframes.dev/design/cartesian"><img src="https://static.heygen.ai/hyperframes-oss/docs/images/design-templates/cartesian.png" alt="Cartesian" width="100%"></a>
      <br><b><a href="https://www.hyperframes.dev/design/cartesian">Cartesian</a></b>
    </td>
    <td width="50%" align="center">
      <a href="https://www.hyperframes.dev/design/cobalt-grid"><img src="https://static.heygen.ai/hyperframes-oss/docs/images/design-templates/cobalt-grid.png" alt="Cobalt Grid" width="100%"></a>
      <br><b><a href="https://www.hyperframes.dev/design/cobalt-grid">Cobalt Grid</a></b>
    </td>
  </tr>
  <tr>
    <td width="50%" align="center">
      <a href="https://www.hyperframes.dev/design/coral"><img src="https://static.heygen.ai/hyperframes-oss/docs/images/design-templates/coral.png" alt="Coral" width="100%"></a>
      <br><b><a href="https://www.hyperframes.dev/design/coral">Coral</a></b>
    </td>
    <td width="50%" align="center">
      <a href="https://www.hyperframes.dev/design/creative-mode"><img src="https://static.heygen.ai/hyperframes-oss/docs/images/design-templates/creative-mode.png" alt="Creative Mode" width="100%"></a>
      <br><b><a href="https://www.hyperframes.dev/design/creative-mode">Creative Mode</a></b>
    </td>
  </tr>
</table>

在 [hyperframes.dev/design](https://www.hyperframes.dev/design) 浏览并改编全部模板。

## 工作原理

将视频定义为 HTML。添加 data 属性来控制时间与轨道。使用 GSAP、CSS、Lottie、Three.js、Anime.js、WAAPI 或你自己的帧适配器来实现可定位动画。

```html
<div id="stage" data-composition-id="launch" data-start="0" data-width="1920" data-height="1080">
  <video
    class="clip"
    data-start="0"
    data-duration="6"
    data-track-index="0"
    src="intro.mp4"
    muted
    playsinline
  ></video>

  <h1 id="title" class="clip" data-start="1" data-duration="4" data-track-index="1">Launch day</h1>

  <audio
    data-start="0"
    data-duration="6"
    data-track-index="2"
    data-volume="0.5"
    src="music.wav"
  ></audio>

  <script src="https://cdn.jsdelivr.net/npm/gsap@3/dist/gsap.min.js"></script>
  <script>
    const tl = gsap.timeline({ paused: true });
    tl.from("#title", { opacity: 0, y: 40, duration: 0.8 }, 1);
    window.__timelines = window.__timelines || {};
    window.__timelines.launch = tl;
  </script>
</div>
```

在浏览器中即时预览。在本地或 Docker 中渲染。渲染器在无头 Chrome 中逐帧定位，并用 FFmpeg 编码结果，因此相同输入产生相同视频。

## HyperFrames 技术栈

HyperFrames 是开源渲染引擎，外加围绕 HTML 原生视频创作不断增长的工具集。

| 组件                                           | 状态              | 作用                                                                                      |
| ----------------------------------------------- | ------------------- | ------------------------------------------------------------------------------------------------- |
| CLI                                             | 可用           | 脚手架、预览、Lint、检查与渲染本地视频项目                                 |
| Core / Engine / Producer                        | 可用           | 解析组合、驱动无头 Chrome、编码视频、混音                            |
| Catalog                                         | 可用           | 可复用 Block 与组件：转场、叠加层、字幕、图表、地图与效果     |
| Agent skills                                    | 可用           | 教会编程智能体通用 Web 文档遗漏的视频制作模式                      |
| Studio                                          | 可用，持续演进 | 用于预览与编辑组合的浏览器界面                                           |
| AWS Lambda rendering                            | 可用           | 部署分布式渲染栈，从笔记本或 CI 驱动渲染                        |
| [hyperframes.dev](https://www.hyperframes.dev/) | 可用           | 社区游乐场：预览、迭代、分享与渲染 HTML 原生视频项目 |
| [frame.md](https://www.hyperframes.dev/design)  | 可用           | 为镜头反转你的设计系统——智能体可据此编排视频的 DESIGN.md 超集   |

## 目录

安装开箱即用的 Block 与组件：

```bash
npx hyperframes add flash-through-white   # 着色器转场
npx hyperframes add instagram-follow      # 社交叠加层
npx hyperframes add data-chart            # 动画图表
```

在 [hyperframes.heygen.com/catalog](https://hyperframes.heygen.com/catalog/blocks/data-chart) 浏览目录。

## 为什么选择 HyperFrames？

- **HTML 原生：**组合就是带 data 属性的 HTML 文件。无需 React，无需专有时间线格式。
- **智能体友好：**智能体本来就会写 HTML，且 CLI 默认非交互式。
- **确定性：**相同输入，相同帧，相同输出。为 CI、回归测试与自动渲染而生。
- **无需构建步骤：**`index.html` 组合开箱即播，可直接在浏览器中预览。
- **基于适配器的动画：**引入 GSAP、CSS 动画、Lottie、Three.js、Anime.js、WAAPI 或自定义运行时。
- **开源：**Apache 2.0 许可证，无按次渲染费用，无商业使用门槛。

## HyperFrames 与 Remotion 对比

HyperFrames 受 [Remotion](https://www.remotion.dev) 启发。两者都用无头 Chrome 与 FFmpeg 渲染视频。主要区别在于创作模型：Remotion 押注 React 组件；HyperFrames 押注人类与智能体都能轻松编写的纯 HTML。

|                          | **HyperFrames**                       | **Remotion**                            |
| ------------------------ | ------------------------------------- | --------------------------------------- |
| 创作方式                | HTML + CSS + 可定位动画       | React 组件                        |
| 构建步骤                | 无；`index.html` 开箱即播        | 需要打包器                        |
| 智能体交接            | 纯 HTML 文件                      | JSX / React 项目                     |
| 库时钟动画 | 可定位、帧精确（通过适配器） | 墙钟动画模式需小心处理 |
| 分布式渲染    | 本地与 AWS Lambda 渲染路径     | Remotion Lambda，成熟的云渲染器  |
| 许可证                  | Apache 2.0                            | 源码可用的 Remotion License       |

完整对比请参阅 [HyperFrames 与 Remotion 对比指南](https://hyperframes.heygen.com/guides/hyperframes-vs-remotion)。

## 文档

完整文档：[hyperframes.heygen.com/introduction](https://hyperframes.heygen.com/introduction)

- [快速上手](https://hyperframes.heygen.com/quickstart)
- [Showcase](https://hyperframes.heygen.com/showcase)
- [指南](https://hyperframes.heygen.com/guides/gsap-animation)
- [API 参考](https://hyperframes.heygen.com/packages/core)
- [目录](https://hyperframes.heygen.com/catalog/blocks/data-chart)
- [示例](https://hyperframes.heygen.com/examples)
- [AWS Lambda 渲染](https://hyperframes.heygen.com/deploy/aws-lambda)

## 软件包

| 软件包                                                          | 说明                                                       |
| ---------------------------------------------------------------- | ----------------------------------------------------------------- |
| [`hyperframes`](packages/cli)                                    | 用于创建、预览、Lint 检查与渲染组合的 CLI |
| [`@hyperframes/core`](packages/core)                             | 类型、解析器、生成器、Linter、运行时与帧适配器   |
| [`@hyperframes/engine`](packages/engine)                         | 使用 Puppeteer 与 FFmpeg 的可定位页面转视频捕获引擎  |
| [`@hyperframes/producer`](packages/producer)                     | 完整的渲染流水线：捕获、编码与音频混音        |
| [`@hyperframes/studio`](packages/studio)                         | 基于浏览器的组合编辑器 UI                               |
| [`@hyperframes/player`](packages/player)                         | 可嵌入的 `<hyperframes-player>` Web Component                   |
| [`@hyperframes/shader-transitions`](packages/shader-transitions) | 用于组合的 WebGL 着色器转场                         |
| [`@hyperframes/aws-lambda`](packages/aws-lambda)                 | 用于分布式渲染的 AWS Lambda SDK 与部署界面     |

## 社区

HyperFrames 已在 [HeyGen](https://www.heygen.com) 投入生产使用，社区示例来自 [tldraw](https://tldraw.com)、[TanStack](https://tanstack.com) 等团队，详见 [ADOPTERS.md](ADOPTERS.md)。如果你的团队也在使用 HyperFrames，欢迎提 PR。

- 问题与想法：[Discord](https://discord.gg/EbK98HBPdk)
- Bug 与功能请求：[GitHub Issues](https://github.com/heygen-com/hyperframes/issues)
- 用户调研：[与 HyperFrames 团队预约一次轻松的 30 分钟交流](https://calendar.google.com/calendar/u/0/appointments/schedules/AcZssZ2cSpKoDgmcmRrgekrnrgqmvPT8W6F2Zg6e7MY7IJqaZKwpn_I0NdTHkN390iguMepE_NVg8ezb?gv=true)——无需准备，没有销售话术
- 安全报告：[SECURITY.md](SECURITY.md)
- 贡献：[CONTRIBUTING.md](CONTRIBUTING.md)

## 开发说明

本仓库对 `packages/producer/tests/**/output.mp4` 下的黄金回归测试基线使用 [Git LFS](https://git-lfs.com)（约 240 MB 的 `.mp4` 文件）。如果你要克隆完整仓库进行开发，请先安装 Git LFS：

```bash
# macOS
brew install git-lfs

# Ubuntu / Debian
sudo apt install git-lfs

# Windows
winget install GitHub.GitLFS

# 然后，每台机器执行一次
git lfs install
```

如果只需要源码，可以跳过 LFS 内容：

```bash
GIT_LFS_SKIP_SMUDGE=1 git clone https://github.com/heygen-com/hyperframes.git
```

## 许可证

[Apache 2.0](LICENSE)
