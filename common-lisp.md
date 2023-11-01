# COMMON-LISP基础

## 基础
+ list         ;;列表
> '(1 2 3) => (quote ( 1 2 3)) => (list 1 2 3) => (1 2 3) 

+ dolist     ;;循环列表
```lisp
(dolist (val list)
    body *
)
```

+ format

+ print   ;;打印,可被读回的形式
> (print obj  outfile)

+ read-line

+ y-or-n-p
>  (y-or-n-p "[y/n]?")  < y  => T

+ return    ;;立即返回,这是一个宏,返回到nil  block块

+ with-open-file  ;;打开文件流并在结束时关闭

+ (sleep 60)   ;;休眠

+ macroexpand-1 ;;查看一个宏展开后的形式

+ (gensym)  ;;返回一个唯一的可以被安全使用的符号

+ (floor a)  ;;向负无穷方向截断
+ (celling a) ;;向正无穷方向截断
+ (truncate a) ;;趋零截断
+ (round a)  ;;四舍五入到最近整数
+ (mod a) ;;取模数
+ (rem a)  ;;相当于c++的%

### 字符操作
```lisp
#\a  ;;可以得到想要的字符
(length str)  ;;获取字符串的长度
(elt)
(aref)
(char)

```

### 赋值
``` lisp
;;setf是赋值宏
;;setq是基础宏，必须带'
(setq 'a 1)  ;;一次只能操作一个
(setf a 1 b 2)  ;;一次可以操作多个

(aref array 0)  ;;数组访问
(gethash 'key hash) ;;hash访问
(field o)  ;;类的属性访问,它们都返回一个可以被设置值的位置

```
+ let  ;;临时重新绑定变量名，防止以外修改外部的变量名定义,离开时恢复成原来的值.另一方面let的形式其实就和函数参数行为是一样的
> （let ((a 13))  body*）
>   (let* ((a 13) (b a)) body*)

+ defparameter
> (defparameter  name 10   "desc")

+ defvar   ；；同上，但是不会覆盖以及存在的同名变量
> (defvar name 10  "desc")

+ defconstant  ;;定义常量,如果对其重新求值会报错
> (defconstant   +name+  10)
;;使用上面的操作符声明变量时无论在什么位置都会自动成为全局变量

+ 测等
```lisp
=      ;;比较数字
char=  ;;比较字符
eq     ;;测试底层是否是同一类型和值
> (eq 1 1) => T
> (eq '(1 3) '(1 3)) => nil
> (eql "aa" "aa")=> nil
eql    ;;和上面类似,比较类型和值,都相同时为真
> (eql "aa" "aa")=> nil
;;eq和eql是比较数字和字符的

equal ;;比上面宽松,如果拥有相同的递归结构也会被认为相同.比如如果向量和列表结构有相同的树形结构,那么就认为是相同的.能比较更多的类型
> (equal '(1 3) '(1 3)) => T
> (eql "aa" "aa")=> T
equalp ;;比上面更宽松,只要在主观上相同就认为它们是相同的
> (equalp "a" "A") => T
> (equalp 1 1.0) => T

```


### 分支
;;依次测试表达式,有一个是真就返回
(or (fun)  (fun2))

;;测试所有表达式,有一个是假就返回
(and (fun) (fun2))

(not  (= 1 1)) =>  nil   ;;对结果布尔取反
;;测试条件,真返回前面的,假返回后面的
(if (< 2 3) 
    (fun)
    (fun2)
)


(progn 
    body*   ;;组合一些列表达式为一个
)

when与unless 测试表达式,为T/nil时执行一些列表达式

;;相当于switch
(cond 
    (a (do afun))
    (b (do bfun))
    (c (do cfun))
)


### 循环
```
;;do,基础的循环结构,还有一个do*,在求值后续变量的每个变量的步长之前为前面的变量赋值,这样可以让后面的引用前面的
(do 
    ((变量定义 var 0 (+ 1 var)) (var1 10 (- 1 var1)))
    ((循环终止测试)  ((满足测试,立即终止并返回结果) (满足测试,立即终止并返回结果)))
    body*  ;;多个表达式
)



;;dolist,用于链表循环取值
(dolist (x '(1 2 3))
    (print x)
)


;;dotimes,指定固定次数循环
(dotimes (i 4) (print i))

;;loop强大的循环宏
(loop
    body*
    (return)
)


tagbody 和 go

```

