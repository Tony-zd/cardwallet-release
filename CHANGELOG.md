# 卡包应用版本日志（CHANGELOG）

> 本文件记录所有正式发布的版本。每次发版前必须更新此文件，并在 app.js 的 `openChangelog()` 函数中同步更新用户可见的更新日志。

---

## v1.4.9 — 2026-10-10

### 🐛 Bug 修复

1. **移除 QQ 联系方式**：问题反馈、隐私政策、用户协议中的 QQ 号已删除，仅保留邮箱联系

---

## v1.4.8 — 2026-10-10

### 🐛 Bug 修复

1. **修复付款弹窗键盘遮挡输入框**：输入邮箱时弹窗自动上移，不再被键盘遮挡
2. **优化付款通知**：付款后自动生成激活码，通过一条 Server酱 通知推送（含激活码）

---

## v1.4.7 — 2026-10-09

### ✨ 功能改进

1. **付款通知全流程打通**：用户付款后通过 Server酱 推送微信通知，管理员可一键生成激活码并通知用户
2. **激活码自动绑定设备**：生成激活码后自动更新设备激活状态，用户输入即激活

### 🐛 Bug 修复

1. 优化后端存储架构，付款记录和试用数据统一存储，提升稳定性

---

## v1.4.6 — 2026-10-08

### 📝 下载页优化

1. **下载页介绍改为功能导向**：突出「银行卡号一键复制」「证件照随时打印」核心价值，不再以安全为主要宣传点
2. **移除下载页更新日志模块**：该模块内容为空，不再展示

### 📝 App 内更新日志优化

1. 技术性条目（CDN 切换、仓库调整、架构变更等）统一归类为「Bug 修复」
2. 用户可见的功能改进单独列出，更新日志更清晰易读

---

## v1.4.5 — 2026-10-08

### 🔧 缓存修复（覆盖安装生效）

1. **WebView 禁用缓存**：MainActivity 设置 `LOAD_NO_CACHE` + 启动时 `clearCache(true)` + ServiceWorkerController 禁用缓存
2. **Service Worker 改 network-first**：移除 app.js/styles.css 预缓存，所有同源请求网络优先（本地资源必定成功），彻底避免旧代码缓存
3. **index.html 加 no-cache meta**：Cache-Control/Pragma/Expires 三重禁用
4. **缓存破坏版本号**：app.js/styles.css 查询串更新为 `?v=1.4.5`

### 🔧 下载入口切换

1. **下载主入口改为 GitHub Pages**：Gitee Pages 已下线，App 内分享和检查更新下载按钮均指向 `https://tony-zd.github.io/cardwallet-release/`
2. **下载页 PC/移动响应式**：PC 端双栏布局（左品牌介绍+右下载卡），移动端单栏

---

## v1.4.4 — 2026-10-08

### 🎨 UI 优化

1. **「我的」页高级会员行**：去掉突兀的绿色/紫色渐变背景，改为白色卡片 + 图标容器颜色区分（琥珀金👑/紫色💎/深紫🔑）+ 右侧小徽章（已激活/随心转账），与设置、分享下载等菜单完全协调

---

## v1.4.3 — 2026-10-08

### 🔧 修复

1. **重新打包**：v1.4.2 APK 中实际打包的是旧 jsDelivr URL，v1.4.3 重新打包确保 App 内为最新 Gitee raw URL
2. **下载页备用链接**：改为 GitHub raw（支持 12MB APK 直链，不受 Gitee 100MB 限制）
3. **公开仓双推**：Gitee + GitHub 双平台冗余备份

---

## v1.4.2 — 2026-10-08

### 🔧 架构调整

1. **代码与发布产物分离**：新建公开仓库 `cardwallet-release`（Gitee + GitHub），专用于 APK 发布，与私有代码仓分离
2. **检查更新改用 jsDelivr CDN 直连公开仓**：撤销 v1.4.1 的云函数代理方案（不再依赖 CloudBase + Gitee token）
3. **付款码图片改用 jsDelivr CDN**：从公开仓拉取，App 内购买激活码弹窗可正常显示
4. **下载页改用公开仓 Pages**：`https://zhongda-st.gitee.io/cardwallet-release/` 和 `https://tony-zd.github.io/cardwallet-release/`

