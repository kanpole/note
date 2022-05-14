# GCC��������MAKE��ʹ��


## GCC


### GCC���벽��

    
    + gcc -o  #����ɵ�Ŀ��
    + gcc -E  #Ԥ���� .i
    + gcc -S  #���  .s
    + gcc -c  #���ɱ����ļ����������� .o
    
    ```shell
    
        ���ò�����
        + -Wl   #���ݸ��������Ĳ������ö��Ÿ���
        + -Wall #�����ϸ񾯸�
        + -I    #include·��
        + -L    #��·��
        + -l    #���ӵĿ�
        + -g    #��ӵ�����Ϣ
        + -O2   #�Ż����� 
        + -static    #����ʱʹ�þ�̬���ӿ�
        + -fPic      #����λ���޹ش��룬һ�߸��˿ⲻ��ָ������ص�λ�ã�����ĳЩϵͳ���Ŀ���Ҫ�������жϺ���Ҫ��ĳ���̶�λ��
        + -shared    #����ʱʹ�ö�̬��
        + -rpath     #ָ������ʱ��̬��Ĳ���·��
        + -rpath-link  #ָ������ʱ��ӿ�Ĳ���·��
        + -march           #����ĳ���ģʽ 32or64
    ```
    
    + ld             #������������Ŀ��
    
    ```
        �ֶ�����һ�ζ�̬�⣬������һ��one.c��
        gcc -E one.c -o one.i
        gcc -S -melf_x86_64 one.i -o one.s
        gcc -c -fpic -shared one.s -o one.o
        ld  -melf_x86_64 -fpic -shared one.o -o one.so
    
    
    
    
    ```
    
    
    + ar               #�����̬��
    > ar rcs           #������ļ�
    
    + ranlib           #�ؽ��������൱��ar -s
    + nm               #����Ϣ�鿴����
    + objdump          #������������
    + size             #ͷ�鿴����
    + string           #���ݲ鿴����
    + ldd              #���Ӳ鿴����
    + linux���� ulimit  #������Դ
### GBD����

    ```c
        file         #���س���
        list         #�鿴Դ��
        display      #�鿴���
        break        #�¶ϵ�
        break if     #�����ϵ�
        info         #�鿴
        watch expr   #���ӱ��ʽ��������ʱ�ж�
        delete       #ɾ���ϵ�
        continue     #����ִ�е��¸��ϵ�
        step         #���룬���뺯��
        next         #���������������ڲ�
        finish       #���е���ǰ�������أ�����ӡ������ջ����Ϣ
        until        #���е�ѭ������
        print        #�鿴����
        ptype        #�鿴����
        print array  #�鿴����
        print *array@len  #�鿴��̬�ڴ�
        print x=5         #�޸�����ʱ����
    
    ```


