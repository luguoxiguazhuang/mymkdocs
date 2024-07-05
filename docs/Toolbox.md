# Toolbox
## mkdocs
[mkdocs instructions](https://squidfunk.github.io/mkdocs-material/){ .md-button }
## shell
[shell 编程](https://www.shellscript.sh/){ .md-button }

### shell 编程要点

??? note "variables"

    * 变量都以字符串形式储存。hello world是两个参数，“hello world”是一个参数。
    * MY_MESSAGE=Hello  中间不可加空格，否则认为是命令
    * 访问值"\${MY_MESSAGE}"eg:  "\${USER_NAME}_file"当然{}在变量单独出现时可消去，即$MY_MESSAGE也可以访问值
    * Scope:运行脚本产生新的shell，破坏interactive shell，interactive shell中的变量赋值在新shell中不可见。除非：
        
        1.在interactive shell中使用export MY_MESSAGE=Hello  
        
        2.在interface中运行脚本。命令为："." eg: . ./script.sh

??? note "Escape Characters"

    * ""内$ \ 需要被转义，转义字符前面加\
    *  \* 表示全部文件 *txt 表示所有以txt结尾的文件

??? note "Loops"

    * for loop
    
    
    ``` sh
        #!/bin/sh
        
        for i in hello 1 * goodbye 
        
        do
            
            echo "Looping ... i 
            
            is set to $i"
        
        done
    ```
    in 后面可跟多个不同参数

    * while loop

    ``` sh
        #!/bin/sh
        INPUT_STRING=hello
        while [ "$INPUT_STRING" != "bye" ]
        do
        echo "Please type something in (bye to quit)"
        read INPUT_STRING
        echo "You typed: $INPUT_STRING"
        done
    ```
    
    ```while : ```中：即true

    ``` sh title="while中case语句"
    #!/bin/sh
    # 逐行读入myfile.txt文件
    while read input_text
    do
    case $input_text in
            hello)          echo English    ;;
            howdy)          echo American   ;;
            gday)           echo Australian ;;
            bonjour)        echo French     ;;
            "guten tag")    echo German     ;;
            *)              echo 
            #* 代表其余情况default
            Unknown Language: $input_text
                    ;;
    esac
    done < myfile.txt
    ```

??? note "Test"

    * ``` if SPACE [ SPACE "$foo" SPACE = SPACE "bar" SPACE ] ``` 注意[ SPACE something SPACE] is a test.

    * format:
        
    ``` sh title="if statement"
        if [ ... ]
        then
        \# if-code
        else
        \# else-code
        fi
    ```

    ``` sh title="if statement"
        if [ ... ]; then
        # do something
            elif [ ... ]; then
            # do something
            else
            # do something
        fi
    ```

    actually,[] is a test. ;连接两个命令在同一行

    ``` sh title="&& || can write if statement"
        [ $X -ne 0 ] && echo "X isn't zero" || echo "X is zero"

    ```

    * check the contents of the variable before you test it

    ``` sh
    echo -en "Please guess the magic number: "
    read X
    echo $X | grep "[^0-9]" > /dev/null 2>&1
    # grep [0-9] finds lines of text which contain digits (0-9) and possibly other characters
    # (^) in grep [^0-9] finds only those lines which don't consist only of numbers
    if [ "$?" -eq "0" ]; then
    # If the grep found something other than 0-9
    # then it's not an integer.
    # The >/dev/null 2>&1 directs any output or errors to the special "null" device
    echo "Sorry, wanted a number"
    else
        if [ "$X" -eq "7" ]; then
            echo "You entered the magic number!"
        fi
    fi
    ```

    * for strings' comparison: =

    * for integers' comparison: -eq(equal) -gt(greater than) -ge(greater than or equal) -lt(less than) -le(less than or equal) -ne is'nt 0 eg: ``` [ "$a" -eq "2" ]``` 

    * 
    ``` sh
        [ -n "$X" ] && \
            echo "X is of nonzero length"
        [ -f "$X" ] && \
            echo "X is the path of a real file" || \
            echo "No such file: $X"
        [ -x "$X" ] && \
            echo "X is the path of an executable file"
        [ "$X" -nt "/etc/passwd" ] && \
            echo "X is a file which is newer than /etc/passwd"

    ```

    * ; join two lines together

    * \ tells the shell that this is not the end of the line, but that the following line should be treated as part of the current line. 

??? note "Case"

    ``` sh title="case statement"
        case $INPUT_STRING in
            hello)
                echo "Hello yourself!"
                ;;
            bye)
                echo "See you again!"
                break
                ;;
            *) # default
                echo "Sorry, I don't understand"
                ;;
        esac
    ```

??? note "Variables -Part2&3"
    
    * $0 is the basename of the program as it was called

    * $1 .. $9 are the first 9 additional parameters the script was called with.The variable $@ is all parameters $1 .. whatever. The variable $*, is similar,but does not preserve any whitespace, and quoting, so "File with spaces" becomes "File" "with" "spaces". 

    * $# is the number of parameters the script was called with.

    * shift:常用于参数数量未知情况的处理，在 shift 命令执行前变量 $1 的值在 shift 命令执行后就不可用了。shift 实行参数左移。
    
    ``` sh
        #!/bin/sh
        while [ "$#" -gt "0" ]
        do
        echo "\$1 is $1"
        shift
        done  
    ```

    * $? is the exit status of the last command executed.

    * $$:The $$ variable is the PID (Process IDentifier) of the currently running shell. This can be useful for creating temporary files, such as /tmp/my-script.$$ which is useful if many instances of the script could be run at the same time, and they all need their own temporary files.

    * $!: The $! variable is the PID of the last run background process.

    * IFS:the Internal Field Separator.Default value is: IFS=SPACE TAB NEWLINE.But we could change.

    ``` sh
    #!/bin/sh
    old_IFS="$IFS"
    IFS=:
    echo "Please input some data separated by colons ..."
    read x y z
    IFS=$old_IFS
    echo "x is $x y is $y z is $z"
    ```

    * Using Default Values

    ``` sh
    echo "Your name is : ${myname:=John Doe}"#若有输入打印输入值，若无输入，打印默认值John Joe
    ```

??? note "External Programs"
    
    External Programs:用``` `cmd` ```来执行命令 
    
    eg:``` MYNAME=`grep "^${USER}:" /etc/passwd | cut -d: -f5`  ```

??? note "Function"
    * eg:
    ``` sh
    add_a_user()
    {
    USER=$1
    PASSWORD=$2
    shift; shift;
    # Having shifted twice, the rest is now comments ...
    COMMENTS=$@
    echo "Adding user $USER ..."
    echo useradd -c "$COMMENTS" $USER
    echo passwd $USER $PASSWORD
    echo "Added user $USER ($COMMENTS) with pass $PASSWORD"
    }

    ###
    # Main body of script starts here
    ###
    echo "Start of script..."
    add_a_user bob letmein Bob Holness the presenter
    add_a_user fred badpassword Fred Durst the singer
    add_a_user bilko worsepassword Sgt. Bilko the role model
    echo "End of script..."
    ``` 

    * Scope of Variables
        
        * can change a global variable in the function.

        * A function will be called in a sub-shell if its output is piped somewhere else.When using a pipe,global variables will not be changed in the function.,for it's in a sub-shell that will soon be destroyed.
    
    * common.lib:we can use libraries to share function between scripts.
    使用前加上``` . . /common.lib ``` 即可调用函数。


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

:arrow_right: [易忘符号](https://www.shellscript.sh/quickref.html)

??? note "Important Command"
    * {==sudo==}:以 su（super user 或 root 的简写）的身份执行一些操作
    
    ??? warning "Warning: shell权限始终为当前用户"
        sudo echo 3 > brightness 报错

        原因：关于 shell，有件事我们必须要知道。|、>、和 < 是通过 shell 执行的，而不是被各个程序单独执行。 echo 等程序并不知道 | 的存在，它们只知道从自己的输入输出流中进行读写。 对于上面这种情况， shell (权限为您的当前用户) 在设置 sudo echo 前尝试打开 brightness 文件并写入，但是系统拒绝了 shell 的操作因为此时 shell 不是根用户。应改为echo 3 | sudo tee brightness

    * {==mkdir==}:生成文件夹
    * {==cat==}:打印粘贴文件内容
    * {==cut==}:剪取某个域（字符，自定义域……）
    * {==tr==}:transition ``` tr '[a-z]' '[A-Z]' ```
    * {==awk==}:```awk '{print $1 }' file.txt```:打印第一个参数，类似scanf,忽略空格，将被空格隔开的参数分别记为\$1,\$2,\$3……
    * {==sed==}:替换功能，若替换为""即为删除功能，sed 's/old/new/g' file.txt:将文件中所有old替换为new，与grep不同的是grep处理的是整行。
    * {==grep==}:抓取文件中 **某行** 并输出
    * {==mv==}:用于重命名或移动文件
        * mv source_file(文件) dest_file(文件):将源文件名 source_file 改为目标文件名 dest_file
        * mv source_file(文件) dest_directory(目录):dest_directory(目录)将文件 source_file 移动到目标目录 dest_directory 中
        * mv source_directory(目录) dest_directory(目录):目录名 dest_directory 已存在，将 source_directory 移动到目录名 dest_directory 中；目录名 dest_directory 不存在则 source_directory 改名为目录名 dest_directory
    * {==cp==}:用于复制文件或目录，，cp [选项] 源文件 目标文件
    \$可以表示所有文件
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