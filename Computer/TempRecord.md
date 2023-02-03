## 临时操作记录
### Github操作记录

##### 生成 ssh key

```c++
1.ssh-keygen -t rsa -C "youremail@example.com" //生成新的rsa密钥
2.ssh-add ~/.ssh/id_rsa //将产生的新ssh key添加到ssh-agent中
3.将SSH key 添加到GitHub账户 //在账户选项中选择 “Settings”–>“SSH and GPG keys”–>“New SSH key”，
//然后打开之前新生成的id_rsa.pub文件，一般在C盘用户名下的.ssh文件夹下，将密钥复制后填写到账户中
4.ssh -T git@github.com //验证ssh key是否配置成功
```

##### 代码拉取

在GitHub中建立新的仓库后拉取到本地：1.先建立本地仓库，再连接远程仓库，最后拉取分支 2.直接从远程仓库克隆到本地

```c++
1.git init //初始化一个本地git版本库
2.git branch -M main //-M 对本地分支重命名，本地创建默认是master，应改为main
3.git remote add origin git@github.com:username/repository.git  //建立和连接一个远程仓库
//origin 即为远程仓库 git@github.com:username/repository.git 的别名
4.git pull origin main //将远程仓库的main分支拉取合并到本地仓库
//git pull <远程主机名> <远程分支名>:<本地分支名>

git clone git@github.com:username/repository.git //将远程仓库克隆到本地
```

##### 代码提交

向Github远程仓库中提交更新：

```c++
1.git add . //将所有更新的文件添加到暂存区
2.git commit -m "提交信息日志" //将本地修改过的文件提交到本地仓库中
3.git push origin main  //将本地仓库中的最新信息发送给远程仓库
```

##### 分支操作

Github创建仓库后默认分支是main，而本地创建是master。提交后发现Github仓库出现master分支，解决步骤如下：

```c++
1.git branch -M main //-M 对本地分支重命名
2.git branch -a //查看当前github仓库的所有分支
3.git push origin --delete master //删除远程分支master
4.git checkout main //切换到当前分支main，也就要保留下来的分支
5.git merge remotes/origin/main //合并分支
6.git push origin main //提交修改
```

### SVN操作记录

##### 查看最新的仓库版本号

```c++
svn info -r HEAD  //可查看最新的仓库信息，或在提交log中查看当前或以往的提交记录和版本号
```

### Typora操作记录

##### 主题中.css文件中的一些设置
```css
#write .CodeMirror-wrap .CodeMirror-code pre {
    white-space: nowrap;        /*设置代码不自动换行*/
    overflow-y: hidden;         /*设置纵向的滑动框隐藏*/
    overflow-x: scroll;         /*设置横向的滑动框显示*/
}
#write .CodeMirror-code {
    background-color: #cfffde;  /*设置代码块背景颜色*/
}
.CodeMirror-scroll::-webkit-scrollbar {
    display: block;             /*设置代码块是否显示滑动框，block为默认，none为不显示*/
    background-color:#606366;   /*设置滑动框的背景颜色*/
}
.CodeMirror-scroll::-webkit-scrollbar-thumb {
    background: #909090;  /*设置滑动框的本身颜色*/
}
```
##### 快捷键操作

### 细碎知识点记录

LF与CRLF：不同操作系统下的默认换行符，LF为Unix/Linux/Mac OS X下的换行符，转义字符为 \n。CRLF为Windows下的换行符，转义字符为 \r\n。
