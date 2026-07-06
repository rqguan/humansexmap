# Changelog

本文件仅记录 **`updatenymxx`** 分支上的变更（互动 Web 版 v6 起）。不含该分支建立前仓库早期的静态文件与删改历史。

格式参考 [Keep a Changelog](https://keepachangelog.com/zh-CN/1.1.0/)。

---

## [Unreleased]

### Performance

- **子区词条懒挂载**：`zoom < 130%`（`Z_ENT`）时不创建子区 label DOM，缩回时销毁；启动时仅保留约 87 个大区 label，减轻拖拽平移时的合成负担。
- 抽取 `mkLabelEl`、`mountEntryLabels`、`unmountEntryLabels`；`lblEls[i]` 在未挂载时为 `null`。
- `toggleLang` / `resetStates` 跳过未挂载的 entry 节点。

### Fixed

- **Label counter-scale**：抽出 `syncLabelScale()`，每帧与 `#world` transform 同步更新 `--inv`（滚轮 / 双指 / 按钮 / 键盘均经 `doZoom` → `applyT`）。
- `resetView()` 重置 `lastZoom`，确保复位时 LOD 与 entry 挂载状态刷新。
- `resize` 时若 zoom 低于动态 `minZoom()` 则抬升 zoom，并重新 `clampPan`。
- **子区坐标**：`fixEntryCoords` 对 `0.x` 格式用 `FX2` 还原到所属大区附近（勿 ×100，否则 432 条子区会偏到地图角落）；`#labels` 加 `overflow:hidden`。
- **缩放出白闪**：去掉 `#world` 白底与 `will-change:transform`；`#vp` 恢复深色底；LOD 不再 `display:none` 整层 `#labels`；平移用亚像素 `translate3d`；子区 `unmount` 延迟 120ms 减少阈值闪烁。
- **缩到最小时大区名消失**：`lodZRgn()` 随 `minZoom()` 下调，宽屏缩到顶仍显示 ~87 个大区名。

---

## [2026-07-06]

### Performance

- **底图瓦片化（Leaflet）**：以 `L.CRS.Simple` 接入 Leaflet 1.9.4（本地 vendored，无 CDN、无构建依赖），底图从整张 `1366.svg` 改为 `tiles/{z}/{x}/{y}.webp` 瓦片金字塔（z0–z3、256px、共 5044 张约 21MB，视口按需加载）。原 SVG 内嵌 8145×7801 PNG（base64 约 3.3MB，解码后占用 250MB+ 纹理内存）不再加载，首屏仅需数十 KB 瓦片，平移/缩放开销恒定。
- 瓦片自矢量源高清渲染：z3 全图 16064×15360，retina 设备自动取高一级瓦片（`tileSize:128` + `zoomOffset:1`），450% 最大缩放下仅约 1.12× 上采样，线稿清晰。
- 分析确认：原内嵌大 PNG 实为纯色海洋底（#3895BF 圆角矩形），海岸线/山脉/河流/边界均为矢量 path，已随瓦片一并渲染。

### Changed

- 平移/缩放/惯性/边界钳制改由 Leaflet 驱动：`zoomSnap:0` 平滑分数缩放，`maxBounds` + `viscosity:1` 等效原 `clampPan`，动态 `minZoom` 仍保证地图铺满视口。
- `#world`（geo-svg + labels + title card）作为自定义 `L.Layer` 挂载到 overlayPane，transform 公式与原实现一致（leaflet zoom = log2(原 zoom)）：LOD 阈值、`--inv` counter-scale、`data.json` 百分比坐标、URL hash 分享编码全部不变。
- `index.html` 不再引用 `1366.svg`（文件保留在仓库作为矢量源）；新增 `leaflet/`（js+css）与 `tiles/` 目录。

---

## [2026-05-15]

### Mobile

- 视口 `touch-action: none`，由页面自行处理双指缩放与拖拽。
- iOS 点击词条去除蓝色高亮（`-webkit-tap-highlight-color`）。
- 拖拽/捏合期间暂时关闭 label 层 `pointer-events`，减轻命中检测开销。
- 精简部分 UI 样式（如移除 legend 的 `backdrop-filter` 等），利于移动端合成。

### Changed

- 将 `index_v6.html` 重命名为 `index.html`，作为 GitHub Pages 入口。

### Added

- 中文互动地图 **v6**：`index.html`（单文件、无构建依赖）、`data.json`（523 词条坐标与边界元数据）、`README.md`。
- 地图导航：滚轮/双指缩放、拖拽平移、动态最小缩放（地图始终铺满视口）。
- 分级显示（LOD）：大区 ~70%、子区 ~130%、地理地貌弧线 ~200%。
- 词条五态点击循环、URL hash 分享（deflate + base64url）。
- 中英双语切换；地理地貌中文弧线 / 英文直排。
- 大区 label 网格碰撞避让；counter-scale（`--inv`）保持屏幕字号恒定。