### 🐛 修复

- 修复检查更新失败：原方案 Gitee raw URL 因仓库私有 + 访问限制返回 403

### 📦 公开仓结构

- `apk/cardwallet-latest.apk` 最新版
- `apk/cardwallet-vX.Y.Z.apk` 历史版本
- `CHANGELOG.md` 版本日志（App 检查更新拉取源）
- `index.html` 下载引导页
- `pay-qrcode.png` 付款码

---

## v1.4.1 — 2026-10-08

### 🐛 修复

1. **检查更新失败**：原方案直接调 Gitee raw URL，因仓库私有 + Gitee raw 限制返回 403 而失败
2. **改为云函数代理**：CloudBase 云函数带 token 调 Gitee API v5 获取 CHANGELOG.md，解析最新版本号 + 更新要点，App 端无 token 泄露风险
3. **更新弹窗新增更新要点**：云函数解析 CHANGELOG 提取该版本的关键变更条目，弹窗展示给用户

### 🔧 技术细节

- 云函数新增 `fetchLatestVersion()`：通过 Gitee API v5 获取 CHANGELOG.md base64 内容，提取 `## vX.Y.Z` 版本号
- 云函数新增 `action: 'version'` POST 接口：返回 `{ ok, version, notes, downloadUrl }`
- App `checkUpdate()` 改为 POST 云函数 `version` action（复用现有签名机制）
- 需在云函数环境变量配置 `GITEE_TOKEN`（用户已在 Gitee 生成的私人令牌）

---

## v1.4.0 — 2026-10-08

### ✨ 新功能 / 调整

1. **品牌升级**：「专业版」全面更名为「高级会员」，名称更贴近用户视角
2. **自由定价**：激活定价从固定 ¥9.9 改为「随心转账 · 多少随你」
   - 购买弹窗新增「转账金额（可选）」输入框，用户可自主填写实际转账金额
   - 后端付款记录保存用户填写的金额，未填则默认「随心转账」
3. **付款码图片正式启用**：付款码图片（微信个人转账码）已上传 Gitee 仓库 assets 目录

### 🔧 技术细节

- `ACTIVATION_PRICE` 由 `'¥9.9'` 改为 `'随心转账'`
- `submitPaymentInfo(email)` → `submitPaymentInfo(email, amount)` 支持动态金额
- CloudBase 云函数默认 amount 同步调整为 `'随心转账'`
- 全局 `专业版` → `高级会员` 共 8 处文案替换

---

## v1.3.9 — 2026-10-08

### 🔧 优化

1. **「我的」页激活入口合并**：移除已激活状态下冗余的「点击查看设备信息」独立弹窗，已激活用户点击「高级会员已激活」入口直接跳转设置页查看详情
2. **设置页高级会员区块统一**：已激活状态下新增「设备 ID」展示项（带复制按钮），与激活状态、激活码集中展示，避免信息分散
3. **设备 ID 异步获取**：优先使用缓存的 deviceId，缺失时异步获取并回填，提升响应速度

### 🐛 修复

- 删除冗余的 `showActivationStatus()` 函数及事件委托分支

---

## v1.3.8 — 2026-10-08

### ✨ 新功能

1. **购买激活码**：「我的」页新增购买入口，展示远程付款码，用户输入邮箱后点击「我已付款」提交设备ID+邮箱到 CloudBase
2. **输入激活码**：独立入口，激活后绑定当前设备
3. **设备信息展示**：已激活用户可查看并复制设备 ID
4. **版本更新检测**：设置页「检查更新」从 Gitee 拉取 CHANGELOG 对比版本，有新版本提示下载
5. **分享下载**：「我的」页新增分享入口，复制或系统分享下载链接
6. **下载引导页**：仓库根目录 `index.html`，可部署到 GitHub Pages / Gitee Pages
7. **个人资料增加邮箱字段**：用于接收激活码，备份恢复时包含

### 🔧 技术细节

- 付款码图片从 Gitee 远程加载，可随时更换无需重新打包
- 付款信息复用现有 CloudBase HTTP 接口 + HMAC 签名校验
- 激活码仍为本地 HMAC-SHA256 校验，无需服务器

---

## v1.3.7 — 2026-10-07

### 🐛 Bug 修复

