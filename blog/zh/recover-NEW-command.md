@def title = "恢复\"新建\"命令"
@def published = "10 January 2019"
@def tags = ["Tips", "Windows"]

~~~
<ul>
{{for tag in tags}}
  &#x1F516;<a href="/tag/{{fill tag}}" style="text-transform: uppercase;color:#696969">{{fill tag}}</a>
{{end}}
</ul>
~~~

> 有的时候我们会发现自己常用的“新建”文件类型丢失了，这时候我们可以修改注册表来添加找回。

**假设丢失的是新建`.txt` 文件类型**
1. 用记事本新建一个文本文件，命名为“ADDTXT.REG”
2. 输入以下代码：
`Windows Registry Editor Version 5.00`
`[HKEY_CLASSES_ROOT\.TXT\ShellNew]"NullFile"=""`
3. 保存该文件并执行。

**REFERENCE： **
电脑爱好者2018-12
