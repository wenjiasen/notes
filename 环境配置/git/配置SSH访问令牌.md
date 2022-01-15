# 配置SSH访问令牌

## 为github配置SSH访问令牌

### 获取Github SSH 秘钥
配置流程：
1. 使用`ssh-keygen`生成ssh秘钥对，如果已经有存在的秘钥对，则可跳过这一步。
2. 将ssh秘钥对加入本地钥匙链
3. 配置本地ssh配置
4. 将ssh秘钥对的公钥加入只github中。

#### 生成新的秘钥
使用`ssh-keygen`生成新的秘钥，
```shell
# 将邮箱替换成自己的github邮箱
ssh-keygen -t ed25519 -C "your_email@example.com"
```
生成秘钥对时可修改秘钥对的存储位置，默认在`[用户目录]/.ssh`目录中，这个地址可以自己修改，中途会提示是否设置密码，建议设置。

最终会生成两个秘钥文件，分别是公钥和私钥文件。
举例，如果用户目录为`/Users/user1`，设置的秘钥文件名称为`my_github`，则最终生成文件为：
公钥文件：`/Users/user1/.ssh//my_github.pub`，后续需要填入到github配置文件中。
私钥文件：`/Users/user1/.ssh//my_github`，自己保留备份。

#### 将ssh秘钥对加入本地钥匙链

执行`ssh-add -K /Users/user1/.ssh//my_github`，将秘钥加入钥匙链。



#### 配置本地ssh
编辑`~/.ssh/config`文件，指定当访问github.com时使用本地指定的私钥。
```
vim ~/.ssh/config
# 在文件中加入以下配置

Host *.github.com
  AddKeysToAgent yes
  UseKeychain yes
  IdentityFile ~/.ssh/github
```
> 如果未设置密码，则去除`UseKeychain`配置

如果报`line 16: Bad configuration option: usekeychain`，则用以下配置：
```
Host *.github.com
  AddKeysToAgent yes
  UseKeychain yes
  IdentityFile ~/.ssh/github
  IgnoreUnknown UseKeychain
```

[Github帮助文档](https://docs.github.com/cn/authentication/connecting-to-github-with-ssh)


## 多git账号SSH配置
多账号使用时，在配置文件中为特定的域名添加特定的配置即可。
以下示例：
```
# ~/.ssh/config的配置
# github的配置
Host *.github.com
  AddKeysToAgent yes
  UseKeychain yes
  IdentityFile ~/.ssh/github

# aliyun的配置
Host *.aliyun.com
  AddKeysToAgent yes
  UseKeychain yes
  IdentityFile ~/.ssh/aliyun

```