1. **"我的"页类型查看区分卡片类型与常用类型**：点击"卡片类型"显示所有用户拥有的类型；点击"常用类型"只显示数量 ≥ 2 的类型

---

## v1.3.6 — 2026-10-07

### 🐛 Bug 修复

1. **修复手动排序不生效的根因**：onDragPointerUp 在 dragCleanup() 之前调用 getLiveItems()，避免 dragList 被清空后返回空数组导致 customOrder 无法更新
2. **优化"我的"页类型查看**：点击"卡片类型"/"常用类型"现在只显示用户实际有的类型列表（按数量降序），不再列出全部固定类型

---

## v1.3.5 — 2026-10-07

**Commit**: 待提交

### 🐛 修复 & 优化

1. **手动排序持久化**：拖拽排序后持久化 `settings.sortBy = 'custom'`，重启后仍生效
2. **全局禁止复制/选中**：CSS 全局 `user-select: none` + JS 禁止 contextmenu/selectstart（输入框除外）
3. **头像和昵称备份恢复**：导出/导入备份时包含 nickname 和 avatar
4. **"我的"页隐藏搜索和排序**：这两个功能只在卡片列表页显示
5. **卡片类型指标可点击**：点击"卡片类型"/"常用类型"弹出所有类型明细
6. **PDF 保存位置只读**：改为灰色只读显示，不再可修改
7. **更新日志简化**：改为用户友好文案，不展示技术细节
8. **身份证反面按钮统一**：拍照 → 拍照识别，与正面一致

---

## v1.3.4 — 2026-10-07

**Commit**: 待提交

### 🐛 Bug 修复

- **修复覆盖安装后更新日志缺失**：根因是 `index.html` 中 app.js 的 cache buster 参数还是老版本 `?v=2.1.5`，WebView 命中旧 JS 缓存导致 changelog 不更新
  - 修复：更新 index.html 中的 cache buster 为 `?v=<versionName>-<versionCode>`，每次发版自动更新
  - 效果：覆盖安装后 WebView 会强制加载最新 JS，版本号、changelog、功能变更全部即时生效
- **发版 Checklist 新增 cache buster 检查项**：防止后续发版遗漏

---

## v1.3.3 — 2026-10-07

**Commit**: 待提交

### ✨ 新功能

- **内置默认 OCR 配置**：开箱即用，用户无需自配百度 OCR 密钥即可使用云端 OCR 识别
  - 凭证信息硬编码在 app.js 中，**不展示给用户**
  - 用户在「设置 → OCR 识别」看到的提示：应用已内置默认配置，可直接使用
- **每月 50 次免费额度**：内置默认配置共享每月 50 次调用上限
  - 用 localStorage 记录当月已用次数（按月份自动重置）
  - 接近上限（剩余 ≤ 5 次）时温和提示用户自配
  - 用尽后引导用户在「设置 → OCR 识别」配置自己的密钥继续使用
- **设置页新增「本月默认额度」状态行**：实时显示剩余 / 总次数
- **自配优先级**：用户自配密钥后不占用应用默认额度，无次数限制

### 🔧 改动

- `getBaiduAccessToken()`：从 `state.settings` 取凭证改为 `getActiveOcrCreds()`，返回 `{ apiKey, secretKey, isDefault }`
- token 缓存增加 `isDefault` 字段，用户切换自配/默认时自动刷新 token
- 设置页 API Key / Secret Key 输入框 placeholder 改为「选填」

### 🔒 安全

- 内置 OCR 凭证写在源码中（APK 已混淆），但仓库私有 + 已开启 R8 代码混淆
- 凭证可被反编译获取，仅作"防止普通用户直接白嫖"用，**不是真正的密钥保护**
- 如需更换凭证，修改 app.js 中 `DEFAULT_BAIDU_OCR` 后重新打包

---

## v1.3.2 — 2026-10-07

**Commit**: `d54a643` (打包本次 v1.3.2 的二次更新版本)

### 🐛 Bug 修复

- **修复通知功能失效**：新增 `LocalNotificationsPlugin.java` 原生插件，与 Capacitor 标准 LocalNotifications 接口完全兼容
  - 修复根因：原 MainActivity 未注册此插件，前端调用拿到的总是 null
  - 接口实现：`checkPermissions` / `requestPermissions` / `createChannel` / `schedule`
