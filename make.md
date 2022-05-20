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
        + -march           #编译的程序模式 32or64
    ```
    
    + ld             #链接器，链接目标
    
    ```
        手动编译一次动态库，假设有一个one.c库
        gcc -E one.c -o one.i
        gcc -S -melf_x86_64 one.i -o one.s
        gcc -c -fpic -shared one.s -o one.o
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
        file         #加载程序
        list         #查看源码
        display /20i $pc      #只能查看正在运行处的20条汇编
        disassemble func      #查看指定函数的汇编
        break        #下断点
        break if     #条件断点
        info         #查看
        watch expr   #监视表达式，发生变时中断
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


  ```

  #make中规则的命令如果属于同一行，那它们就会在同一个shell中执行，否则就是相互独立的，所以最后使用；和\来分割以及表示换行
  #-l表示依赖库,规则和gcc相同,|之后的依赖更新不会导致规则重建
  $(TARGET):$(OBJS) -lstdc| head.h;ls -a \
    @$(CC) -c $(CFLAGS) $(OBJS)  -o $(TAGGET); $(LD) (LDFLAGS) $@ -lstdc \
    -ls -a \
    +rm *.o
    
  #隐含规则可以省略编译命令
  one.o:one.h 

  main.o::main.c                         #最终规则,说明目标不是中间文件产生的,不要查找隐含规则,去生成中间文件来构建目标,可以有多个重建目标的最终,哪个规则的依赖更新就只执行哪个规则的依赖.单冒号会将所有目标规则合并到一起执行
      command 

  [-]include *.mk foo other              #包含其他make,-表示找不到包含的文件不会报错终止
  
  sources = main.c one.c
  sinclude $(sources:.c=.mk)             #sinclude表示包含多个文件 是gnu的扩展


  环境变量
  MAKEFILES                               #此环境变量会在make执行时,优先读取,并且不会报错也不会作为终极目标
  MAKEFILE_LIST                           #这个变量存储了makefile链表,最后一个表示当前的mk文件
  
  %::force;                               #模式规则，表示所有目标，优先级最低
   
  缺省规则,目标与所有现有规则都不匹配的时候就执行此规则
  %::
    command
    
  缺省规则 
  .DEFAULT:
    command
  
  
  后缀规则,后缀规则不能有依赖,否则就是普通规则,也可以使用.SUFFIXES来实现,变量SUFFIXE存储了可识别的后缀列表,表示只给出目标.o时,此规则利用.c文件来执行命令重建.o
  .c.o:
    command
  
  

静态库规则,这种规则,只能使用ar这种操作库的命令,隐含规则使用ar来构建
stdclib(one.o two.o):one.o two.o
    command

stdclib:stdclib(one.o) stdclib(two.o) #表示当one.o two.o更新时,重新构建此库
    command
  
  
  cd subdir && $(MAKE)                                 #表示make程序
  $(MAKE) -C subdir                                    #-C表示进入subdir目录并执行make,当$(MAKE)作为规则命令，并且主make带有-n -t -q的时候，它还是会执行
  
  变量
  CURIDR                                               #会被自动赋值，表示当前进入的目录
  MAKEFLAGS                                            #表示传给make的参数
  MAKELEVEL                                            #代表了make嵌套的深度
  .EXPORT_ALL_VARIABLES                                #表示导出所有变量到子例程
  export                                               #导出所有变量到子例程
  export variables = value
  unexport variables

  
  
  
  #定义变量方式,有延后和立即展开
  =   #使用时展开,字面赋值
  :=  #定义时展开,逻辑赋值
  ?=  #没有定义就用=定义,否则不改变值
  +=  #添加,没有就用=定义,有直接添加
  
  
  VPATH = src : ../include                               #从当前目录开始,使用这些目录作为搜索目标和依赖的位置
  vpath %.h ../heards                                    #为某种文件或者模式单独设置目录
  vapth %.h 
  vpath                                                  #清除所有目录搜索规则
  
  GPATH = src : ../execute                          #当需要重建,但目标不存在时,将会在工作目录下构建目标,否则就在目标所在的目录下重建,和VPATH有所不同
  .LIBPATTERNS =% lib%.so                    #这个变量控制了,在依赖中包含-lstdc++这种库时,如何解析
  
  .DELETE_ON_ERROR：%.o %.exe                        #发生错误时,删除构建的目标,有一堆这种特殊目标
  
  one.out two.out files.out:head.h                  #多目标
      @echo head.h $(subst out,,$@) >$@ 
  
#静态规则,目标是非模式匹配的
OBJS = main.o two.o one.h 

    $(OBJS) :%.o:%.c                               #可以看成是一个目标依赖于一个模式规则
        gcc $< -o $@



需要明确的是,在模式规则中,例如模式e%t:%,如果目标是src/eat,那么依赖是src/a,在进行模式替换时,会先去掉
目标部分,等匹配完成后再加上目录.在模式规则中目标带目录的时候
    %.c:%.o ;command                                #模式规则,目标是模式匹配的
        

```


