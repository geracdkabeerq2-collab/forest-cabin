# 森林小屋 · GitHub 开源项目调研与 MVP 方案

> 检索与数据核验时间：2026-11-05（数据来自 GitHub REST API 实时查询，非记忆）  
> 基线：现有自研项目 `index.html` 77.6KB（单文件 ES module）+ `vendor/three` 1.4MB（three r160 本地化，13 文件），零构建、零外部依赖。



---

## 一、候选项目清单

| #  | 项目                     | 链接                                                 | 定位                          | Stars   | License                                 | 技术栈                                       |
| -- | ---------------------- | -------------------------------------------------- | --------------------------- | ------- | --------------------------------------- | ----------------------------------------- |
| 1  | Elemental-Serenity     | <https://github.com/SahilK-027/Elemental-Serenity> | 四季 + 天气 + 昼夜的 3D 露营 diorama | 267     | **MIT**                                 | Vite 6 + three 0.182 + GSAP + GLSL + Sass |
| 2  | Mistwood Cottage       | <https://github.com/amiradeu/mistwood-cottage>     | 可进入室内的小屋世界（第三人称 + 物理）       | 25      | README 标 **CC BY 4.0**，API 无 license 字段 | three + Rapier3D + Vite + GSAP            |
| 3  | Bamboo Old House（四时竹声） | <https://github.com/yuukiLike/bamboo-old-house>    | 竹林老屋漫游，昼夜 + 天气 + 自然音景       | 37      | **无 LICENSE 文件**                        | React + TypeScript + three + Vite         |
| 4  | Cozy Cabin             | <https://github.com/fremorie/cozy-cabin>           | 雪中小屋场景（PBR 贴图）              | 0       | **MIT**                                 | Vite + three + Poly Haven 贴图              |
| 5  | Forest Game Scene      | <https://github.com/DominicFox/Forest-Game-Scene>  | 第一人称森林 + 小屋（含碰撞/手电/门交互）     | 0       | 无                                       | 原生 three + GLTF                           |
| 6  | forest_lsystem         | <https://github.com/muraliavarma/forest_lsystem>   | L-system 程序化森林              | 6       | **MIT**                                 | 原生 three（引擎版本很老）                          |
| 7  | Interior ThreeJS       | <https://github.com/priyalkhatri/Interior-ThreeJS> | 室内点击式漫游                     | 0       | 无                                       | Vite + three + Rapier                     |
| 8  | cabin-in-the-woods     | <https://github.com/ahujasid/cabin-in-the-woods>   | Blender → Three.js 导出流程示例   | 6       | 无                                       | Blender MCP（仅 12KB，非成品）                   |
| 底座 | three.js               | <https://github.com/mrdoob/three.js>               | 渲染引擎                        | 117,625 | MIT                                     | —                                         |
| 底座 | react-three-fiber      | <https://github.com/pmndrs/react-three-fiber>      | React 版 three 绑定            | 33,511  | MIT                                     | —                                         |

---

## 二、维护状态与活跃度

| 项目                 | 最近推送       | 创建         | 开放 Issue | 判断                              |
| ------------------ | ---------- | ---------- | -------- | ------------------------------- |
| Elemental-Serenity | 2026-06-17 | 2025-12-28 | 0        | **活跃**，半年内 55 commits，近 5 个月无更新 |
| Bamboo Old House   | 2026-09-30 | 2026-09-10 | 0        | **新项目**，1 个月前仍有提交，但无 license    |
| Mistwood Cottage   | 2025-08-08 | 2024-05-21 | 0        | **已停更 15 个月**                   |
| Cozy Cabin         | 2026-05-06 | 2026-04-07 | 0        | 个人作品，1 个月内活跃，0 star             |
| forest_lsystem     | 2025-04-21 | 2013-04-05 | 0        | 核心代码 2013 年，只更新过 README         |
| Forest Game Scene  | 2023-07-11 | 2023-07-11 | 0        | **停更 3 年**（课程作业）                |
| Interior ThreeJS   | 2025-11-15 | 2025-11-15 | 0        | 创建后无更新，仅 4 commits              |

### 已知问题（来自各自 README / Roadmap）

