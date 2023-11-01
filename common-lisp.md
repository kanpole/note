# COMMON-LISP����

## ����
+ list         ;;�б�
> '(1 2 3) => (quote ( 1 2 3)) => (list 1 2 3) => (1 2 3) 

+ dolist     ;;ѭ���б�
```lisp
(dolist (val list)
    body *
)
```

+ format

+ print   ;;��ӡ,�ɱ����ص���ʽ
> (print obj  outfile)

+ read-line

+ y-or-n-p
>  (y-or-n-p "[y/n]?")  < y  => T

+ return    ;;��������,����һ����,���ص�nil  block��

+ with-open-file  ;;���ļ������ڽ���ʱ�ر�

+ (sleep 60)   ;;����

+ macroexpand-1 ;;�鿴һ����չ�������ʽ

+ (gensym)  ;;����һ��Ψһ�Ŀ��Ա���ȫʹ�õķ���

+ (floor a)  ;;�������ض�
+ (celling a) ;;���������ض�
+ (truncate a) ;;����ض�
+ (round a)  ;;�������뵽�������
+ (mod a) ;;ȡģ��
+ (rem a)  ;;�൱��c++��%

### �ַ�����
```lisp
#\a  ;;���Եõ���Ҫ���ַ�
(length str)  ;;��ȡ�ַ����ĳ���
(elt)
(aref)
(char)

```

### ��ֵ
``` lisp
;;setf�Ǹ�ֵ��
;;setq�ǻ����꣬�����'
(setq 'a 1)  ;;һ��ֻ�ܲ���һ��
(setf a 1 b 2)  ;;һ�ο��Բ������

(aref array 0)  ;;�������
(gethash 'key hash) ;;hash����
(field o)  ;;������Է���,���Ƕ�����һ�����Ա�����ֵ��λ��

```
+ let  ;;��ʱ���°󶨱���������ֹ�����޸��ⲿ�ı���������,�뿪ʱ�ָ���ԭ����ֵ.��һ����let����ʽ��ʵ�ͺͺ���������Ϊ��һ����
> ��let ((a 13))  body*��
>   (let* ((a 13) (b a)) body*)

+ defparameter
> (defparameter  name 10   "desc")

+ defvar   ����ͬ�ϣ����ǲ��Ḳ���Լ����ڵ�ͬ������
> (defvar name 10  "desc")

+ defconstant  ;;���峣��,�������������ֵ�ᱨ��
> (defconstant   +name+  10)
;;ʹ������Ĳ�������������ʱ������ʲôλ�ö����Զ���Ϊȫ�ֱ���

+ ���
```lisp
=      ;;�Ƚ�����
char=  ;;�Ƚ��ַ�
eq     ;;���Եײ��Ƿ���ͬһ���ͺ�ֵ
> (eq 1 1) => T
> (eq '(1 3) '(1 3)) => nil
> (eql "aa" "aa")=> nil
eql    ;;����������,�Ƚ����ͺ�ֵ,����ͬʱΪ��
> (eql "aa" "aa")=> nil
;;eq��eql�ǱȽ����ֺ��ַ���

equal ;;���������,���ӵ����ͬ�ĵݹ�ṹҲ�ᱻ��Ϊ��ͬ.��������������б�ṹ����ͬ�����νṹ,��ô����Ϊ����ͬ��.�ܱȽϸ��������
> (equal '(1 3) '(1 3)) => T
> (eql "aa" "aa")=> T
equalp ;;�����������,ֻҪ����������ͬ����Ϊ��������ͬ��
> (equalp "a" "A") => T
> (equalp 1 1.0) => T

```


### ��֧
;;���β��Ա��ʽ,��һ������ͷ���
(or (fun)  (fun2))

;;�������б��ʽ,��һ���Ǽپͷ���
(and (fun) (fun2))

(not  (= 1 1)) =>  nil   ;;�Խ������ȡ��
;;��������,�淵��ǰ���,�ٷ��غ����
(if (< 2 3) 
    (fun)
    (fun2)
)


(progn 
    body*   ;;���һЩ�б��ʽΪһ��
)

when��unless ���Ա��ʽ,ΪT/nilʱִ��һЩ�б��ʽ