###变量

```

    make a=val                        #可以在make运行时定义变量的值
                                      #引用一个没有定义的变量，默认为空

    $a                                #单字变量可以不带括号
    $abc = $(a)bc

    a:a.c b.c
    d=$(a:.c=.o)                      #d= a.o b.o快速替换，相当于函数patsubst
 or d=$(a:%.c=%.o)
 
 

    a =b.c
    a+=d.c                            #a本身立即展开,但是a里面的引用变量不会展开
 
    overried CFLAGS += -g             #可以防止变量被命令行修改


    define name
        command \                     #多行变量定义,相当于 = 定义的变量
        command
    
    endef
    
    
    %.o:CFLAGS = -g -w                  #目标指定变量,模式指定变量,可以和其他变量同时存在，对它的修改不会影响全局，重建目标时优先使用
    
    
    ifeq ifneq ifdef ifndef else endif    #条件测试语句
    ifeq($(CC),gcc)
       content
    else ifdef G++
       content
    endif
    
    
    
    
    

```


### make 变量

```
此变量记录了命令行传给make的终极目标列表
MAKECMDGOALS 

隐含规则会使用一些列这种变量,来控制隐含规则的编译成功,如LINK.o,COMPILE.c等这些表示隐含规则的变量里存了编译目标的命令
OUTPUT_OPTION ;可以指定一个移动命令如;mv $*.o $@


---------------
自动化变量


规则中目标的文件名
$@

如果目标是一个静态库文件,它代表一个成员名,否则为空
$%

规则依赖的第一个
$<

所有比目标新的依赖文件列表,如果目标是静态库文件名,则代表库成员(.o)的更新情况
$?

规则的所有依赖列表,会去掉重复依赖,当目标是静态库文件名时,代表所有库成员名
$^

和$^相同,但是保留重复
$+

在模式规则中代表茎,也就是模式规则中%匹配的目标的部分,在非模式规则中,代表后辍之前的部分,如果是未识别的后辍,则为空
$*


变种自动变量(自动变量前面加上D,F),相当于函数dir,notdir
$(@D) #目标的目标名
$(@F) #目标的文件名,不包含目录




特殊变量
一般是不能在规则中使用自动变量的,但是可以使用这种方式来使用
/usr/include/stdio.c:$$@ $$(@D) $$(@F)


```



### 内嵌函数

```
字符串替换函数,不能使用模式
$(subst from,to,text)

模式字符串替换函数
$(patsubst)

去掉前后空格,中间空格保留一个
$(strip string)

在in里查找字符串,查到返回find否则为空
$(findstring find,in)


返回符合指定模式的结果
$(filter %.c %o , text)


反过滤函数
$(filter-out %.c %.o..   , text)


排序并去重复
$(sort text)



返回text的第一个单词
$(firstword text)

取单词函数,取text的第n个项,从1开始
$(word n,text)

范围取单词函数,取从s,到e的项
$(wordlist s,e,text)

计算text中单词的数目
$(words text)

返回text中各项的目录部分,如果没有默认为./ .返回的结果中包含最后的/
$(dir text)

返回text中各项的非目录部分,只有目录的项对应返回空
$(notdir text)


取后缀函数
$(suffix text)

取前缀函数
$(basename text)


给text中的每一项添加后缀
$(addsuffix suffix,text)


添加前缀,给每一项的前面添加前缀
$(addprefix prefix,text)

链接函数,在list1的第一项后面添加list2的第一项,以此类推,多余的原样返回
$(join list1,list2)

返回当前目录下所有匹配模式的文件,可以多模式
$(wildcard *.c)

循环函数,循环将text中的每一项赋值给var,并执行eval的表达式
$(foreach var,text,eval)

添加函数,测试第一个值,为真返回第二项,否则返回第三项,如果两项都没有返回空
$(if $(GCC) ,$(GCC),$(G++))


调用自己定义的函数变量
$(call variable,param,param,...)
link = $(addsuffix $(1),$(2))
result :=$(call link ,one,.c) 
result := one.c

获取变量的字面值,也就是不展开时的值,但是对与:=这中立即展开的会返回其立即展开后的值
$(value name)

对make表达式求值并返回
$(eval make-val)

返回变量的来源,即在哪定义的
$(origin name)


返回一个致命错误
$(error text)


返回一个警告给用户
$(warning text)




```



