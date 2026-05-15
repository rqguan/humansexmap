# 人类性癖地图 · 中文互动版

**A Map of the Lands of Human Sexuality — Chinese Interactive Edition**

原作 [Franklin Veaux](https://humansexmap.com) · 中文互动版制作 Enzo · 已获原作者授权

---

## ⚠️ 版权声明

本项目为 **[A Map of the Lands of Human Sexuality](https://humansexmap.com)** 的中文互动改编版本，已获得原作者 Franklin Veaux 授权，仅供非商业使用。

商业用途请联系原作者：[humansexmap.com](https://humansexmap.com)

---

## 关于

将 Franklin Veaux 的原版性向地图重制为可交互的中文网页。地图涵盖 **87 个大区、436 个子区词条、18 条地理地貌**，共 523 个可标记条目。

用户可以对每个词条打分，生成一个包含自己所有选择的专属分享链接，发送给伴侣或朋友互相了解。

---

## 功能

**地图导航**
- 滚轮 / 双指捏合 缩放，拖拽平移
- 地图始终填满屏幕，不会出现黑色背景区域
- 初始视角定位在标题区，可自由探索
- `+` / `-` / `0` 键盘快捷键

**分级显示**
- 缩放到 70% 以上：大区名称出现
- 缩放到 130% 以上：子区词条出现
- 缩放到 200% 以上：地理地貌弧线文字出现

**词条标记**
- 点击任意词条弹出状态面板
- 五种状态，颜色区分：

  | 圆点 | 含义 |
  |------|------|
  | ⚫ 灰 | 未选择（默认可见） |
  | 🔴 红 | 讨厌 |
  | 🟡 黄 | 可以尝试 |
  | 🟢 绿 | 喜欢 |
  | 🟣 紫 | 非常喜欢 |

**中英双语**
- 工具栏点击 `EN` / `中文` 一键切换所有词条语言
- 地理地貌中文显示弧线排版，英文切换为直排（避免长名称截断）

**分享**
- 点击「分享」生成专属链接
- 所有选择压缩编码进 URL，无需登录，无需服务器
- 对方打开链接即可看到你的完整标注

---

## 文件说明

```
index.html    主页面（所有逻辑内嵌，无依赖）
1366.svg      地图底图（矢量，无文字）
data.json     词条数据、坐标与边界（将你的 JSON 改名为此）
README.md     本文件
```

> **重要**：`data.json` 是固定文件名。如果你的 JSON 文件叫 `sexmap_annotated_v7.json`，上传前请改名为 `data.json`。

---

## 本地运行

不能直接双击打开 HTML（`file://` 协议下数据加载会失败），需要本地服务器：

```bash
# 进入项目目录
cd 你的项目文件夹

# 任选一种方式启动本地服务器
python3 -m http.server 8080
npx serve .

# 然后访问
http://localhost:8080
```

---

## 部署到 GitHub Pages

```bash
# 初始化并推送（已有仓库跳过前两行）
git init
git remote add origin https://github.com/你的用户名/仓库名.git

# 确保 JSON 已改名为 data.json，然后提交
git add index.html 1366.svg data.json README.md
git commit -m "deploy"
git push -u origin main
```

在 GitHub 仓库页面开启 Pages：

**Settings → Pages → Source → Deploy from a branch → main / (root) → Save**

约 1–3 分钟后生效，访问地址：`https://你的用户名.github.io/仓库名/`

---

## 更新数据

每次用标注工具导出新版本后：

```bash
# 将导出文件改名为 data.json，然后推送
cp 新导出的文件.json data.json
git add data.json
git commit -m "update data"
git push
```


---

## 致谢

- 原作：**Franklin Veaux** — [humansexmap.com](https://humansexmap.com)
- 中文翻译与互动版制作：**Enzo - [bysbxh@gmail.com]**

---

<sub>本项目仅供成年人在合法环境下参考使用。内容涉及成人话题，请确认你已年满 18 岁。</sub>
