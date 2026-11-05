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
| **签到添柴** | 每天首次打开自动签到，连续天数决定火势（1.0~1.4 倍），烛火与篝火随之变旺；断签回落。只影响视觉，不锁任何功能 |
| **成就墙** | 9 个成就：初次点灯 / 四海为家 / 六节气 / 听风者 / 雷雨夜 / 七日之约 / 心愿升空 / 拾荒者 / 专注一小时。解锁实时提示 |
| **季节彩蛋** | 树林四角各藏一颗发光晶石（春笋/萤火瓶/松果/冰晶），集齐 4 个后门廊出现旋转走马灯 |
| **许愿孔明灯** | 写一句心愿 → 灯笼从门前升起、摇摆、渐隐；本地保留最近 3 条 |
| **时间胶囊** | 埋一句话 + 选 7/30/100 天，树下土堆木牌；到期后木牌脉冲发光可开启 |
| **GitHub 星标** | 左下角实时显示仓库收藏数（缓存 1 小时，断网静默降级） |
| **沉浸 / 壁纸模式** | `I` 键或「更多 → 沉浸」隐藏全部界面，相机沿极坐标缓慢环绕，适合当桌面背景长期挂着 |
| **场景预设卡片** | 8 组精选组合（秋日黄昏 / 雪夜守岁 / 仲夏萤火 / 清明细雨 / 端午出游 / 七夕星河 / 中秋赏月 / 圣诞雪夜），一键切换季节 × 主题 × 天气 × 居所 × 亮度 |
| **可交互物件** | 点击烛台点亮/熄灭、点击篝火三档火势、点击灯笼换色；带 hover 指针反馈 |
| **音景混音器** | 雨 / 风 / 篝火 / 虫鸣 / 落雪 五轨独立音量，全部 Web Audio 实时合成（零音频文件）；随天气、居所、季节自动预设，手动拖过即锁定 |
| **照片模式** | 4 种滤镜（原色 / 暖调 / 黑白 / 胶片）+ 2× 超采样导出 + 白边与下方签名，PNG 下载 |
| **专注番茄钟** | 25 分钟倒计时，`performance.now()` 锚定零漂移；结束播 C5-E5-G5 琶音铃 |
| **可选居所** | 小屋 / 帐篷 / 房车 / 树屋 四种可切换（`H` 键或「居所」按钮）。每种独立建模：帐篷含 A 形帆布、门帘与门前篝火；房车含厢体、驾驶室、遮阳篷与四轮；树屋含树干、平台、栏杆与绳梯。切换时相机自动重新构图、炊烟锚点跟随 |
| **四季轮转** | 春樱 / 夏萤 / 秋叶 / 冬雪。地形材质、植被配色、光照曲线、天空渐变、飘落粒子、字幕文案整套联动 |
| **天气系统** | 晴 / 雨 / 雷雨 / 雪 / 雾 五种。雨丝与雪花共用一套粒子池，按天气切换形态；雾浓度、天空灰化程度、太阳强度按系数叠加；雷雨带双闪闪电与延迟滚雷 |
| **节日主题** | **中国传统节日 6 个**：春节（灯笼杆、鞭炮、年桔、暗红夜空、自动下雪）/ 元宵（漂浮花灯、灯谜、汤圆）/ 清明（柳枝、青团、纸鸢、自动细雨）/ 端午（粽子、艾草束、香囊）/ 七夕（鹊桥、粉灯、星河）/ 中秋（圆月、月饼、桂花）。**西洋节日 2 个**：万圣节（南瓜灯、蝙蝠、墓碑）/ 圣诞（彩灯串、雪松、屋顶积雪）。每个节日自带光照、天空、粒子、窗光与 4 句字幕 |
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
| `H` | 切换居所（小屋 / 帐篷 / 房车 / 树屋） |
| `T` | 切换节日主题 |
| `C` | 自动巡航开关 |
| `I` | 沉浸 / 壁纸模式 |
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

