# macOS SSH 连接到localhost：

## 1. 检查 SSH 服务是否开启
   macOS 默认安装了 OpenSSH，但 SSH 服务器可能未启用。你可以用以下命令检查 SSH 服务状态：

```bash
sudo systemsetup -getremotelogin
```
如果返回：
```bash
Remote Login: On
```
说明 SSH 服务已开启。如果是：
```bash
Remote Login: Off
```
可以运行以下命令开启 SSH 服务：
```bash
sudo systemsetup -setremotelogin on
```




