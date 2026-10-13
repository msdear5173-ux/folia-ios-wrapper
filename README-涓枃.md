# Folia iOS 网页封装工程（个人测试版）

这个项目用一个最小 iOS 应用壳打开 `https://folia.lanerc.me/`。它不是 Folia 官方应用，也不包含 Folia 网站的后端或源码。所有主要内容仍由网站在线加载，因此必须联网。

## 你需要知道的事

- Windows 本身不能运行 Xcode；此项目用 GitHub Actions 提供的 macOS runner 编译一个未签名 IPA。
- 然后在 Windows 上用 Sideloadly 通过你自己的 Apple ID 重新签名并安装到自己的 iPhone。
- 免费 Apple ID 的签名通常 7 天到期，到期后需要重新侧载/签名。
- 网站在 WKWebView 中的麦克风权限、语音播放与登录行为要以实机测试为准。此封装不能保证修复 Folia 网站本身的故障。
- 请不要把 Apple ID 密码交给陌生网站。只从 Sideloadly 官方网站下载软件，并仔细检查 iPhone 的授权提示。

## 步骤一：把项目传到 GitHub

1. 打开 https://github.com/ 并登录/注册账号。
2. 右上角 `+` → `New repository`，仓库名填 `folia-ios-wrapper`。建议选 `Private`，点击 `Create repository`。
3. 在仓库页面点 `Add file` → `Upload files`。
4. 解压本 ZIP，把里面的所有内容（包括 `.github` 文件夹、`FoliaApp.xcodeproj` 文件夹、`FoliaApp` 文件夹和 workflow 文件）上传到仓库根目录。注意：上传文件时要保留 `.github/workflows/build-ipa.yml` 的目录结构。
5. 点击 `Commit changes` 保存。

## 步骤二：在 GitHub 云端构建 IPA

1. 进入你的仓库，点 `Actions` 标签页。如果第一次使用，按提示启用 Actions。
2. 左边选 `Build unsigned Folia IPA` 工作流。
3. 点击 `Run workflow` → 再点击绿色 `Run workflow`。
4. 等运行结束，显示绿色勾号后，点进这次运行记录。
5. 在页面底部 `Artifacts` 区域下载 `Folia-unsigned-IPA`。下载文件是 ZIP 包，解压后取得 `Folia-unsigned.ipa`（如果浏览器下载的是 `.zip`，先按普通 ZIP 解压）。

## 步骤三：在 Windows 上签名并安装

1. 从 Sideloadly 官方网站 https://sideloadly.io/ 下载 Windows 版本。按其要求安装 Apple 官网版 iTunes/iCloud（不要使用 Microsoft Store 版时，遵循 Sideloadly 当前官方说明）。
2. 用数据线连接 iPhone，解锁并在手机上点“信任此电脑”。
3. 打开 Sideloadly，把 `Folia-unsigned.ipa` 拖到 IPA 区域，设备应显示为你的 iPhone。
4. 在 Apple ID 栏输入自己的 Apple ID 邮箱，点击 Start。按提示完成双重验证。不要把账号密码告诉其他人或输入到非官方工具里。
5. 等待 Sideloadly 完成签名及安装。
6. 若 iPhone 要求启用开发者模式：设置 → 隐私与安全性 → 开发者模式，按提示重启并确认。
7. 若出现开发者未受信任提示：设置 → 通用 → VPN 与设备管理（有些系统版本名称可能略有不同）→ 选择对应的开发者 App 并信任。
8. 回到主屏幕点 Folia 图标。首次使用麦克风时，按 iOS 提示允许访问。

## 常见问题

- **GitHub 构建失败**：打开失败的 workflow 查看日志；常见原因是项目文件上传时层级错误，导致找不到 `FoliaApp.xcodeproj`。
- **Sideloadly 安装失败**：先更新 Sideloadly 和 Apple 设备支持组件，换数据线/USB 口，并确认设备已解锁、信任电脑。不要关闭 iPhone 的安全机制来绕过报错。
- **应用 7 天后打不开**：用相同流程重新签名安装。不要删除/重装前就先清理网站数据；如有重要网站数据，先确认它是否与账号同步。
- **仍然没声音/麦克风无效**：打开 Folia 网站自身确认网页端语音可用，检查 iPhone 设置中的麦克风权限、音量、静音/蓝牙输出。网页壳无法保证修复网站或服务器问题。

## 重要提醒

本项目是个人实验性网页壳，不是 Folia 官方出品。若网站不允许内嵌运行、登录使用外部认证，或使用了 WKWebView 不支持的浏览器功能，仍可能需要修改代码。
