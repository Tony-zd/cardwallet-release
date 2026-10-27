# CardWallet 信息卡包 · 公开下载仓库

> 本仓库专门用于存放 CardWallet 信息卡包的发布产物，与代码仓库分离。
> 代码仓库为私有，不对外开放。

## 下载

- **最新版本**：[v2.2.5](apk/cardwallet-latest.apk)
- **历史版本**：见 [apk/](apk/) 目录

## 文件结构

| 文件/目录 | 说明 |
|---|---|
| `apk/cardwallet-latest.apk` | 最新版 APK（始终是最新） |
| `apk/cardwallet-vX.Y.Z.apk` | 历史归档 APK |
| `CHANGELOG.md` | 版本更新日志（App 内检查更新会读取此文件） |
| `index.html` | 下载引导页（部署到 Gitee Pages / GitHub Pages） |
| `pay-qrcode.png` | 激活码购买付款码（App 内购买激活码弹窗展示） |
| `promo/` | 推广物料：海报 poster.png（1080×1440）+ 小红书软文 + 二维码与海报源文件 |

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
| Gitee CDN 直链（主） | https://raw.giteeusercontent.com/zhongda-st/cardwallet-release/raw/main/apk/cardwallet-latest.apk | 国内 CDN，**无需登录**，海报二维码即此链 |
| Gitee Release 附件 | https://gitee.com/zhongda-st/cardwallet-release/releases/download/v2.2.4/cardwallet-latest.apk | 国内备用；**微信 WebView 内会跳微信 OAuth 登录墙**，需在浏览器打开 |
| Gitee Release 下载页 | https://gitee.com/zhongda-st/cardwallet-release/releases/tag/v2.2.4 | 版本说明 + 全部通道（普通浏览器匿名可看） |
| GitHub Pages | https://tony-zd.github.io/cardwallet-release/ | 备用入口，页面内含全部下载通道 |
| GitHub Raw | https://raw.githubusercontent.com/Tony-zd/cardwallet-release/main/apk/cardwallet-latest.apk | 海外直链，国内访问慢 |
| Gitee Raw | https://gitee.com/zhongda-st/cardwallet-release/raw/main/CHANGELOG.md | CHANGELOG 直链，App 检查更新源 |

> **Gitee 行为备注（2026-10 实测）**：
> - `gitee.com` 域名的 raw/attach 路径对**微信 WebView（UA 含 MicroMessenger）强制跳转微信 OAuth**——微信内扫码会撞登录墙
> - `raw.giteeusercontent.com` 独立域名（路径需带 `/raw/`）不经过认证网关，匿名 + 微信 UA 均可直下
> - Gitee 官方规则：公开仓 raw 文件 **>10MB 需认证**，APK 须保持 <10MB（当前 9.8MB，接近上限，需关注体积）
> - jsDelivr（cdn.jsdelivr.net）无登录但国内时快时慢，仅作兜底
> - Gitee Pages（gitee.io 域名）完全无验证，但需实名认证 + 网页端手动开启 + 每次推送后手动点「更新」

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
5. **创建 Gitee Release 并上传 APK 附件**（API，约 1 分钟；二维码指向 `cardwallet-latest.apk` 固定路径，无需改动）：
   ```bash
   TOKEN=<gitee token>
   # 建 Release（target_commitish 必填）
   curl -X POST "https://gitee.com/api/v5/repos/zhongda-st/cardwallet-release/releases?access_token=$TOKEN" \
     -H "Content-Type: application/json" \
     -d '{"tag_name":"vX.Y.Z","name":"信息卡包 vX.Y.Z","target_commitish":"main","body":"<下载页 Markdown>"}'
   # 上传附件（取上一步返回的 id）
   curl -X POST "https://gitee.com/api/v5/repos/zhongda-st/cardwallet-release/releases/<id>/attach_files?access_token=$TOKEN" \
     -F "file=@apk/cardwallet-latest.apk;type=application/vnd.android.package-archive"
   ```
6. App 检查更新时即时拉取最新 CHANGELOG（GitHub raw 有约 5 分钟缓存）

## 联系反馈

- 邮箱：sunt.flow@gmail.com