;;�൱��switch
(cond 
    (a (do afun))
    (b (do bfun))
    (c (do cfun))
)


### ѭ��
```
;;do,������ѭ���ṹ,����һ��do*,����ֵ����������ÿ�������Ĳ���֮ǰΪǰ��ı�����ֵ,���������ú��������ǰ���
(do 
    ((�������� var 0 (+ 1 var)) (var1 10 (- 1 var1)))
    ((ѭ����ֹ����)  ((�������,������ֹ�����ؽ��) (�������,������ֹ�����ؽ��)))
    body*  ;;������ʽ
)



;;dolist,��������ѭ��ȡֵ
(dolist (x '(1 2 3))
    (print x)
)


;;dotimes,ָ���̶�����ѭ��
(dotimes (i 4) (print i))

;;loopǿ���ѭ����
(loop
    body*
    (return)
)


tagbody �� go

```

### ����
```lisp
;;���庯��
(defun name (&whole whole a b &operation (c (+ a b)) &rest rest &key (:ddd d) (:eee e) f)
    "document"
    body*
    (list d e f)
)
;;���Ը��ؼ���ָ������,�������ڲ���ʵ�ʱ�ʾ
(name 'a 'b 'c :ddd 10 :eee 5 :f10)


(defun name (&whole whole a b &optional c (d a d-supplied-p) &rest rest &key one (two 0 c-supplied-p) three)
    "my fun"
    (print whole) ;;whole�����������ʽ,�����ڵ�һ��
    (+ a b c)    ;;����ֵ
    (+ rest)   ;;rest�Ὣֵ�ռ����б��в���չ��ʱ��չ���б���
    (if d-supplied-p ;;��Ĭ��ֵδ����ֵ����������ֵδnil����Ϊt
        (print T)
        (print nil)
    )
    (+ one two three)
)



;;������
{
(block name
    body*
    (return-from name (list 1 3))  ;;���Զ෵��ֵ
)

}



;;������
(defmacro name () ;;�����뺯��һ��
    body*
)

���¶�����һ��when��,���ʽΪTʱ,ִ��һЩ�в���. ����һ��unless���ʽΪnilʱ�Ż�ִ��
(defmacro when (condtion &rest body)
    "desc"
    `(if ,condtion 
        (progn ,@body)))


