# video-collector-tv

「简·TV播放器」（安卓 TV 版）的应用内更新仓库。App 启动后会读这里的 `version.json`，发现有更高的 `versionCode` 就在应用内下载并拉起系统安装器，不跳浏览器。

- 仓库：https://github.com/jerryzwj/video-collector-tv （public，只做更新分发）
- 清单：`version.json`
- APK：主地址放 Release（`tv-v<版本号>`），备用镜像放 `apk/` 目录（走 jsDelivr 加速）

## version.json 字段

| 字段 | 说明 |
|---|---|
| `versionCode` | 整数，App 拿它比对，比本机大才算有新版本 |
| `versionName` | 显示用，如 `1.0.2` |
| `apkUrl` | 主下载地址，GitHub Release 的 asset 直链 |
| `apkUrlMirror` | 备用地址，jsDelivr 在国内更稳 |
| `mirrors` | 可选，更多备用地址数组 |
| `size` | APK 字节数，下载后校验 |
| `md5` | 可选，下载后校验 |
| `notes` | 更新说明，直接显示在更新弹窗里 |

App 端读取顺序：jsDelivr → GitHub raw → gitmirror，任意一个拿到清单就停；APK 下载同样是 `apkUrl` 失败自动换 `apkUrlMirror`。

## 发新版的流程

1. `app/build.gradle.kts` 里 `versionCode` +1、`versionName` 改新版号，`build.ps1` 打 APK
2. 本仓库建 Release，tag 用 `tv-v<版本号>`，把 APK 传上去
3. APK 同时放到 `apk/` 目录（jsDelivr 镜像，单文件限 20MB）
4. 改 `version.json` 的 `versionCode` / `versionName` / `apkUrl` / `size` / `md5` / `notes`

jsDelivr 有几分钟缓存，发完新版不是立刻生效，急的话把 `apkUrlMirror` 换成 raw 地址。
