# js相等运算

## ===

* 机制：

  1. 如果两边类型不同，返回`false`
  2. 如果两边有`NAN`，返回`false`
  3. 如果两边是数字，相等返回`true`
  4. 如果两边是`Boolean`，全为`true|false`返回`true`
  5. 如果两边是`undefined`或者`null`，转化为`false`
  6. 如果两边是字符串，每个位置的字符相等返回`true`
  7. 如果两边是引用类型，引用对象相同返回`true`

* 案例：

  * ```javascript
    //类型
    123 === '123';				//false
    NAN === NAN;				//false
    123 === 123; 				//true
    true === true;				//true
    null === null;				//true
    '123' === '123';			//true
    []	=== []					//false
    
    const arr = [];
    const arr2 = arr;
    arr === arr2;				//true
    ```



## ==

* 机制：

  1. 如果两边类型相同，返回 `a === b`
  2. 如果两边有`NAN`，返回`false`
  3. 如果两边是`undefined`或者`null`，转化为`false`
  4. 如果两边有`Boolean`，则转化为`Boolean`
  5. 如果一边是字符串：
     * 另一边是数字，字符串转化为数字
     * 另一边是`Boolean`，转化为`0|1`
     * 另一边是引用类型，调用`ValueOf|toString`

* 案例：

  * ```javascript
    123 == '123'  				//true
    NAN == NAN;					//false
    null == undefined;			//true
    
    const obj = {
        valueOf: function(){
            return 0;
        }
    }
    obj == 0;					//true
    ```



## instanceof

* 作用：`instanceof` 运算符用来检测右边对象的原型是否存在于左边对象的原型链上。

* 机制：

  1. 如果右边不是`object`类型，报错`right is not object`
  2. 如果左边不是一个`function|class`类型，报错`left is not callable`
  3. 左边的对象如果不是实例对象，返回`false`
  4. 从左边的对象的原型开始，延原型链迭代，到`Object`的原型为止，将每个迭代项与右边对象的原型依次对比，若存在匹配，则返回`true`，否则返回`false`。

* 案例：

  * ```javascript
    1 instanceof 2;				//right is not object,left is not callable
    
    function A(){
    	a = 1;    
    }
    
    function B(){
        b = 2;
    }
    
    //B继承A
    B.prototype = new A();
    
    new B() instanceof A;		//true
    A instanceof B;				//false
    B instanceof A;				//false，左边应为一个实例对象
    ```

* 实现instanceof

  * ```
    
    ```

    