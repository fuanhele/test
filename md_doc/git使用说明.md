


# git的基本使用方法

1. 初始化git仓库: `git init`
   
2. 在git仓库中添加文件: `git add readme.txt andme.md` (可以提交多个文件)
    (相当于提交到暂存区,暂存区里面存放的只是add命令当时的修改部分) 
    `git add --all` 一次提交所有文件

3. 提交文件: `git commit -m "add 2 files"` (一次性把暂存区的所有修改提交到分支)
   
4. 查看当前仓库下文件的状态: `git status` 可以看哪个文件被修改但是没有提交
    untracked 表示文件没有使用add添加过

5. 查看当前文件和之前的版本有哪些不同之处: `git diff readme.txt`
   
6. 查看都提交了哪些版本: `git log`,显示从近到远的提交日志，或者使用`git log --pretty=oneline`查看精简输出
   
7. 回到某个版本: `git reset --hard HEAD^` HEAD^表示上一个版本，HEAD^^表示上上个版本，HEAD~100表示上100个版本

8. 当从第三个版本回到第二个版本时，再想回到第三个版本时，就需要使用第三个版本号，然而如果在第二个版本状态下
    使用git log命令时，不会显示第三个版本的版本号，所以需要使用`git reflog`来查看每一次操作的记录信息
    然后使用`git reset --hard 版本号前几位`   推荐经常使用`git log`命令，并且不要关闭git窗口

9.  如果放弃对当前版本的修改，想回到上一次add或者commit的状态，可以使用`git checkout --readme.txt`
    
10. 把暂存区的文件撤销，放回到工作区 `git reset HEAD readme.txt` HEAD表示最新的版本

11. 删除文件，使用`rm readme.txt`删除工作区的文件，再使用`git rm readme.txt`删除版本库中的文件，
    然后用`git commit -m "delete readme.txt"`

12. 如果是在工作区不小心删错了，可以使用`git checkout --readme.txt`将文件从版本库中恢复到工作区


13. 使用`git remote add origin git@github.com:lly327/SRNet-datagen.git` 将远程库和本地库关联起来
    origin就是远程库的名字

14. 第一次使用`git push -u origin master` 将本地master分支推送到远程origin分支上，并且关联起来
    以后使用`git push origin master` 就可以了

15. 查看远程库信息: `git remote -v`  删除远程库: `git remote rm origin` 
    此处的“删除”其实是解除了本地和远程的绑定关系，并不是物理上删除了远程库。远程库本身并没有任何改动。
    要真正删除远程库，需要登录到GitHub，在后台页面找到删除按钮再删除。

16. 从远程库下载: `git clone git@github.com:lly327/SRNet-datagen.git`  使用ssh下载比使用https下载速度快，并且还方便


## 分支的作用

分支在实际中有什么用呢？假设你准备开发一个新功能，但是需要两周才能完成，第一周你写了50%的代码，如果立刻提交，由于代码还没写完，不完整的代码库会导致别人不能干活了。如果等代码全部写完再一次提交，又存在丢失每天进度的巨大风险。

现在有了分支，就不用怕了。你创建了一个属于你自己的分支，别人看不到，还继续在原来的分支上正常工作，而你在自己的分支上干活，想提交就提交，直到开发完毕后，再一次性合并到原来的分支上，这样，既安全，又不影响别人工作。

有关分支的具体工作流程: https://www.liaoxuefeng.com/wiki/896043488029600/900003767775424

17. 创建并且切换到一个新分支上: `git checkout -b dev` -b表示创建并且切换
    相当于执行两个命令 `git branch dev` + `git checkout dev`
    注意：推荐使用以下命令，更容易理解记忆 `git switch -c dev` `git switch dev`

18. 使用`git branch`查看分支，当前分支前会有一个星号。
19. 使用`git merge dev` 将dev分支合并到当前分支。
20. 删除分支`git branch -d dev` 
21. 合并分支时，加上--no-ff参数就可以用普通模式合并，合并后的历史有分支，能看出来曾经做过合并，
    而fast forward合并就看不出来曾经做过合并
    `git merge --no-ff -m "merge with no-ff" dev`



