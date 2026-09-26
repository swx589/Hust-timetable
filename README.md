# Hust课表 · 下载页（GitHub Pages 发布目录）

这个文件夹里的东西就是要传到 GitHub 的内容：

| 文件 | 说明 |
| --- | --- |
| `index.html` | 下载页（单文件，16.8 KB，无外部依赖、无二维码模块） |
| `hust-timetable.apk` | 安装包（57.5 MB，与页面同目录，页面用相对路径 `hust-timetable.apk` 下载） |
| `.nojekyll` | 空文件，告诉 GitHub Pages 不要用 Jekyll 处理，保证静态文件原样发布 |
| `README.md` | 本说明，传上去也行、删掉也行（不影响页面） |

已配置的远端：`git@github.com:swx589/Hust-timetable.git`（SSH），仓库里已有一次 `first commit`。
对应的 Pages 地址形如：**https://swx589.github.io/Hust-timetable/**

直接下载apk文件直链：**https://www.hnykxd.me/fileovo/files/7d89039c63fd6ae5/hust-timetable.apk**

---

## 一、把改动推上去

改了 `index.html`（去掉了二维码模块）之后：

```powershell
cd C:\Users\59302\Desktop\hust-timetable-pages
git add -A
git commit -m "下载页去掉二维码模块"
git push
```

> 远端是 SSH 方式；如果这台机器访问 `github.com:443` 被阻断（`Connection was reset` / 超时），先开代理再 push。
> 用 GitHub Desktop 的话：该文件夹已经是仓库，直接 Commit → Push 即可。

## 二、开启 Pages（只在第一次需要）

仓库页面 → **Settings → Pages** → Build and deployment：
- **Source**：`Deploy from a branch`
- **Branch**：`main`（或你用的分支）/ 目录选 `/ (root)` → **Save**

等 1~2 分钟，站点地址就是 `https://swx589.github.io/Hust-timetable/`。

## 三、分享

- 这个地址就是给同学的**下载页链接**；页面里有「下载安卓版 APK」和「复制本页链接 / 复制下载直链」两个按钮，转发时用得上。
- **微信里不能直接下载 APK**：页面会自动显示提示条（「点右上角 ⋯ → 在浏览器打开」），分享时最好也补一句提醒。

## 四、以后发新版本

在项目里跑一条命令，再把两个文件覆盖过来推一次：

```powershell
# 在 C:\Users\59302\Desktop\课表APP 下
pwsh -File publish\publish_release.ps1 -Version 1.1.0 -Notes "修复xxx"
# 脚本会重新构建 APK、更新页面上的版本/大小/SHA256、生成 update.json

# 覆盖到这个文件夹并推送
copy C:\Users\59302\Desktop\课表APP\publish\index.html         C:\Users\59302\Desktop\hust-timetable-pages\index.html
copy C:\Users\59302\Desktop\课表APP\publish\hust-timetable.apk C:\Users\59302\Desktop\hust-timetable-pages\hust-timetable.apk
cd C:\Users\59302\Desktop\hust-timetable-pages
git add -A; git commit -m "v1.1.0"; git push
```

记得同时把项目里 `pubspec.yaml` 的 `version: x.y.z+N` 的 **N 递增**（以后做应用内更新时会用到）。

## 五、几个要注意的地方

1. **单文件 100 MB 上限**：APK 现在 57.5 MB 能推；哪天超过 100 MB 就必须改用 OSS/网盘等外部直链。
2. **仓库会变大**：每发一版 git 历史里都会多留一份约 59 MB 的 APK。发十几个版本后建议新建仓库重来，或者把 APK 挪到对象存储，只让页面留在 Pages（把 `publish/index.template.html` 里的 `CONFIG.apkUrl` 改成 APK 的绝对直链，重跑 `build_page.ps1`）。
3. **国内访问 `github.io` 不稳定**：校园网/移动网络下经常很慢或打不开，同学反馈打不开时，把这两个文件放到 Gitee Pages 或阿里云 OSS 再发一份链接即可（页面不用改）。
4. **想再加回二维码**：见项目里 `publish/README.md` 的「恢复二维码」一节（库文件还在，加回模板里的占位符与模块即可）。
