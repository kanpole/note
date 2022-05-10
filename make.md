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
        + -m           #编译的程序模式 32or64
    ```
    
    + ld             #链接器，链接目标
    
    ```
        手动编译一次动态库，假设有一个one.c库
        gcc -E one.c -o one.i
        gcc -S -m64 -fpic -shared one.i -o one.s
        gcc -c -m64 -fpic -shared one.s -o one.o
        ld  -melf_x86_64 -fpic -shared one.o -o one.so
    
    
    
    
    ```
    
    
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
+        file         #加载程序
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


## Make规则

  例子
  ```
  OBJS = main.o lib.o math.o
  TARGET = main
  CC  = gcc
  AR = ar -crs
  LD = ld 
  MAR = -melf-x86-64
  SHARED = -fpic -shared $(MAR)
  EFLAGS = -e -o
  CFLAGS = -c -w $(MAR) -g
  AFLAGS = -s -m64 -o
  LFLAGS = -L/usr/lib/ -lstdc
  
  $(TARGET):$(OBJS)
    $(CC) $(CFLAGS) $(OBJS)  -o $(TAGGET)
  
  one.o:one.c two.c \                  #可以使用\进行换行
        three.c
      gcc one.c two.c -o one.o
      
  three.o:three.h
  one.o:one.h three.o                  #隐含规则可以省略

  clean:                               #没有任何依赖的规则永远不会被自动执行
      rm *.o
  .PHONY:clean1
  clean1:
      -rm *.exe                           #-表示忽略执行错误
  ::always:                               #双冒号规则总是会被无条件执行
    echo "ex" 
  [-/]include *.mk foo other              #包含其他make,-/s表示找不到包含的文件不会报错终止
  环境变量MAKEFILES                         #此环境变量会在make执行时,优先读取,并且不会报错也不会作为终极目标
  MAKEFILE_LIST                           #这个变量存储了makefile链表,最后一个表示当前的mk文件
  %:force                                 #模式规则
  force:;
  
  #定义变量方式,有延后和立即展开
  =
  :=
  ?=
  +=
  define name
    defdata
  endef
  
  #条件语句
  ifdef  ifeq  ifndef  ifneq
  
  ```