## Make����

  ����
  ```
  OBJS = main.o lib.o math.o
  TARGET = main
  CC  = gcc
  AR = ar -rcs
  LD = ld 

  SHARED = -fpic -shared $(MAR)

  CFLAGS := -c -w $(MAR) -g

  LFLAGS = -L/usr/lib/ -lstdc
  
  $(TARGET):$(OBJS);ls -a
    $(CC) $(CFLAGS) $(OBJS)  -o $(TAGGET) \
    ls -a
  
  one.o:one.h three.o                     #�����������ʡ�Ա�������

  .PHONY:clean
   clean:                                 #û���κ������Ĺ�����Զ���ᱻ�Զ�ִ��, �����ܻᱻ��Ϊ�����µ�
      -rm *.o                             #-��ʾ����ִ�д���@��ʾ���û���
      
  main.o::main.c                         #Ŀ������ж���ؽ����򣬵��Ǳ�����ͬһ��ð�Ÿ�ʽ��˫ð�ŵĴ����뵥ð�Ų�ͬ���ĸ�������������¾�ִֻ���ĸ����������
      command 
    
  main.o::main.h
      command
    



  #make�й���������������ͬһ�У������Ǿͻ���ͬһ��shell��ִ�У���������໥�����ģ��������ʹ�ã���\���ָ��Լ���ʾ����
    
  [-]include *.mk foo other              #��������make,-��ʾ�Ҳ����������ļ����ᱨ����ֹ
  
  sources = main.c one.c
  sinclude $(sources:.c=.mk)             #sinclude��ʾ��������ļ�
  
  ��������MAKEFILES                         #�˻�����������makeִ��ʱ,���ȶ�ȡ,���Ҳ��ᱨ��Ҳ������Ϊ�ռ�Ŀ��
  MAKEFILE_LIST                           #��������洢��makefile����,���һ����ʾ��ǰ��mk�ļ�
  %:force                                 #ģʽ���򣬱�ʾ����Ŀ�꣬���ȼ����

  
  
  cd subdir && $(MAKE)                                 #��ʾmake����
  $(MAKE) -C subdir                                    #-C��ʾ����subdirĿ¼��ִ��make,��$(MAKE)��Ϊ�������������make����-n -t -q��ʱ�������ǻ�ִ��
  
  ����CURIDR                                            #�ᱻ�Զ���ֵ����ʾ��ǰ�����Ŀ¼
  MAKEFLAGS                                            #��ʾ����make�Ĳ���
  MAKELEVEL                                            #������makeǶ�׵����
  .EXPORT_ALL_VARIABLES                                #��ʾ�������б�����������
  export variables = value
  unexport variables
  export                                               #�������б�����������
  
  
  
  #���������ʽ,���Ӻ������չ��
  =   #ʹ��ʱչ��
  :=  #����ʱչ��
  ?=  #û�ж������=����,���򲻸ı�ֵ
  +=  #���,û�о���=����,��ֱ�����
  

  
  #�������
  
  main.exe:main.c -lstdcpp| main.h ; gcc main.c                    #-l��ʾ������,�����gcc��ͬ,|֮����������²��ᵼ�¹����ؽ�
  
  VPATH = src : ../include                               #�ӵ�ǰĿ¼��ʼ,ʹ����ЩĿ¼��Ϊ����Ŀ���������λ��
  vpath %.h ../heards                                    #Ϊĳ���ļ�����ģʽ��������Ŀ¼
  vapth %.h 
  vpath                                                  #�������Ŀ¼��������
  
  GPATH = src : ../execute                          #����Ҫ�ؽ�,��Ŀ�겻����ʱ,�����ڹ���Ŀ¼�¹���Ŀ��,�������Ŀ�����ڵ�Ŀ¼���ؽ�,��VPATH������ͬ
  .LIBPATTERNS =lib%.so lib%.dll                    #�������������,�������а���-lstdc++���ֿ�ʱ,��ν���
  
  .DELETE_ON_ERROR��%.o %.exe                       #��һ����������Ŀ��
  
  one.out two.out files.out:head.h                         #��Ŀ��
      @echo head.h $(subst out,,$@) >$@ 
  



OBJS = main.o two.o one.h                           #��̬����,ģʽ�滻����,��̬�滻����

    $(filter %.o,$(OBJS)):%.o:%.c
        gcc $< -o $@


    aout bout:%out,%.c
        gcc $< -o $@
        echo $*                                     #echo a b
        

```


###����

```

    make a=val                        #������make����ʱ���������ֵ
                                      #����һ��û�ж���ı�����Ĭ��Ϊ��

    $a                                #���ֱ������Բ�������
    $abc = $(a)bc
    
    a=$(b)                            #��a��ֵ����������ʱa��������$(b),���ָ�ֵ�������ú���ı���
    b=c 


    
    a:=c
    b:=$(a)                           #�������ʱ��b=c�������ʱ��Ա��������ã��������ú���δ����ı���,������ǿ�
    
    a?=b                              #����aû��ֵ��ʱ��Żᱻ��ֵ
    
    
    a:a.c b.c
    d=$(a:.c=.o)                      #d= a.o b.o�����滻���൱�ں���patsubst
 or d=$(a:%.c=%.o)
 
 
    a+=b                              #���֮ǰaû�ж������=������
    
    a:=b.c
    a+=d.c                            #�൱��a:= b.c d.c,����չ��
 
    overried CFLAGS += -g             #���Է�ֹ�������������޸�


    define name
        command \                     #���б�������,�൱�� = ����ı���
        command
    
    endef
    
    
    %.o:CFLAGS = -g -w                  #Ŀ��ָ������,ģʽָ������,���Ժ���������ͬʱ���ڣ��������޸Ĳ���Ӱ��ȫ�֣��ؽ�Ŀ��ʱ����ʹ��
    
    
    ifeq ifneq ifdef ifndef else end    #�����������
    ifeq($(CC),gcc)
       content
    else ifdef G++
       content
    endif
    
    
    
    
    

```