- **修复系统设置中无法打开应用通知的问题**
  - 根因：AndroidManifest 未声明 `POST_NOTIFICATIONS` 权限（Android 13+ 必需）
  - 修复：在 AndroidManifest.xml 添加 `android.permission.POST_NOTIFICATIONS` 声明

### 📦 工程规范

- 修正「版本」菜单项的显示，去掉括号中的 versionCode 数字
- 新增 `构建环境备忘.md` 文档，固化打包工具版本，避免后续重试
- 新增 `release-history/` 目录，按版本号归档 APK，不覆盖旧版本

### 📝 同步更新

- app.js 默认版本号 → `1.3.2 / versionCode 7`
- app.js `openChangelog()` 加入 v1.3.1 / v1.3.2 条目

---

## v1.3.1 — 2026-10-07

**Commit**: `fd95d79`

### ✨ 调整

- **试用次数统一为 10 次**（OCR + PDF 导出，原为 5 + 3）
- **联系方式更新**：
  - 邮箱：`sunt.flow@gmail.com`
  - QQ：`3159743212`
- **固定正式签名 keystore**：解决「换 APK 就丢 deviceId」问题
  - 启用 25 年有效期正式签名（debug.keystore 替换为 cardwallet-release.keystore）
  - 同一用户在同一设备重装 APK 后，ANDROID_ID 保持稳定，激活码继续有效
- **支持覆盖安装**：versionCode 递增 + 签名一致 → 系统允许覆盖升级，数据保留

---

## v1.3.0 — 较早版本

### 🔒 安全加固

- 卡片与设置经 Android Keystore AES-256-GCM 加密存储
- 关闭 allowBackup，数据不可通过系统备份外泄
- WebView 加固：禁用文件访问，防止恶意脚本读取本地文件
- 启用 R8 代码混淆，提升反编译难度

### ⚙️ 功能

- Root 检测：检测到设备已 Root 时提示安全风险
- 统一弹窗样式：帮助说明、问题反馈、导出结果等改为居中浮层

### 📄 法律文案

- 隐私政策、用户协议、SDK 清单参考行业规范重写

---

## v1.2.0 — 较早版本

### ✨ 功能

- 集成 SmartCropper 智能裁剪，自动识别卡片边框
- 新增隐私政策、用户协议、SDK 清单

### 🐛 修复

- 修复应用内版本号与系统版本号不一致
- 关闭数据备份，加固 WebView 安全

### 📦 优化

- 优化 APK 体积（仅保留 arm64-v8a）

---

## v1.1.0 — 早期版本

### ✨ 功能

- 新增卡片旋转裁剪功能
- 支持导出备份到公共下载目录
- 备份包含全部配置数据
- 默认主题改为浅色

---

## 发版 Checklist（每次发版必走）

1. [ ] 更新 `app/build.gradle` 的 `versionCode`（至少 +1）和 `versionName`
2. [ ] 更新 `app.js` 中的 `let appVersion = { versionName: 'x.x.x', versionCode: N };`
3. [ ] 更新 `index.html` 中的 cache buster：`<script src="app.js?v=versionName-versionCode">`
4. [ ] 在 `app.js` 的 `openChangelog()` 函数顶部追加新版本条目
5. [ ] 在本文件顶部追加新版本条目
6. [ ] 同步 PWA 源码（app.js + index.html）到 Android 工程的 `assets/public/`
7. [ ] 运行 `./gradlew assembleRelease`
8. [ ] 验证签名：`apksigner verify --print-certs <APK>` 看 SHA-256 是否为 `00a57aa0...508e`
9. [ ] 验证版本号：`aapt dump badging <APK> | grep versionCode`
10. [ ] 验证 changelog 内嵌：`unzip -p <APK> assets/public/app.js | grep -c "v新版本号"`
11. [ ] 验证 cache buster：`unzip -p <APK> assets/public/index.html | grep "app.js"`
12. [ ] 归档：`cp <APK> 04-APK产物/release-history/cardwallet-v<versionName>.apk`
13. [ ] 更新指向：`cp <APK> 04-APK产物/cardwallet-release.apk`
14. [ ] 提交并推送到 GitHub + Gitee
