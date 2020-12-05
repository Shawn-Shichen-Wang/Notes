- 浏览
  - `echo 'Hello, shello!'`
  - `echo $COLUMNS x $LINES`
  - `ls`浏览 
    - 后边可加目录名称
  - `cd`更改目录
  - ".." 父目录
  - "~" 主目录
  - ";"可以一行写多个命令
  - `pwd` 打印当前目录(Print Working Directory)

  - UNIX使用"/"正斜杠分隔目录名称
  - `ls *.pdf` 通配符



- 整理

  - `mkdir` 创建目录(Make Directory)
  - `mv 'Documents/xxx.pdf' Documents/Books/` 移动文件
    - 单引号表示特殊字符被视为非特殊字符(即转义)
    - `“弯引号”`。弯引号在 shell 中**不可行**
    - 单引号和双引号的作用不太一样。如果不确定使用哪种引号，请选择单引号。

- 下载

  - `curl` C URL $\Rightarrow$ see URL
  - `curl -L -o new_name 'https://www.douban.com'`
    - 不加'单引号也行，但是网址中经常会有特殊字符，为了防止bug产生养成良好的代码习惯加上单引号

- cat $\Rightarrow$ Catenate or Concatenate

  -  less 显示第一页内容
    - 空格或者方向键浏览
    - B 返回
    - / 搜索

- 删除

  - rm $\Rightarrow$ remove
  - `rm -i ValuableFile.txt` i 表示interactive 交互 
  - rmdir 删除目录

- 搜索 grep

  - `grep shell dictionary.txt | less`

    - '|' 管道命令
    - 要求Shell将grep的输出发送给less命令

  - `curl -L 'https://www.douban.com' | grep '豆瓣'`

  - `curl -L 'https://www.douban.com' | grep '豆瓣'| wc -l`
    - wc (word count);  -l 计算行数

  - curl -L 'https://www.douban.com' | grep -c '豆瓣'`

      - -c count

- 变量

  - `number='one two three'`
  - 等号两边不要加空格
  - 引用时前边加`$`
    
    - $number
  - 分为两种

    1. shell变量，shell程序本身的内部变量🌰： \$LINES * \$COLUMNS

    2. 环境变量，是与在Shell中运行的程序共享的变量

       🌰：\$PATH 

- bin 代表二进制文件

- 起始文件 .bash_profile

  - 包含Shell命令的文件叫做Shell脚本

  - Linux系统中非登陆Shell会话会加载`.bashrc`文件

    - 解决办法：在`.bash_profile中加入下列语句`

      - ```bash
        if [-f ~/.bashrc ] ; then
        	source ~/.bashrc
        fi
        ```

      - 如果找到.bashrc文件则运行它

  - Mac
  
- `alias ll='ls -la'` 别名

  - alias 查看所有别名
