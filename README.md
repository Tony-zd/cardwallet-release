# CardWallet 卡包应用 · 公开下载仓库

> 本仓库专门用于存放 CardWallet 卡包应用的发布产物，与代码仓库分离。
> 代码仓库为私有，不对外开放。

## 下载

- **最新版本**：[v1.4.1](apk/cardwallet-latest.apk)
- **历史版本**：见 [apk/](apk/) 目录

## 文件结构

| 文件/目录 | 说明 |
|---|---|
| `apk/cardwallet-latest.apk` | 最新版 APK（始终是最新） |
| `apk/cardwallet-vX.Y.Z.apk` | 历史归档 APK |
| `CHANGELOG.md` | 版本更新日志（App 内检查更新会读取此文件） |
| `index.html` | 下载引导页（部署到 Gitee Pages / GitHub Pages） |
| `pay-qrcode.png` | 激活码购买付款码（App 内购买激活码弹窗展示） |

## 检查更新机制

App 内「我的 → 检查更新」会通过 **jsDelivr CDN** 拉取本仓库的 `CHANGELOG.md`，提取最新版本号与用户对比：

```
https://cdn.jsdelivr.net/gh/zhongda-st/cardwallet-release@main/CHANGELOG.md
```

> jsDelivr CDN 提供 CORS 支持，App WebView 可直接 fetch。
> 拉取的是公开仓库，无需 token，无访问限制。

## 部署下载页

### Gitee Pages

1. 进入仓库 → 服务 → Gitee Pages
2. 部署分支：`main`，目录：`/`
3. 访问地址：`https://zhongda-st.gitee.io/cardwallet-release/`

### GitHub Pages

1. 进入仓库 → Settings → Pages
2. Source：`Deploy from a branch`，分支 `main` / `root`
3. 访问地址：`https://tony-zd.github.io/cardwallet-release/`

## 新版本发布流程

1. 在代码仓库打包新版 APK
2. 复制 APK 到本仓库 `apk/` 目录，同时覆盖 `cardwallet-latest.apk`
3. 更新 `CHANGELOG.md`，在顶部新增 `## vX.Y.Z — YYYY-MM-DD` 段落
4. 提交并推送到 Gitee/GitHub
5. 等待 jsDelivr CDN 缓存刷新（约 10 分钟），App 即可检测到新版本

## 联系反馈

- 邮箱：sunt.flow@gmail.com
