# Toolbox
## mkdocs
[mkdocs instructions](https://squidfunk.github.io/mkdocs-material/){ .md-button }
## shell
[shell 编程](https://www.shellscript.sh/){ .md-button }

### shell 编程要点
<div class="grid cards" markdown>

-   :toolbox:{ .lg .middle } __variables__

    ---
    * 变量都以字符串形式储存。hello world是两个参数，“hello world”是一个参数。
    * MY_MESSAGE=Hello  中间不可加空格，否则认为是命令
    * 访问值"\${MY_MESSAGE}"eg:  "\${USER_NAME}_file"当然{}在变量单独出现时可消去，即$MY_MESSAGE也可以访问值
    * Scope:运行脚本产生新的shell，破坏interactive shell，interactive shell中的变量赋值在新shell中不可见。除非：
        
        1.在interactive shell中使用export MY_MESSAGE=Hello  
        
        2.在interface中运行脚本。命令为："." eg: . ./script.sh




</div>

1.shell中导航

* 如果某个路径以 / 开头，那么它是一个 **绝对路径**，其他的都是 **相对路径** 。

* . 表示的是 **当前目录** ，而 .. 表示 **上级目录**

2.在程序间创建连接

* 重定向输入流、输出流

    {==< file==}:重定向输出流 eg:echo hello > hello.txt

    {==> file==}:重定向输入流

* 管道

    | 操作符允许我们将一个程序的输出和另外一个程序的输入连接起来

3.Shell函数

:arrow_right: [shell函数菜鸟教程](https://www.runoob.com/linux/linux-shell-func.html)

4.Command

:arrow_right: [Linux命令大全](https://www.runoob.com/linux/linux-command-manual.html)

:arrow_right: [tldr简明版man](https://tldr.sh/)

:arrow_right: 用man程序： man + 程序名 展示用户手册

* 标记和选项：- 开头，并可以改变程序的行为。eg: 在执行程序时--help 可以打印帮助信息 

??? note "Important Command"
    * {==sudo==}:以 su（super user 或 root 的简写）的身份执行一些操作
    
    ??? warning "Warning: shell权限始终为当前用户"
        sudo echo 3 > brightness 报错

        原因：关于 shell，有件事我们必须要知道。|、>、和 < 是通过 shell 执行的，而不是被各个程序单独执行。 echo 等程序并不知道 | 的存在，它们只知道从自己的输入输出流中进行读写。 对于上面这种情况， shell (权限为您的当前用户) 在设置 sudo echo 前尝试打开 brightness 文件并写入，但是系统拒绝了 shell 的操作因为此时 shell 不是根用户。应改为echo 3 | sudo tee brightness

    * {==mkdir==}:生成文件夹
    * {==cat==}:打印粘贴文件内容
    * {==grep==}:抓取文件中某行并输出
    * {==mv==}:用于重命名或移动文件
        * mv source_file(文件) dest_file(文件):将源文件名 source_file 改为目标文件名 dest_file
        * mv source_file(文件) dest_directory(目录):dest_directory(目录)将文件 source_file 移动到目标目录 dest_directory 中
        * mv source_directory(目录) dest_directory(目录):目录名 dest_directory 已存在，将 source_directory 移动到目录名 dest_directory 中；目录名 dest_directory 不存在则 source_directory 改名为目录名 dest_directory
    * {==cp==}:用于复制文件或目录.cp [选项] 源文件 目标文件
    * {==ls==}:打印当前目录下文件
        * missing:~$ ls -l /home ,输出drwxr-xr-x 1 missing  users  4096 Jun 15  2019 missing。首先，本行第一个字符 d 表示 missing 是一个目录。然后接下来的九个字符，每三个字符构成一组。 （rwx）. 它们分别代表了文件所有者（missing），用户组（users） 以及其他所有人具有的权限。其中 - 表示该用户不具备相应的权限。r:read w:write x:可执行
    * {==read==}:标准输入
    * {==echo==}:echo命令用于在shell中打印shell变量的值或直接输出指定的字符串。echo命令的基本用法是在命令后面跟上要输出的文本。
        * 只输出字符串可不加“" eg:echo This is a test
        * -n表示不换行输出。默认自动换行
        * 换行输出：echo -e "OK! \n"；不换行输出：echo -e "OK! \c" 
        * 用于显示变量的值，例如：name="John" echo "My name is $name"。
        * 输出转义字符，如echo \"Hello,World!\";echo -e:开始转义
        * 输出特殊字符，如echo "The price is $100"
        * 结合管道和重定向操作符一起使用，以将输出传递给其他命令或文件。
        * 原样输出字符串，不进行转义或取变量(用单引号)。eg: echo '$name\"'
        * 显示命令执行结果 echo `date` 注意： 这里使用的是反引号 `, 而不是单引号 '
    * {==which==}:查找绝对路径。eg:which echo
    * {==pwd==}:获取当前工作目录绝对路径
    
## markdown
### syntax
* [icons,Emojis](https://squidfunk.github.io/mkdocs-material/reference/icons-emojis/)
* [latex](https://latex-tutorial.com/tutorials/amsmath/)
* [CSS classes](*)
### mermaid
* [mkdocs supported](https://squidfunk.github.io/mkdocs-material/reference/diagrams/)
* others
## environment
### WSL
* [WSL Documentation](https://learn.microsoft.com/en-us/windows/wsl/)