- **Mistwood Cottage**（问题最多，全部未修）：
  - High：物理跨浏览器不一致，角色移动/跳跃不平滑
  - High：浏览器空闲返回后贴图循环会突变
  - High：切换左侧屋顶会让前脸透明度跳变
  - High：角色模型粗糙；天空/雾色随循环变化不自然
  - Medium：贴图未做懒加载；相机缩放交互待改滚轮；墙体可切换缺少视觉提示
- **Elemental-Serenity**：无公开 issue；已知约束是重度依赖 GLB 模型与 Draco 解码器，资产替换成本高
- **Bamboo Old House**：无 license（默认版权保留，**不可直接复制代码或资产**）
- **Cozy Cabin / Forest Game Scene**：无 issue 跟踪，个人作品，无维护承诺

---

## 三、部署难度评估

| 项目                 | 构建工具               | 环境要求                            | 仓库体积       | 运行方式                                     | 主要难点                                       |
| ------------------ | ------------------ | ------------------------------- | ---------- | ---------------------------------------- | ------------------------------------------ |
| Elemental-Serenity | Vite 6             | Node 18+                        | 9.3 MB     | `npm i && npm run dev/build`，产 dist 静态托管 | Draco 解码器路径、GLB/环境贴图/音频四类资产同时加载            |
| Mistwood Cottage   | Vite               | Node + Rapier wasm              | **307 MB** | `npm i && npm run build`，Vercel          | 仓库过大（含 Blender 源文件），物理引擎 wasm 首次加载         |
| Bamboo Old House   | Vinext/Vite + pnpm | **Node ≥ 22.13.0、pnpm 11.14.0** | 62.6 MB    | `pnpm i && pnpm dev`                     | Node/pnpm 版本门槛高；React 组件化结构，改造成本高于原生 three |
| Cozy Cabin         | Vite               | Node（有 .nvmrc）                  | 38.7 MB    | `npm run dev`（8080）/`build`              | PBR 贴图体积大，首屏需优化                            |
| Forest Game Scene  | 无                  | 无                               | 4 MB       | `python3 -m http.server` 直跑              | 无构建负担，但 three 版本老、文件结构扁平                   |
| forest_lsystem     | 无                  | 无                               | 1.1 MB     | 任意静态服务器                                  | three 版本极老（2013），API 与 r160 不兼容            |
| **现有自研**           | **无**              | **无（浏览器即可）**                    | **1.5 MB** | 任意静态服务器                                  | **无**（唯一要求：必须走 http，不能 file://）            |

---

## 四、功能覆盖与复用判断

### 4.1 覆盖矩阵（✅ 具备 / ⚠️ 部分 / ❌ 缺失）

| 功能          | 现有自研            | Elemental-Serenity | Mistwood       | Bamboo | Cozy Cabin | Forest Game |
| ----------- | --------------- | ------------------ | -------------- | ------ | ---------- | ----------- |
| 小屋建筑        | ✅ 单坡屋顶 + 玻璃幕墙   | ❌ 仅帐篷              | ✅ 精细           | ✅ 老屋   | ✅ 雪屋       | ✅           |
| 森林/地形       | ✅ 程序化地形 + 树     | ✅                  | ✅ 山丘池塘         | ✅ 竹林   | ✅          | ✅ 随机树       |
| 四季切换        | ✅ 4 季换色 + 粒子    | ✅ 4 季 + 平滑过渡       | ❌ 仅昼夜          | ❌      | ❌          | ❌           |
| 天气系统        | ❌ 仅有雪/落叶粒子      | ✅ 雨/雪/风/雷暴         | ❌              | ✅ 雨/风  | ❌          | ❌           |
| 节日主题（万圣/圣诞） | ✅               | ❌                  | ❌              | ❌      | ❌          | ❌           |
| 音乐/音景       | ✅ 程序化 Web Audio | ✅ 音频文件             | ✅ 环境音 + 脚步     | ✅ 自然音景 | ❌          | ❌           |
| 字幕叙事        | ✅               | ❌                  | ❌              | ❌      | ❌          | ❌           |
| 相机巡航        | ✅               | ❌                  | ⚠️ 第三人称跟随      | ⚠️     | ❌          | ✅ 第一人称      |
| 亮度调节（能量条）   | ✅               | ❌                  | ❌              | ❌      | ❌          | ❌           |
| 截图导出        | ✅               | ❌                  | ❌ 仅 roadmap 提及 | ❌      | ❌          | ❌           |
| 移动端适配       | ⚠️ 基础响应式        | ✅ 触感反馈             | ⚠️             | ✅      | ❌          | ❌           |
| 室内可进入       | ❌ 贴图假室内         | ❌                  | ✅              | ✅      | ❌          | ✅           |

