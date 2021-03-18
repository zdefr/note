## Map

* Map是es6中出现的“键/值”存储的新规范

* 创建Map实例：

  * ```javascript
    //空map
    let map = new Map();
    
    //以键值对形式赋值
    let map1 = new Map([
    	[1,2],			//1=>2
    	[3,4],			//3=>4
    ]);
    
    //自动忽略第三个元素
    let map1 = new Map([
    	[1,2,3],		//1=>2
    	[3,4,5],		//3=>4
    ]);
    ```

* 增删改查：

  * ```javascript
    let map = new Map();
    
    //添加“键/值”：map.set([key, value]);
    //set方法返回映射实例，所以可以将多个set方法连缀起来
    map.set(1,2);				//1=>2
    map.set(3,4).set(5,6);		//3=>4,5=>6
    
    //map可以用Object作为key
    const o = {};
    map.set(o,1);				//o=>1
    
    //判断key是否在map中：map.has(key);
    map.has(o);					//true
    
    
    ```

    