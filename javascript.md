# javascropt 重点

## 运算符
  *  `==` 只比较两个值是否相同。
     > `1 == '1' result: true`
  * `===` 则判断类型是否一致，上面的示例，结果为false
  
## 对象
  * 对象有多种创建方式
  
```
    var obj = {
        name:"bob",
        age:18,
        show:function(){alert(this.name)},
        showage(){...}; //和上面相同，函数可以省略属性名。
    }
    
```