### 选项

```c

    -b/-m #提供和其他make的兼容性
    -B #强制重建所有规则的目标文件
    -C #进入目录并执行
    -d #打印调试信息
    -e #表示使用环境变量的值，文件内或者命令行变量不会替换环境变量
    -f 指定文件,可以同时指定多个

    -i #忽略错误
    -I #指定makefile的搜索目录
    -j #指定同时运行的job数
    -k #忽略终极目标之前的错误

    -n #只打印要执行的命令,不执行
    -o #指定文件不需要重建
    -p #可以查看make执行前读取的信息,如make -p -f ./null
    -q #不运行命令,只查是否有目标需要被重建,返回0不需要重建,1有目标需要重建,2错误
    -r #取消所有有隐含规则
    -R #取消所有隐含变量
    -s #取消命令回显
    -S #取消-k选项,也就是遇到错误会退出,用在递归make中
    -t #只更新目标文件的时间戳
    -w #切换目录时，打印进入的目录
    -W #假定目标是最新的,执行更新命令,可以和n配合使用查看依赖与此文件的目标


```



### 其他

```

    target:;                   #空目标，总是被认为是最新的，以它为依赖的目标总是会被执行

    .PHONY                     #特殊目标，可以实现一些功能，用来兼容老式make程序

    

当一个规则有目标和依赖,没有命令时,会使用对应的隐含规则,重建此目标
当一个目标没有依赖,,会使用隐含规则重建依赖,如果此目标没有命令,也会使用隐含规则的命令重建此目标

目标和依赖中不能使用自动变量,但是可以使用模式,和$$@ $$(@D)这类特殊自动变量

```





### 一个完整例子

```make
makefile
    VPATH = src:/heards
    vpath %.h ../heards
    CC := gcc
    CFLAGS := -g -w -O2 -melf_x86_64
    AR := ar rcs
    LD := ld
    LDFLAGS := -melf_x86_64
    INCLUDE := -I./include -I"%INCLUDE%/include"
    LIB := -L./lib -L"%LIB%/lib"
    STATIC := -static
    SHARED := -fpic -shared 
    

    PREFIX := ./
    BUILD=./build
    INSTALL := ./bin
    CLEAN = -rm -rf *.o *.i *.s ./build ./bin


    SOURCS := main.o
    LINKLIBS := -lc -lstdc++ -lgcc
    
    TARGET := one.exe
    
    include make2.mk
    include make3.mk
    
    
make2.mk
vpath %.h
OBJS =$(patsubst %.c,%.o,$(wildcar *.c)
$(TARGET) : $(SOURCES) | $(LINKLIBS) -lstdc++
    $(CC) -c $(CFLAGS) $(SHARED) $(INCLUDE) $(LIB)  -o $(TARGET)

%.o:%.c

print:*.c
    echo $?
    touch print
    
make3.mk
    vpath


%.d:%.c
    $(CC) -MM $(CFLAGS) $< > $@.$$$$;\
    sed 's ,\($*\)\.o[:]*,\1.o $@:, g' <$@.$$$$ > $@;\
    rm -f $@.$$$$


source =main.c one.c
sinclude $(source %.c=%.d)



.PHONY:clean;
clean:
    $(CLEAN)
```