**结论：现有自研在「叙事型凝视场景」这个定位上覆盖度最高（11 项中 8 项完整具备），唯一天然短板是天气系统与可进入室内。**

### 4.2 可直接复用 vs 需改造（模块级评估）

| 模块               | 来源                         | 授权        | 复用方式                                       | 工作量   | 风险             | 适配点                                 |
| ---------------- | -------------------------- | --------- | ------------------------------------------ | ----- | -------------- | ----------------------------------- |
| 天气状态机（雨/雪/风/雷切换） | Elemental-Serenity         | MIT       | 参考其 season/weather manager 结构，自写 200-300 行 | 4-6h  | 粒子性能           | 需对接现有 `setParticles` 季节粒子层，避免两套粒子并存 |
| 雨/雪 shader 技巧    | Elemental-Serenity         | MIT       | 可复制 GLSL chunk 思路                          | 2-3h  | 与现有雾/体积光冲突     | 现有用 Sprite 粒子，改 shader 需重写粒子管       |
| PBR 贴图素材清单       | Cozy Cabin（Poly Haven CC0） | 贴图 CC0    | **直接下载使用**，无需署名                            | 1-2h  | 体积增大           | 需压缩为 webp/jpg，控制首屏                  |
| 程序化树 L-system    | forest_lsystem             | MIT       | 可复制算法                                      | 3-4h  | three 版本不兼容    | 需重写为 InstancedMesh，替换现有低模树          |
| 第三人称 + 室内漫游      | Mistwood / Bamboo          | CC BY / 无 | **仅参考思路，不复用代码**                            | 3-5 天 | 物理引擎引入、场景要重做尺度 | 与"凝视型"定位冲突，不建议做                     |
| 墙体切换偷看室内         | Mistwood                   | CC BY     | 参考思路                                       | 1 天   | 需真实建模室内        | 现有室内是贴图，无法直接套用                      |
| 自然音景调度           | Bamboo                     | 无         | 仅参考架构（音效需自采或另找 CC0）                        | 1-2 天 | 授权风险           | 现有已有程序化音乐，价值重叠                      |

---

## 五、明确建议

### 建议：**保留现有自研项目为主干，按模块吸收 MIT 项目的具体技术；不直接使用、也不整体 fork 任何候选。**

四条依据：

1. **定位不匹配**  
   8 个候选里没有一个是「森林小屋 + 四季 + 节日主题 + 字幕叙事 + 亮度调节」的叙事型凝视场景。
   - Elemental-Serenity 是露营点（帐篷 + 篝火 + 池塘），**没有建筑**
   - Mistwood / Bamboo 是"走进去"的漫游世界，交互节奏与"静观"完全不同
   - Cozy Cabin 是纯静态雪景，无 UI 无交互层
2. **授权风险显著**  
   只有 3 个有明确 MIT（Elemental-Serenity / Cozy Cabin / forest_lsystem）。
   - 气质最贴近的 **Bamboo Old House 没有 LICENSE** → 默认版权保留，复制代码或资产即侵权
   - Mistwood 用的是 **CC BY 4.0**（内容许可，用于代码存在争议，商用需署名）
3. **现有项目完成度反而更高**  
   77.6KB 单文件、零构建、零外部依赖、14 项功能、浏览器实测零报错、3 秒加载。  
   换基座 = 丢掉已验证成果，再花 3-5 天对齐资产与 shader。
