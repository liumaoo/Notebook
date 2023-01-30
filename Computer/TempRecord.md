## 临时操作记录
### Github操作记录
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