### ��Ƕ����

```
�ַ����滻����,����ʹ��ģʽ
$(subst from,to,text)

ģʽ�ַ����滻����
$(patsubst)

ȥ��ǰ��ո�,�м�ո���һ��
$(strip string)

��in������ַ���,�鵽����find����Ϊ��
$(findstring find,in)


���ط���ָ��ģʽ�Ľ��
$(filter %.c %o , text)


�����˺���
$(filter-out %.c %.o..   , text)


����ȥ�ظ�
$(sort text)


ȡ���ʺ���,ȡtext�ĵ�n����,��1��ʼ
$(word n,text)

��Χȡ���ʺ���,ȡ��s,��e����
$(word s,e,text)

����text�е��ʵ���Ŀ
$(words text)


����text�ĵ�һ������
$(firstword text)


����text�и����Ŀ¼����,���û��Ĭ��Ϊ./ .���صĽ���а�������/
$(dir text)

����text�и���ķ�Ŀ¼����,ֻ��Ŀ¼�����Ӧ���ؿ�
$(notdir text)


ȡ��׺����
$(suffix text)

ȡǰ׺����
$(basename text)


��text�е�ÿһ����Ӻ�׺
$(addsuffix suffix,text)


���ǰ׺,��ÿһ���ǰ�����ǰ׺
$(addprefix prefix,text)

���Ӻ���,��list1�ĵ�һ��������list2�ĵ�һ��,�Դ�����,�����ԭ������
$(join list1,list2)

���ص�ǰĿ¼������ƥ��ģʽ���ļ�,���Զ�ģʽ
$(wildcard *.c)

ѭ������,ѭ����text�е�ÿһ�ֵ��var,��ִ��eval�ı��ʽ
$(foreach var,text,eval)

��Ӻ���,���Ե�һ��ֵ,Ϊ�淵�صڶ���,���򷵻ص�����,������û�з��ؿ�
$(if $(GCC) ,$(GCC),$(G++))


�����Լ�����ĺ�������
$(call variable,param,param,...)
link = $(addsuffix $(1),$(2))
result :=$(link one,.c) 
result := one.c

��ȡ����������ֵ,Ҳ���ǲ�չ��ʱ��ֵ,���Ƕ���:=��������չ���Ļ᷵��������չ�����ֵ
$(value name)

��make���ʽ��ֵ������
$(eval make-val)

���ر�������Դ,�����Ķ����
$(origin name)


����һ����������
$(error text)


����һ��������û�
$(warning text)




```



### ѡ��

```c
    -w #�л�Ŀ¼ʱ����ӡ�����Ŀ¼
    -s #��ֹ��ӡ�����
    -C #����һ��Ŀ¼��ִ��
    -i #������������˳�
    --keep-going #-������������˳�
    -e #��ʾʹ�û���������ֵ���ļ��ڻ��������б��������滻��������
        -t #�����ļ���ʱ�������ִ������������Ŀ����������д�+�Ż��ǻ�ִ��
```



### ����

```

    define name
        echo "$@"
        echo "$<" 
    endef

main.c:main.o
    @$(name)                   #����@��ʾ������ڵ����������ֹ����

-----------------------
    target:;                   #��Ŀ�꣬���Ǳ���Ϊ�����µģ�����Ϊ������Ŀ�����ǻᱻִ��

    .PHONY                     #����Ŀ�꣬����ʵ��һЩ���ܣ�����������ʽmake����

    



```





### һ����������

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
