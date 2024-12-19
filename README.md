管理实验室电脑和宿舍笔记本的笔记同步

## 常用命令

git add .

git commit -m " "

查看远程库
git remote -v

获取远程库
git fetch [远程库别名]   

将branch合并到当前分支
git merge [alias]/[branch]  

切换/创建分支
git checkout [分支] 

查看分支列表
git branck

## 常见问题

1、Changes not staged for commit:

可能的原因有两个

- 修改之后没有进行add操作将其添加到暂存区
  `git add filename`

- add添加到暂存区之后，在commit提交之前，又进行了修改

2、git pull 失败的原因是，远程库和本地库不一致，比如分支结构变化，所以先fetch
