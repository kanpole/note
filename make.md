# GCC工具链与MAKE的使用


## GCC


### GCC编译步骤

    
    + gcc -o  #编译成的目标
    + gcc -E  #预编译 .i
    + gcc -S  #汇编  .s
    + gcc -c  #生成编译文件，但不链接 .o
    
    ```shell
    
        常用参数：
        + -Wl   #传递给连接器的参数，用逗号隔开
        + -Wall #启用严格警告
        + -I    #include路径
        + -L    #库路径
        + -l    #链接的库
        + -g    #添加调试信息
        + -O2   #优化级别 
        + -static    #链接时使用静态链接库
        + -fPic      #生成位置无关代码，一边个人库不会指定库加载的位置，但是某些系统级的库需要，比如中断函数要在某个固定位置
        + -shared    #链接时使用动态库
        + -rpath     #指定运行时动态库的查找路径
        + -rpath-link  #指定链接时间接库的查找路径
        
    ```
    
    + link             #链接器，链接目标
    + ar               #打包静态库
    > ar rcs           #打包库文件
    
    + ranlib           #重建索引，相当于ar -s
    + nm               #库信息查看工具
    + objdump          #反汇编产看工具
    + size             #头查看工具
    + string           #数据查看工具
    + ldd              #链接查看工具
    + linux命令 ulimit  #限制资源
### GBD调试

    ```c
        file         #加载程序
        list         #查看源码
        display      #查看汇编
        break        #下断点
        break if     #条件断点
        info         #查看断点
        watch expr   #变量发生改变时中断
        delete       #删除断点
        continue     #继续执行到下个断点
        step         #步入，进入函数
        next         #步过，不进函数内部
        finish       #运行到当前函数返回，并打印函数堆栈等信息
        until        #运行到循环结束
        print        #查看变量
        ptype        #查看类型
        print array  #查看数组
        print *array@len  #查看动态内存
        print x=5         #修改运行时数据
    
    ```

