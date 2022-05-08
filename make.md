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
        + -m           #����ĳ���ģʽ 32or64
    ```
    
    + ld             #������������Ŀ��
    
    ```
        �ֶ�����һ�ζ�̬�⣬������һ��one.c��
        gcc -E one.c -o one.i
        gcc -S -m64 -fpic -shared one.i -o one.s
        gcc -c -m64 -fpic -shared one.s -o one.o
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
        info         #�鿴�ϵ�
        watch expr   #���������ı�ʱ�ж�
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

