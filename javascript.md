# javascropt 重点

## 运算符
  *  `==` 只比较两个值是否相同。
     > `1 == '1' result: true`
  * `===` 则判断类型是否一致，上面的示例，结果为false
  
## 对象
  * 对象有多种创建方式
  
```javascropt
    var obj = {
        name:"bob",
        age:18,
        show:function(){alert(this.name)},
        showage(){...}; //和上面相同，函数可以省略属性名。
    }
    
```
  * 另一种是在一个函数里面构造对象然后将对象返回
```
  function createObj(name){
      const obj = {};
      obj.name = name;
      obj.show = () => {...};
      return obj;
  }

```
  * 上面的方式可以简化,通过new创建一个对象
```
  function Obj(name){
      this.name = name;
      this.show = ()=>{...};
  }

let obj = new Obj("bob");

```

> 创建完成的对象可以继续往里面添加属性。

