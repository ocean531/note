```
git init //初始化本地仓库

git add //将代码上传到暂存区，此机制便于多次添加 之后一次性上传到仓库中

git commit -m “描述” //将暂存区的代码上传到仓库中 ， 后面带上描述 

```
## 将代码上传到github

```
ssh-keygen -t rsa -b 4096 -C "你的邮箱" //生成ssh密钥
```

然后在github中添加shh密钥
```
git remote set-url origin git@github.com:你的用户名/你的仓库名.git //连接远程仓库
git push //上传代码
```
