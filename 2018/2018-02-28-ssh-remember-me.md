# SSH配置
## 1. 公钥登录
口令登录模式时记住密码的方式：
执行
```
ssh-keygen
```
会在~/.ssh/下面生成id_rsa.pub(公钥)和id_rsa(私钥)
然后通过
```
ssh-copy-id user@host
```
将自己的公钥储存在远程主机上，这样再次登录的时候，远程主机就会想用户发送一个随机的字符串，用户用自己的私钥加密后，再发回来；远程主机用事先存储好的公钥进行解密，如果成功，就证明该用户是可信的，允许直接登录shell，必须要密码了。

## 2. private key 登录
```
ssh -i private_key_file_path user@host
```
有可能无权限访问private_key_file，可以通过
```
chmod 600 private_key_file_path
```
设置

## 3. 别名登录
用
```
ssh jojo
```
来替代
```
ssh user@host
```
在~/.ssh/config中添加
```
# jojo
Host jojo
HostName x.x.x.x
User root
IdentityFile ~/.ssh/id_rsa

# jojo2
Host jojo2
HostName x.x.x.x
User root
IdentityFile ~/.ssh/privatekey.txt
```
第一个是密码形式，第二个是privatekey形式
