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

     `延原型链比对的是原型在内存中的位置，不同全局作用域中的相同对象的原型是不同的，因为他们在内存中的地址不同`

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

* 实现`instanceof`

  * ```javascript
    function instance_of(a, b) {
            if (typeof a !== 'object') {
            throw('reght is not object')
            }
    
            if (typeof b !== 'function'){
                throw('left is not function');
            }
    
            const cPrototype = b.prototype;
            let oPrototype = Object.getPrototypeOf(a);
    
            while(cPrototype !== oPrototype){
               if(oPrototype === Object.prototype){
                    return false;
               }
               oPrototype = Object.getPrototypeOf(oPrototype)     
            }
    
            return true;
    }
    ```
    



## SaveValue

* `SameValue`算法为JavaScript内部计算方法，无法调用，其可调用实现为：`Object.is(x,y);`

* 规则：

  1. 如果 `Type(x)` 与 `Type(y)` 的结果不一致，返回` false`，否则
  2. 如果 `Type(x) `结果为 `Undefined`，返回 `true`
  3. 如果 `Type(x) `结果为 `Null`，返回 `true`
  4. 如果 `Type(x)` 结果为` Number`，则如果 `x` 为 `NaN`，且 `y` 也为 `NaN`，返回 `true`如果 `x` 为 `+0`，`y `为 `-0`，返回 `false`如果 `x` 为 `-0`，`y `为 `+0`，返回 `false`如果 `x` 与 `y` 为同一个数字，返回 `true`返回 `false`
  5. 如果 `Type(x)` 结果为 `String`，如果 `x `与 `y` 为完全相同的字符序列（相同的长度和相同的字符对应相同的位置），返回 `true`，否则，返回 `false`
  6. 如果 `Type(x)` 结果为 `Boolean`，如果 `x` 与 `y` 都为 `true` 或 `false`，则返回 `true`，否则，返回 `false`
  7. 如果 `x` 和 `y `引用到同一个 `Object` 对象，返回 `true`，否则，返回 `false`

* 案例：

  * ```javascript
    SameValue(NAN,NAN);				//true
    SameValue(0,-0);				//false
    ```

## SameValueZero

* 与SameValue类似，但是0和-0返回true。Map与Set使用SameValueZero判断key

* 案例

  * ```javascript
    SameValueZero(NAN,NAN);				//true
    SameValueZero(0,-0);				//true
    
    //map类中的SameValueZero
    const map = new Map();
    const obj = {};
    map.set(0,1);
    map.set(obj,2);
    map.has(-0);						//true
    map.has(obj);						//true
    obj.a = 'a';
    map.has(obj);						//true
    ```



