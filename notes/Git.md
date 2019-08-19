# Git

###### Git 对象模型

SHA：索引

对象：类型（blob、tree、commit、tag）、大小、内容

blob：存储文件内容 `git show SHA1`

tree：

commit：

tag：



设置用户名和邮箱作为标识

```bash
git config --global user.name "visitor"
git congif --global user.email "visitor@126.com"
```

创建仓库

```bash
git init
```

删除仓库

```bash
rm -rf .git
```

添加文件

```bash
git add readme.md
```

删除文件

```bash
rm readme.md
# git rm * 将删除所有文件
git rm readme.md
```

查看仓库状态

```bash
git status
```

提交更改到仓库

```bash
git commit -m "提交readme.md文件"
```

查看文件修改内容

```bash
git diff readme.md
```

查看历史记录

```bash
git log
git log --pretty=online
```

版本回退

```bash
git reset --hard HEAD^
git reset --hard HEAD^^
git reset --hard HEAD~100
```

查看文件内容

```bash
cat readme.md
```

获取版本号

```bash
git reflog
```

回退到最新版本

```hash
git reset --hard 版本号
```

丢弃工作区修改

```bash
git checkout -- readme.md
```

删除文件

```bash
rm readme.md
```

创建SSH key

```bash
ssh-keygen -t rsa -C "visitor@126.com"
```

关联 github 远程仓库

```bash
git remote add origin https://github.com/xxx/xrepository
```

推送本地内容到远程

```bash
git push -u origin master
```

克隆远程仓库

```bash
git clone https://github.com/xxx/xrepository
```