### 函数
```lisp
;;定义函数
(defun name (&whole whole a b &operation (c (+ a b)) &rest rest &key (:ddd d) (:eee e) f)
    "document"
    body*
    (list d e f)
)
;;可以给关键字指定别名,来隐藏内部的实际表示
(name 'a 'b 'c :ddd 10 :eee 5 :f10)


(defun name (&whole whole a b &optional c (d a d-supplied-p) &rest rest &key one (two 0 c-supplied-p) three)
    "my fun"
    (print whole) ;;whole代表整个表达式,必须在第一个
    (+ a b c)    ;;正常值
    (+ rest)   ;;rest会将值收集到列表中并在展开时扩展到列表外
    (if d-supplied-p ;;在默认值未传递值的情况下这个值未nil否则为t
        (print T)
        (print nil)
    )
    (+ one two three)
)



;;函数块
{
(block name
    body*
    (return-from name (list 1 3))  ;;可以多返回值
)

}



;;函数宏
(defmacro name () ;;其他与函数一样
    body*
)

如下定义了一个when宏,表达式为T时,执行一些列操作. 还有一个unless表达式为nil时才会执行
(defmacro when (condtion &rest body)
    "desc"
    `(if ,condtion 
        (progn ,@body)))


;;这些配合宏一起使用效果更加
{
 #'(lambda (x) (< 1 x))
 #' 获取函数本身 => (function name);;返回一个函数对象
 (funcall #'name 1 3) ;;调用一个函数
 (apply #'name  #'name2 (list 2 3 4)) ;;循环将列表的每一项作用于前面的所有函数一次
 '(1 2 3) => (quote 1 2 3) =>(1 2 3)     ;;阻止表达式求值
 `(1 ,(+ 2 3))                                           ;;阻止表达式求值,其中以逗号开头的会被求值
 `(1 ,@(list 1 2 3))  => (1 1 2 3)                       ;;   ,@表示将列表扩展到外围
}


```


### 结构

#### 向量
> (vector ) => #()
> (vector 1) => #(1)
> (make-array 5 :initial-element nil) => #(nil nil nil nil nil)
;;可变数组,最大宽度为3,当前计数是0的可变数组,元素类型是字符
> (make-array 3 :initial-element 'a fill-pointer 0 :adjustable t :element-type 'character) => ""
> (vector-push 'a name)  ;;压入一个向量
> (vector-pop  name)   ;;弹出一个值
^
;;push和pop只能在带有fill-pointer的向量中使用
> (vector-push-extend 'a name) ;;向可变数组压入值,如果长度不够就扩展

(length name)  => number  ;;序列长度
(elt name 0) => ;;返回序列上的指定索引的值
(setf  (elt name 0) 11) => ;;设置相应索引的值
^
;;理论上所有的序列操作都可以看作是length和elt,setf的操作
(count  1 name) ;;查看索引1在序列中出现的次数
(find 1 name)  ;;在序列中查找指定的值
(postion 返回相应值的索引)
(remove #\a "abcd") => "bcd"  ;;返回移除指定值后的序列
(substitute #\x  #\b "bbb") => "xxx" ;;返回替换后的序列
;;test传入一个测试参数, :key传入一个抽值参数,把抽取的值传给test测试函数测试
;;test-not ;;以相反的逻辑执行
;;其他有用的参数是:start :end指示边界
;;from-end ;;将倒叙执行
;;count 指定执行的次数
;;complement 对函数的结果取反
(find 'a #((c e) (g a) (a u))  :test (complement #'string=)   :key #'first) 
(remove-duplicates name) ;;去重
(copy-seq name) ;;返回一个新的序列,但是不会复制序列绑定的元素
(reverse name) ;;返回一个反转后的新序列,不会深度复制

;;type,可以是string,list,vector
(concatenate type  value value) ;;将任意数量的序列组合成一个新序列

;;排序
(sort  (vector "sd" "gf")  #'string<) 
(stable-sort )  和上面类型,只是不会排相同的元素,而上面那个则不能保证
(merge type  list1 list2 #'<) 返回合并后的排序序列
(subseq "sdfasdfa" 3) => "asdfa"  ;;返回指定索引的子序列
(subseq "sdfadf" 3 6)
(search list1 list2) ;;返回list1在list2出现的索引位置

;;返回两个序列首次不匹配的位置
(mismatch "fooera" "foorae")  => 3 

(every #'evenp #(1 2 3 4)) => nil ;;全为真,结果为真
(some #'evenp #(1 2 3 4)) => T  ;;有一个为真,结果为真
(notany #'>  list1 list2) nil   ;;全为真,则返回假
(notevery ...);;有一个是假,就返回真,全为真时返回假

;;操作多个列表,应用传送的函数,并返回相应类型的数据结构
(map 'vector  #'*  #(1 2 3 4)   #(1 2 3 4)) #(1 4 9 16)
(map-into a  #'+ a b c)   ;;将abc的结果相加并赋值给a
(reduce #+ list)  ;;循环的顺序的将函数应用到list上

#### hash表
;;创建一个hash表,并且测试函数是equal
(make-hash-table :test 'equal)
(gethash 'foo  hash)   ;;查询hash表
(setf  (gethash 'foo hash) 'name)   ;;设置值
(multiple-value-bind (value1 value2 )  (gethash key  hashtable)) ;;获取多个返回值
(remhash 'foo hash) ;;移除键值
(clrhash hash) ;;清空hash表
(maphash #'(lambda (k v)  (print k) (print v)))    hash表操作



#### 列表
(cons 1 2)  ;;创建一个链表结构
(car list) ;;取链表的prev指针
(cdr list) ;;获取链接结构的next指针
(append list1 list2) ;;链接链表
(nconc list  list) ;;链接两个链表,只是单纯的链接,不会创建新的链表
(copt-list )   ;;复制链表
(nth 2 list) ;;返回链表的第n个元素
(nthcdr  1 list)  ;;返回n次cdr之后的结果
(last )  ;;返回最后一个链表
(butlast) ;;返回除最后一个链接的结果
(ldiff)  ;;获取指定长度的链表
(tailp)  ;;如果是链表返回真
(list* )  ;;构造一个链表,但是让最后的那个参数成为cdr
(make-list  5 :initial-element 0) ;;构造一个初始值为0的5元素链表
(consp)  ;;测试对象是否是点对单元
(atom)  ;;测试对象是否不是点对单元
(listp)  ;;测试对象是否为点对单元或者nil
(null)  ;;测试对象是否为nil
(mapcar  #'+  list1 list2)  ;;总是返回一个链表
(maplist) ;;和上面一样,但是它将整个链表传递给测试函数
(mapcan)
(mapcon) ;;这两个是将结果用nconc拼接在一起产生新的结果
(mapc)
(mapl)  ;;这俩函数只返回第一个列表的实参


#### 点对单元
(copy-tree) ;;复制树
(tree-equal)  ;;测等
(adjoin 1 name)  ;;使用name数据和1构建一个集合
(member )  ;;查询某项是否在集合中
(intersection)
(union)
(set-differenge)
(set-exclusive-or)
(subsetp) ;;测试

#### 其他结构
plist和alisp
(destructuring-bing  (x y z) (1 2 3)  (list :x x :y y :z z))=>(:x 1 :y 2 :z 3)  ;;绑定



## 面向对象
### 类
;;这是最简单的情况
(defclass  name ()
    (shape
    bank) 
)

;;创建一个实例
(make-instance 'name)

;;访问槽
(slot-value classname  'solt-name ) ;;访问槽 和setf配合使用就是设置槽

(defclass other ( name)  括号里的那么表示继承
    (
        (shape 
            :initarg :shape      ;;指定一个关键字,给外部用来初始化这个成员
            :initform (error "msg") )  ;;如果外部没有传入这个初始化的值,就使用默认的值
            :read shape           ;;访问函数
            :writer (setf shape)   ;;写入函数,创建默认的读写函数
            :accessor   ;;同时创建上面两种函数
            :docementation "desc"
            :allocation :class   ;;默认是:instance,如果是class的的话就是所有类共享这个成员
    )
)

(make-instance 'name  :shape "eea" ) ;;传入成员值创建一个类
(with-slots (slot*)  classvalue   body*)

;;当类使用了accessor这个标记时,可以使用这个
(with-accessors   ...)

### 成员函数
;;声明一个函数
(defgeneric draw (shape sha)
(:documentation "desc"))

;;定义一个函数,当调用这个对象并传入一个circle值的时候,就调用这个
(defmethod draw ((shape circle)  sha)
    (<  1 shape)
    (+ 1 sha)
)
;;另一个函数,相当于泛型
(defmethod draw ((shape square)  bea) 
(call-next-method)  ;;相当于super调用基类的函数
)

;;三个辅助方法;before , after 
;;其中around优先于其他任何代码执行,也就是调用下一个类的around的方法,最后才开始类的主方法
(defmethod draw :before ((shape square) bea)   ...);这个函数优先于原函数执行


## 文件
;;有定三种不同的未找到的行为
;;error ,create ,nil
(open "name"  :if-does-no-exist)
(close)
(read)
(print)
(read-char)
(read-line)
(read-sequence)
## 编译
```lisp
;;加载
(load "file.lisp")

(comp)
```

## 其他
+ 特定类型的对象,诸如特定大小以下的整数与字符,可能会直接在内存中表示,而其他对象,会被表示成一个实际的指针.这就是需要eq和eql的原因
+ 绑定变量前不许要声明
+ 强类型,使用过程碰到与期望的值不同时不会进行隐式转换,而是会报错
+ 函数参数是引用传递的
+ 同一个符号可以绑定多个不同类型的值
+ 变量分为词法作用域和全局作用域
+ 非数值参数几乎都是按引用传递
+ 闭包,将局部变量封存,并永久存储,外部无法访问,但是整个闭包内却可以
```lisp 
(defparameter mfn 
    (let ((ccc 0)) 
        #'(lambda () (setf ccc (+ 1 ccc)))))  ;;ccc被封闭在这个匿名函数中,并作为let的形式返回
(funcall *fun*) ;;每次调用都会使count加一

```


## 特殊符号
+ return-from
    defun定义函数时,也是在外围包裹了一个函数同名的block块,所以可以直接从函数返回

