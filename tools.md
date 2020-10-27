

# study-notes

**1.nvm：node版本管理工具，适用于多node版本项目切换时使用**

nvm ls 查看 node版本列表

nvm install 12.11.0 安装node版本

nvm use 12.11.0 切换node版本  

**2.kindEditor:是一套开源的在线HTML编辑器，主要用于让用户在网站上获得所见即所得编辑效果，开发人员可以用KindEditor 把传统的多行文本输入框(textarea)替换为可视化的富文本输入框**

首先在官网上下载最新的KindEditor文件,把整个文件夹放到你的web服务器里（放哪都行，但放的位置需要有访问权限，最好建立一个权限为777的public文件夹，把KindEditor文件夹放进去）。

之后再在这个页面写一个js脚本，注意：这个脚本的作用就是控制该页面的KindEditor编辑器，js脚本如下

```
<script type="text/javascript">
    //KindEditor脚本
    var editor;
    KindEditor.ready(function(K) {
        editor = K.create('#你的textarea的id名', {
        });
    });
</script>
```

这个脚本里的 editor = K.create('#你的textarea的id名', {}); 这个，对应的就是一个KindEditor编辑器

例子：

假设我们现在有一个textarea标签，id设为"kindEditor_demo"

```
<textarea id="kindEditor_demo"></textarea>
```

我们再引入刚刚提到的两个js文件后，再写如下js代码到该页面

<script type="text/javascript">
    //KindEditor脚本
    var editor;
    KindEditor.ready(function(K) {
        editor = K.create('#kindEditor_demo', {
        });
    });
</script>

那么这个textarea标签就会变成KindEditor编译器