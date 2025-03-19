# SSH权限配置
可能报错：
```bash
dfs-start.sh 

localhost: Warning: Permanently added 'localhost' (ED25519) to the list of known hosts.
localhost: fly@localhost: Permission denied (publickey,password,keyboard-interactive).
Starting datanodes
localhost: fly@localhost: Permission denied (publickey,password,keyboard-interactive).
```
表明在尝试通过 SSH 连接 localhost 时，身份验证失败了。通常是由于 公钥/私钥 或 密码 认证失败。

## 生成SSH 密钥对：
```bash
ssh-keygen -t rsa
```
## 将公钥添加到 ~/.ssh/authorized_keys 文件中：
```bash
cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys
```
## 文件权限
```bash
chmod 600 ~/.ssh/authorized_keys
```
确保 ~/.ssh/authorized_keys 是正确的：

bash
复制
编辑
chmod 600 ~/.ssh/authorized_keys
## 试着通过 SSH 登录本地：
```bash
ssh localhost
```
如果 SSH 连接成功且没有要求密码，那就说明公钥认证已经设置好了。