## 数据结构

四个维度**正交组合**（互不覆盖），全部状态可写进 URL：

```js
// 全局状态
state = { season: 'autumn', theme: 'none' }
curWeather   = 'clear'    // clear | rain | storm | snow | mist
curDwelling  = 'cabin'    // cabin | tent | rv | treehouse
brightLevel  = 6          // 1..10

// 居所表：每种居所声明自己的组、炊烟锚点、相机目标点与机位
DWELLINGS = {
  cabin:     { label:'小屋', group:cabin,     smoke:Vector3, target:Vector3, cam:Vector3 },
  tent:      { label:'帐篷', group:tentGroup, ... },
  rv:        { label:'房车', group:rvGroup,   ... },
  treehouse: { label:'树屋', group:treeGroup, ... },
}

// 节日主题表：light 会整体替换季节光照，fest 指向道具组，weather 为推荐天气
THEMES = {
  none, springFest, lanternFest, qingming, dragonBoat, qixi, midAutumn, halloween, christmas
}
// 每个主题字段：label / light / particles / window / captions / (fest) / (weather) / (snow)

// 节日道具组（与居所解耦，布置在居所外围）
FESTIVALS = { springFest, lanternFest, qingming, dragonBoat, qixi, midAutumn }
```

URL 参数：`?s=季节&t=主题&w=天气&d=居所&b=亮度`

### v2.0 新增数据结构

```js
// 场景预设（纯数据表，幂等切换）
PRESETS = [{ id, name, s, t, w, d, b, c1, c2 }, ...]

// 音景轨道（每轨独立 GainNode，接 ambBus）
AMB_TRACKS = [{ id:'rain'|'wind'|'fire'|'cricket'|'snow', label }, ...]
audio.setVolume(id, v)      // 用户手动调，标记 manual 后不再被场景覆盖
audio.setAmbience(w, d, sn) // 按天气/居所/季节自动预设音景

// 可交互物件注册表
interactables = [{ obj, label, onClick }]
makeInteractive(obj, label, onClick)   // 拾取时会过滤掉不可见对象

// 专注番茄钟（deadline 锚定，零漂移）
FOCUS = { total, deadline, running, timer }

// v3 社交化进度：全部走 localStorage（前缀 cabin_），无后端
cabin_streak  = { last: 20261106, days: 7 }        // 签到（日期键为 YYYYMMDD 整数）
cabin_ach     = { firstLight: 1699... }            // 已解锁成就
cabin_seen    = { dwelling:{}, theme:{}, track:{} } // 探索记录（驱动成就）
cabin_eggs    = { spring: 1, summer: 1 }           // 彩蛋收集
cabin_wishes  = [{ text: '愿家人平安', at: 1699... }]
cabin_capsule = { text: '...', at: ..., unlockAt: ... }
cabin_stars   = { n: 1, t: 1699... }               // GitHub 星标缓存（TTL 1h）
```

---

## 居所 × 节日 推荐组合

居所与节日**不做硬绑定**，4 × 9 = 36 种组合全部可用；以下仅是气质上更贴合的建议：

| 节日 | 推荐居所 | 理由 |
|---|---|---|
| 春节 | 小屋 | 门廊贴春联、檐下挂灯笼，最有"回家过年"的感觉 |
| 元宵 | 小屋 / 帐篷 | 花灯环绕帐前空地，野外赏灯 |
| 清明 | 帐篷 | 踏青露营，细雨柳枝最贴 |
| 端午 | 房车 | 车边遮阳篷下摆粽子，像自驾出游 |
| 七夕 | 帐篷 | 帐前篝火 + 鹊桥星河，露营观星 |
| 中秋 | 树屋 | 抬高视角，月亮与平台齐平，赏月最佳 |
| 万圣节 | 小屋 | 南瓜灯与墓碑围着小屋 |
| 圣诞 | 小屋 / 树屋 | 屋顶积雪与彩灯串效果最明显 |

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
