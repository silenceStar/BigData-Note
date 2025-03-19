# 全磁盘访问权限
在 macOS 上启用远程登录时，系统要求给予 全磁盘访问权限。你可以按照以下步骤授予权限：

报错：
```bash
(base) fly@FlydeMacBook-Pro hadoop-3.2.4 % sudo systemsetup -setremotelogin on

setremotelogin: Turning Remote Login on or off requires Full Disk Access privileges.
```
## 1. 打开系统设置
   点击 苹果菜单（屏幕左上角的苹果图标），然后选择 系统设置（System Settings）。
## 2. 进入隐私与安全性设置
   在左侧菜单中，选择 隐私与安全性（Privacy & Security）。
   向下滚动到 完全磁盘访问（Full Disk Access）。
## 3. 授予权限
   在 完全磁盘访问 部分，点击右下角的 锁形图标，输入管理员密码来解锁设置。
   然后在列表中找到 终端（Terminal），勾选它，允许它访问你的文件系统。