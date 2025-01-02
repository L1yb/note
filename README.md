管理实验室电脑和宿舍笔记本的笔记同步

## 常用命令

git add .

git commit -m " "

git push -u origin main

查看远程库
git remote -v

添加远程

git remote rm [name]

获取远程库
git fetch [远程库别名]   

将branch合并到当前分支
git merge [alias]/[branch]  

切换/创建分支
git checkout [分支] 

查看分支列表
git branck

## 常见问题

1. Changes not staged for commit:

可能的原因有两个

- 修改之后没有进行add操作将其添加到暂存区
  `git add filename`

- add添加到暂存区之后，在commit提交之前，又进行了修改

2. git pull 失败的原因是，远程库和本地库不一致，比如分支结构变化，所以先fetch

3. fatal: unable to acces

- 编辑代理服务器，设置代理ip 127.0.0.1   port 7890

[完美解决 git 报错 “fatal: unable to access ‘https://github.com/.../.git‘: Recv failure Connection was rese-CSDN博客](https://blog.csdn.net/qq_43546721/article/details/139506583)

- 设置git使用本地代理

```bash
git config --global http.proxy http://127.0.0.1:7890
```

- 检查是否设置成功

```bash
git config --global -l
```

4. error：本地未保存

   ```bash 
   git stash //备份到本地git栈
   git pull
   git stash pop //从git栈获取最近一次stash的内容，回复工作区的内容，删除栈中对应的stash
   ```

   具体使用方法和使用场景：

   假如代码在commit 之后做了修改，或者理解为新写的代码还没来得及（没必要）commit，假如叫**状态1**，但是这时候需要pull代码进行修改：
   这时候git stash 会保存当前还没commit的状态（正在写的**状态1**），然后pull，会替换当前代码，叫**状态2**，pull下来的代码完成以后，想要回复刚才的**状态1**，就使用git stash pop，这时候就会恢复之前**状态1**，并且删除git本地栈暂存的**状态1**
