# CardWallet 卡包应用 · 公开下载仓库

> 本仓库专门用于存放 CardWallet 卡包应用的发布产物，与代码仓库分离。
> 代码仓库为私有，不对外开放。

## 下载

- **最新版本**：[v1.4.4](apk/cardwallet-latest.apk)
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

App 内「我的 → 检查更新」通过 **Capacitor HttpNative**（原生 HTTP 客户端）直接拉取本仓库的 `CHANGELOG.md`，提取最新版本号与用户对比：

```
https://gitee.com/zhongda-st/cardwallet-release/raw/main/CHANGELOG.md
```

> HttpNative 走原生网络层，**不受 WebView CORS 限制**。
> 公开仓库匿名访问，无需 token。

## 下载源

| 通道 | URL | 备注 |
|---|---|---|
| Gitee Pages | https://zhongda-st.gitee.io/cardwallet-release/ | 主入口（需先开启 Pages） |
| GitHub Pages | https://tony-zd.github.io/cardwallet-release/ | 备用入口（需先开启 Pages） |
| GitHub Raw | https://raw.githubusercontent.com/Tony-zd/cardwallet-release/main/apk/cardwallet-latest.apk | APK 直链，无需 Pages |
| Gitee Raw | https://gitee.com/zhongda-st/cardwallet-release/raw/main/CHANGELOG.md | CHANGELOG 直链，App 检查更新源 |

> GitHub raw 支持 12MB APK 直链下载（Gitee raw 对大文件返回 403）。

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
4. 提交并推送到 Gitee 和 GitHub
5. App 检查更新时即时拉取最新 CHANGELOG（GitHub raw 有约 5 分钟缓存）

## 联系反馈

- 邮箱：sunt.flow@gmail.com
