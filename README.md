# 森林小屋 · Forest Cabin

[![License: MIT](https://img.shields.io/badge/License-MIT-2ea44f.svg)](./LICENSE)
[![three.js](https://img.shields.io/badge/three.js-r160-000000.svg)](https://threejs.org/)

> 一个跑在浏览器里的 3D 森林小屋。四季轮转、天气变化、节日主题、程序化配乐与字幕叙事。
> 纯静态、零构建、零外部依赖（three.js 已本地化），克隆下来起个静态服务器就能离线运行。

**在线体验**：https://geracdkabeerq2-collab.github.io/forest-cabin/

---

## 特性

| 模块 | 说明 |
|---|---|
| **四季轮转** | 春樱 / 夏萤 / 秋叶 / 冬雪。地形材质、植被配色、光照曲线、天空渐变、飘落粒子、字幕文案整套联动 |
| **天气系统** | 晴 / 雨 / 雷雨 / 雪 / 雾 五种。雨丝与雪花共用一套粒子池，按天气切换形态；雾浓度、天空灰化程度、太阳强度按系数叠加；雷雨带双闪闪电与延迟滚雷 |
| **节日主题** | 万圣节（南瓜灯、蝙蝠、墓碑、诡异和弦）/ 圣诞（彩灯串、雪松、屋顶积雪、铃声音色） |
| **亮度能量条** | 左侧竖向 10 格，自下而上点亮。联动色调映射曝光、环境光、辉光强度；支持点击 / 拖动 / 滚轮 / `[` `]` 键；设置持久化到 localStorage |
| **烛光照明** | 屋内一律蜡烛与 LED 灯珠：烛台带火焰 sprite 与微弱点光源，双正弦驱动摇曳；窗内景为暗室剪影，不做大面积暖白 |
| **程序化配乐** | Web Audio 合成，无任何音频文件。和弦垫 + 拨弦 + 铃 + 虫鸣，主题切换换和弦色彩；环境音层叠加雨声、风声、雷声，与天气联动 |
| **字幕叙事** | 四季各 4 句文案，7 秒轮播，带进度圆点可点击跳转 |
| **镜头语言** | 开场 5 秒推进、空闲自动巡航、自由环视（OrbitControls）、电影画幅遮幅 |
| **状态分享** | URL 携带当前状态 `?s=autumn&t=none&w=rain&b=6`，一键复制链接即可分享同一画面 |
| **截图** | 保存当前画面为 JPG |
| **移动端** | 按 CPU 核心数与指针类型分档：像素比、阴影分辨率、粒子上限自动降级；单指旋转 / 双指缩放 |

---

## 快速开始

ES module + importmap 需要 http 协议，不能直接双击打开：

```bash
# 任意静态服务器均可
python -m http.server 8931
# 打开 http://127.0.0.1:8931/
```

无 npm、无构建步骤、无 CDN 依赖，断网也能跑。

---

## 快捷键

| 键 | 作用 |
|---|---|
| 拖动 / 单指 | 环视 |
| 滚轮 / 双指 | 缩放 |
| `[` `]` | 亮度 ±10% |
| `1` `2` `3` `4` | 春 / 夏 / 秋 / 冬 |
| `W` | 切换天气 |
| `T` | 切换节日主题 |
| `C` | 自动巡航开关 |
| `F` | 电影画幅 |
| `M` | 音乐开关 |
| `P` | 截图 |
| `?` | 操作说明 |

---

## 目录结构

```
.
├── index.html              # 全部实现:HTML + CSS + ES module(约 94 KB)
├── vendor/three/           # three.js r160 本地依赖(13 个文件,1.4 MB)
│   ├── build/three.module.js
│   └── examples/jsm/       # OrbitControls / EffectComposer / UnrealBloom / ...
├── docs/
│   └── 开源调研与MVP方案.md  # 同类开源项目调研与取舍依据
├── LICENSE
└── README.md
```

---

## 实现要点

- **单文件架构**：所有场景代码内联在一个 `<script type="module">` 里，改完刷新即可见，没有构建等待。
- **地形**：值噪声 + fbm 生成高度场，按季节重刷顶点色；树木用 InstancedMesh，风摆通过 `onBeforeCompile` 注入顶点动画。
- **后期**：EffectComposer → UnrealBloom → OutputPass → 自写 GLSL 调色 pass（饱和度、双色调、暗角、胶片噪点，带时间扰动）。
- **天气分层**：季节/主题提供基础光照参数，天气以乘数叠加（雾密度、天空双色、太阳强度、薄雾浓度），三者正交组合互不冲突。
- **深度冲突治理**：玻璃、贴片这类薄面片必须与承载盒体表面留出间隙并加 `polygonOffset`，否则会与窗框共面产生逐帧闪烁。
- **音频**：所有音源接入统一 bus，带卷积混响（程序生成 IR）；环境音与音乐分轨，共用 master 与音乐开关。

---

## 浏览器兼容

Chrome / Edge / Firefox / Safari 新版均可（需 WebGL2）。移动端已做性能降档，建议 iOS Safari 15+ / Android Chrome 100+。

---

## 许可

本项目采用 **MIT License**，可自由使用、修改、分发与商用，请保留版权与许可声明。

Copyright (c) 2026 Tanay Gera · 详见 [LICENSE](./LICENSE)

---

## 参考与致谢

- [**Elemental-Serenity**](https://github.com/SahilK-027/Elemental-Serenity)（MIT, © SahilK-027）— 本项目的季节 / 天气分层结构、粒子与音频分层思路参考该项目。**未直接复制其源码**；若后续引入其代码片段，将保留原 MIT 版权声明。
- [**three.js**](https://github.com/mrdoob/three.js)（MIT, © three.js authors）— 渲染引擎，已本地化到 `vendor/`。
- [**Poly Haven**](https://polyhaven.com/)（CC0）— 若替换贴图，素材来源建议。

> 第三方授权说明：`vendor/three` 保持上游 MIT 许可与版权头，二次分发请一并保留。
