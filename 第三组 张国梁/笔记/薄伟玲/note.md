## MAC安装
安装homebrew
https://brew.sh/index_zh-cn.html
```
brew install git
```

> 安装过xcode会自动安装git

## 使用git
- 告诉git你是谁？
```
git config --global user.name zhufengzhufeng
git cinfig --global user.email 894918097@qq.com
```

> 只需设置一次(没有设置则不能提交)

## 查看配置
```
git config --list
```



## 创建文件
```
mkdir(make directory) 文件名 -p可以循环创建
```

## 删除目录
```
rm -rf 文件夹
```

## 查看目录下所有文件
```
ls -al
```

## 版本控制(文件)
- 初始化使用git的位置
```
git init (初始化)
```

## vi编辑
```
vi 1.txt
i 插入  变成编辑模式 
esc + :wq(保存+退出) 退出  命令模式
esc + :q!(强制退出)
```

## 查看文件内容
```
cat 文件名
```

## 将文件进行提交
- 将文件加入到暂存区中(不能直接提交到历史区)
```
git add .或者-A或者<文件名>
```

## 提交到历史
```
git commit -m ''
```

> 必须要备注提交参数

## 查看提交历史
```
git log(日志)
```

## 代码比较
- 工作区和暂存区
```
git diff (不同different)
```
- 工作区和历史区
```
git diff master
```
- 暂存区和历史区
```
git diff --cached(暂存区)
```

## checkout
- 从暂存区拉回工作区
```
git checkout 文件名
```

## 取消本次add
- 将暂存区中的内容回到上次add的情况
```
git reset HEAD '文件名'
``` 

## 回到过去&&回到未来
- 回到过去
```
git log (获取commit_id)
git reflog (记录着所有版本)
git reset --hard  commit_id
```

> log只能查看当前版本之前的内容
# 分支
- 主干，通过主干建立一个分支，当代码完成后确认无误后在合并到主干上

## 创建分支
- 会在当前状态下克隆一份一模一样的
```
git branch dev
```

> 查看分支git branch

## 切换分支
```
git checkout dev
```

## 删除分支
```
git branch -D dev
```

## 创建分支并切换
```
git checkout -b(分支) dev
```

> 要将内容提交到dev分支上，才会归dev管理，否则只是工作区的内容

## 合并分支
```
git merge dev
```

> 在master上合并dev分支
 origin
## 创建readMe文件(可以不创建)
```
echo '# welcome ' >> README.md
```

## 添加忽略文件
```
touch .gitignore
.DS_Store
.idea
```

## 添加远程仓库
```
git remote add origin https://github.com/zhufengzhufeng/201614nodetest.git

```

## 删除远程仓库
```
git remote rm 仓库名
``` 

## 推送到远程仓库
```
git push origin master -u(upstream)
```
###部署静态的github页面

- 先建一个仓库 static
- git init (初始化本地仓库)
- git checkout -b gh-pages (分支名不能更改)
- git add . (添加到暂存区)
- git commit -m”trans” (提交到历史区)
- git remote add origin 地址
- git push origin gh-pages

###合作 老师 组长 组员
   
-    组长fork老师的项目
-    将代码下载到本地
-    git clone 地址 文件夹的名字
-    组长给组员开通权限
   
-    自己的仓库下点击settings > Collaborators  
###添加用户
-    开通权限后组员操作
   
-    组员将代码克隆下来
-    找到自己组放自己的文件夹
-    git add .
-    git commit -m ‘xxx作业ok’
-    git pull origin master(拉取组长仓库最新代码，防止提交后覆盖)
-    git push origin master