4. **维护风险**  
   除 Elemental-Serenity 外，其余项目 0-25 star 的个人作品，停更即无人修。Mistwood 的 Roadmap 里 High 级 bug 已挂 15 个月未修。

### 唯一例外分支

如果目标改为「可进入室内、第一人称/第三人称漫游」，则应 fork **Mistwood Cottage**（功能最全）或 **Bamboo Old House**（气质最贴），但必须先解决 license（联系作者/改用 CC BY 署名）与 60-300MB 资产瘦身。这条路的 MVP 成本约 3-5 天，是当前定位的 3-5 倍。

---

## 六、最快 MVP 方案

### 6.1 技术选型

| 项  | 选择                                            | 理由                               |
| -- | --------------------------------------------- | -------------------------------- |
| 渲染 | three r160（已本地化）                              | 现有环境已验证，不升级                      |
| 架构 | 单文件 ES module（保持现状）                           | 77KB 手写可控；引入 Vite 会让"改完即看"变成构建流程 |
| 音频 | Web Audio 程序化合成（保持现状）                         | 零资产、零授权风险                        |
| 素材 | Poly Haven CC0 贴图（按需）                         | 唯一可无条件商用的高质量来源                   |
| 部署 | 静态托管（WorkBuddy sites / GitHub Pages / Vercel） | 无后端、无构建产物                        |

### 6.2 范围取舍

**做（3 项，全部是增量）**

1. 天气系统：雨 / 雪 / 雷暴，与季节联动（雪→冬、雨→春/夏、雷暴→夏夜）
2. 移动端打磨：单指旋转 / 双指缩放、像素比自适应降级、按钮换行布局
3. 在线部署：出可分享链接

**不做（明确排除，避免范围膨胀）**

- 第三人称角色与物理碰撞（与凝视定位冲突，成本最高）
- 真实室内建模（现有是贴图假室内，改真实建模需重建整栋房屋）
- 多语言（单语中文场景，无收益）

### 6.3 实施步骤与工时

| 步骤 | 内容                                                                     | 预估     | 风险与对策                                        |
| -- | ---------------------------------------------------------------------- | ------ | -------------------------------------------- |
| 1  | 天气层：雨/雪粒子（复用 Points + 自定义 shader）、雷暴闪光（后处理脉冲）、雨声/雷声 Web Audio 合成、UI 开关 | 4-6h   | 粒子性能 → 单一 BufferGeometry 复用，上限 4000 粒子并做设备分级 |
| 2  | 移动端：Pointer Events 手势、`devicePixelRatio` 上限 clamp、按钮布局重排               | 2-3h   | iOS Safari 音频必须用户手势触发 → 复用现有"音乐"按钮事件         |
| 3  | 回归验证：四季 × 3 主题 × 亮度 5 档组合冒烟，零报错 + 帧率抽检                                 | 1-1.5h | 组合爆炸 → 只测对角组合 + 全量单变量                        |
| 4  | 部署上线：静态托管 + 分享链接 + 首屏体积核对                                              | 0.5h   | 无                                            |

**总工时：7.5-11 小时（约 1.5 个工作日）**

### 6.4 交付形态

- 产物：仍是单文件 `index.html` + `vendor/`，目录可直接压缩分发
- 首屏：1.5MB（three 1.27MB + 场景代码 77KB），gzip 后约 400KB
- 部署：任意静态服务器；本地预览 `python -m http.server 8931 --directory <项目目录>`

### 6.5 若坚持"换基座"的对照方案

fork Elemental-Serenity → 用自建小屋替换帐篷/篝火 → 补 UI 与控制 → 需 3-5 天，且要处理 GLB 资产、Draco、环境贴图三套加载链路，并自证 MIT 合规（保留版权声明）。

---

## 附：数据可信度说明

- 所有 star / fork / issue / 推送时间来自 `api.github.com/repos/{owner}/{repo}` 于 2026-11-05 的实时返回
- License 判断依据：API `license.spdx_id` 字段 + 仓库根目录 LICENSE 文件存在性；Mistwood 的 CC BY 4.0 摘自其 README 的 License 段落
- 功能清单摘自各项目 README 与 Roadmap 原文，未做实际部署验证；「现有自研」一列为我方实测结论