;;��Щ��Ϻ�һ��ʹ��Ч������
{
 #'(lambda (x) (< 1 x))
 #' ��ȡ�������� => (function name);;����һ����������
 (funcall #'name 1 3) ;;����һ������
 (apply #'name  #'name2 (list 2 3 4)) ;;ѭ�����б��ÿһ��������ǰ������к���һ��
 '(1 2 3) => (quote 1 2 3) =>(1 2 3)     ;;��ֹ���ʽ��ֵ
 `(1 ,(+ 2 3))                                           ;;��ֹ���ʽ��ֵ,�����Զ��ſ�ͷ�Ļᱻ��ֵ
 `(1 ,@(list 1 2 3))  => (1 1 2 3)                       ;;   ,@��ʾ���б���չ����Χ
}


```


### �ṹ

#### ����
> (vector ) => #()
> (vector 1) => #(1)
> (make-array 5 :initial-element nil) => #(nil nil nil nil nil)
;;�ɱ�����,�����Ϊ3,��ǰ������0�Ŀɱ�����,Ԫ���������ַ�
> (make-array 3 :initial-element 'a fill-pointer 0 :adjustable t :element-type 'character) => ""
> (vector-push 'a name)  ;;ѹ��һ������
> (vector-pop  name)   ;;����һ��ֵ
^
;;push��popֻ���ڴ���fill-pointer��������ʹ��
> (vector-push-extend 'a name) ;;��ɱ�����ѹ��ֵ,������Ȳ�������չ

(length name)  => number  ;;���г���
(elt name 0) => ;;���������ϵ�ָ��������ֵ
(setf  (elt name 0) 11) => ;;������Ӧ������ֵ
^
;;���������е����в��������Կ�����length��elt,setf�Ĳ���
(count  1 name) ;;�鿴����1�������г��ֵĴ���
(find 1 name)  ;;�������в���ָ����ֵ
(postion ������Ӧֵ������)
(remove #\a "abcd") => "bcd"  ;;�����Ƴ�ָ��ֵ�������
(substitute #\x  #\b "bbb") => "xxx" ;;�����滻�������
;;test����һ�����Բ���, :key����һ����ֵ����,�ѳ�ȡ��ֵ����test���Ժ�������
;;test-not ;;���෴���߼�ִ��
;;�������õĲ�����:start :endָʾ�߽�
;;from-end ;;������ִ��
;;count ָ��ִ�еĴ���
;;complement �Ժ����Ľ��ȡ��
(find 'a #((c e) (g a) (a u))  :test (complement #'string=)   :key #'first) 
(remove-duplicates name) ;;ȥ��
(copy-seq name) ;;����һ���µ�����,���ǲ��Ḵ�����а󶨵�Ԫ��
(reverse name) ;;����һ����ת���������,������ȸ���

;;type,������string,list,vector
(concatenate type  value value) ;;������������������ϳ�һ��������

;;����
(sort  (vector "sd" "gf")  #'string<) 
(stable-sort )  ����������,ֻ�ǲ�������ͬ��Ԫ��,�������Ǹ����ܱ�֤
(merge type  list1 list2 #'<) ���غϲ������������
(subseq "sdfasdfa" 3) => "asdfa"  ;;����ָ��������������
(subseq "sdfadf" 3 6)
(search list1 list2) ;;����list1��list2���ֵ�����λ��

;;�������������״β�ƥ���λ��
(mismatch "fooera" "foorae")  => 3 

(every #'evenp #(1 2 3 4)) => nil ;;ȫΪ��,���Ϊ��
(some #'evenp #(1 2 3 4)) => T  ;;��һ��Ϊ��,���Ϊ��
(notany #'>  list1 list2) nil   ;;ȫΪ��,�򷵻ؼ�
(notevery ...);;��һ���Ǽ�,�ͷ�����,ȫΪ��ʱ���ؼ�

;;��������б�,Ӧ�ô��͵ĺ���,��������Ӧ���͵����ݽṹ
(map 'vector  #'*  #(1 2 3 4)   #(1 2 3 4)) #(1 4 9 16)
(map-into a  #'+ a b c)   ;;��abc�Ľ����Ӳ���ֵ��a
(reduce #+ list)  ;;ѭ����˳��Ľ�����Ӧ�õ�list��

#### hash��
;;����һ��hash��,���Ҳ��Ժ�����equal
(make-hash-table :test 'equal)
(gethash 'foo  hash)   ;;��ѯhash��
(setf  (gethash 'foo hash) 'name)   ;;����ֵ
(multiple-value-bind (value1 value2 )  (gethash key  hashtable)) ;;��ȡ�������ֵ
(remhash 'foo hash) ;;�Ƴ���ֵ
(clrhash hash) ;;���hash��
(maphash #'(lambda (k v)  (print k) (print v)))    hash�����



#### �б�
(cons 1 2)  ;;����һ������ṹ
(car list) ;;ȡ�����prevָ��
(cdr list) ;;��ȡ���ӽṹ��nextָ��
(append list1 list2) ;;��������
(nconc list  list) ;;������������,ֻ�ǵ���������,���ᴴ���µ�����
(copt-list )   ;;��������
(nth 2 list) ;;��������ĵ�n��Ԫ��
(nthcdr  1 list)  ;;����n��cdr֮��Ľ��
(last )  ;;�������һ������
(butlast) ;;���س����һ�����ӵĽ��
(ldiff)  ;;��ȡָ�����ȵ�����
(tailp)  ;;�������������
(list* )  ;;����һ������,�����������Ǹ�������Ϊcdr
(make-list  5 :initial-element 0) ;;����һ����ʼֵΪ0��5Ԫ������
(consp)  ;;���Զ����Ƿ��ǵ�Ե�Ԫ
(atom)  ;;���Զ����Ƿ��ǵ�Ե�Ԫ
(listp)  ;;���Զ����Ƿ�Ϊ��Ե�Ԫ����nil
(null)  ;;���Զ����Ƿ�Ϊnil
(mapcar  #'+  list1 list2)  ;;���Ƿ���һ������
(maplist) ;;������һ��,�����������������ݸ����Ժ���
(mapcan)
(mapcon) ;;�������ǽ������nconcƴ����һ������µĽ��
(mapc)
(mapl)  ;;��������ֻ���ص�һ���б��ʵ��


#### ��Ե�Ԫ
(copy-tree) ;;������
(tree-equal)  ;;���
(adjoin 1 name)  ;;ʹ��name���ݺ�1����һ������
(member )  ;;��ѯĳ���Ƿ��ڼ�����
(intersection)
(union)
(set-differenge)
(set-exclusive-or)
(subsetp) ;;����

#### �����ṹ
plist��alisp
(destructuring-bing  (x y z) (1 2 3)  (list :x x :y y :z z))=>(:x 1 :y 2 :z 3)  ;;��



## �������
### ��
;;������򵥵����
(defclass  name ()
    (shape
    bank) 
)

;;����һ��ʵ��
(make-instance 'name)

;;���ʲ�
(slot-value classname  'solt-name ) ;;���ʲ� ��setf���ʹ�þ������ò�

(defclass other ( name)  ���������ô��ʾ�̳�
    (
        (shape 
            :initarg :shape      ;;ָ��һ���ؼ���,���ⲿ������ʼ�������Ա
            :initform (error "msg") )  ;;����ⲿû�д��������ʼ����ֵ,��ʹ��Ĭ�ϵ�ֵ
            :read shape           ;;���ʺ���
            :writer (setf shape)   ;;д�뺯��,����Ĭ�ϵĶ�д����
            :accessor   ;;ͬʱ�����������ֺ���
            :docementation "desc"
            :allocation :class   ;;Ĭ����:instance,�����class�ĵĻ����������๲�������Ա
    )
)

(make-instance 'name  :shape "eea" ) ;;�����Աֵ����һ����
(with-slots (slot*)  classvalue   body*)

;;����ʹ����accessor������ʱ,����ʹ�����
(with-accessors   ...)

### ��Ա����
;;����һ������
(defgeneric draw (shape sha)
(:documentation "desc"))

;;����һ������,������������󲢴���һ��circleֵ��ʱ��,�͵������
(defmethod draw ((shape circle)  sha)
    (<  1 shape)
    (+ 1 sha)
)
;;��һ������,�൱�ڷ���
(defmethod draw ((shape square)  bea) 
(call-next-method)  ;;�൱��super���û���ĺ���
)

;;������������;before , after 
;;����around�����������κδ���ִ��,Ҳ���ǵ�����һ�����around�ķ���,���ſ�ʼ���������
(defmethod draw :before ((shape square) bea)   ...);�������������ԭ����ִ��


## �ļ�
;;�ж����ֲ�ͬ��δ�ҵ�����Ϊ
;;error ,create ,nil
(open "name"  :if-does-no-exist)
(close)
(read)
(print)
(read-char)
(read-line)
(read-sequence)
## ����
```lisp
;;����
(load "file.lisp")

(comp)
```

## ����
+ �ض����͵Ķ���,�����ض���С���µ��������ַ�,���ܻ�ֱ�����ڴ��б�ʾ,����������,�ᱻ��ʾ��һ��ʵ�ʵ�ָ��.�������Ҫeq��eql��ԭ��
+ �󶨱���ǰ����Ҫ����
+ ǿ����,ʹ�ù���������������ֵ��ͬʱ���������ʽת��,���ǻᱨ��
+ �������������ô��ݵ�
+ ͬһ�����ſ��԰󶨶����ͬ���͵�ֵ
+ ������Ϊ�ʷ��������ȫ��������
+ ����ֵ�����������ǰ����ô���
+ �հ�,���ֲ��������,�����ô洢,�ⲿ�޷�����,���������հ���ȴ����
```lisp 
(defparameter mfn 
    (let ((ccc 0)) 
        #'(lambda () (setf ccc (+ 1 ccc)))))  ;;ccc��������������������,����Ϊlet����ʽ����
(funcall *fun*) ;;ÿ�ε��ö���ʹcount��һ

```


## �������
+ return-from
    defun���庯��ʱ,Ҳ������Χ������һ������ͬ����block��,���Կ���ֱ�ӴӺ�������

