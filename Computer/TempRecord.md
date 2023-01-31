## 临时操作记录
### Github操作记录

在GitHub中建立新的仓库后拉取到本地：1.先建立本地仓库，再连接远程仓库，最后拉取分支 2.直接从远程仓库克隆到本地

```c++
1.git init //初始化一个本地git版本库
2.git remote add origin git@github.com:username/repository.git  //建立和连接一个远程仓库
//origin 即为远程仓库 git@github.com:username/repository.git 的别名
3.git pull origin main //将远程仓库的main分支拉取合并到本地仓库
//git pull <远程主机名> <远程分支名>:<本地分支名>

git clone git@github.com:username/repository.git //将远程仓库克隆到本地
```

向Github远程仓库中提交更新：

```c++
1.git add . //将所有更新的文件添加到暂存区
2.git commit -m "提交信息"
3.git push -u origin main
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