## 内部调用方法

### 无new构造一个对象方式
```
	var _ = function(obj) {
        // 以下均针对 OOP 形式的调用
        // 如果是非 OOP 形式的调用，不会进入该函数内部

        // 如果 obj 已经是 `_` 函数的实例，则直接返回 obj
        if (obj instanceof _)
            return obj;

        // 如果不是 `_` 函数的实例
        // 则调用 new 运算符，返回实例化的对象
        if (!(this instanceof _))
            return new _(obj);

        // 将 obj 赋值给 this._wrapped 属性
        this._wrapped = obj;
    };
```

### 判断是不是类似数组
```
var property = function(key) {
        return function(obj) {
            return obj == null ? void 0 : obj[key];
        };
};

var getLength = property('length');

var isArrayLike = function(collection) {
        // 返回参数 collection 的 length 属性值
        var length = getLength(collection);
        return typeof length == 'number' && length >= 0 && length <= MAX_ARRAY_INDEX;
};
```

### 修改回调函数的this指向
```
	// underscore 内部方法
    // 根据 this 指向（context 参数）
    // 以及 argCount 参数
    // 二次操作返回一些回调、迭代方法
    var optimizeCb = function(func, context, argCount) {
        // 如果没有指定 this 指向，则返回原函数
        if (context === void 0)
            return func;

        switch (argCount == null ? 3 : argCount) {
            case 1: return function(value) {
                return func.call(context, value);
            };
            case 2: return function(value, other) {
                return func.call(context, value, other);
            };

            // 如果有指定 this，但没有传入 argCount 参数
            // 则执行以下 case
            // _.each、_.map
            case 3: return function(value, index, collection) {
                return func.call(context, value, index, collection);
            };

            // _.reduce、_.reduceRight
            case 4: return function(accumulator, value, index, collection) {
                return func.call(context, accumulator, value, index, collection);
            };
        }

        // 其实不用上面的 switch-case 语句
        // 直接执行下面的 return 函数就行了
        // 不这样做的原因是 call 比 apply 快很多
        // .apply 在运行前要对作为参数的数组进行一系列检验和深拷贝，.call 则没有这些步骤
        // 具体可以参考：
        // https://segmentfault.com/q/1010000007894513
        // http://www.ecma-international.org/ecma-262/5.1/#sec-15.3.4.3
        // http://www.ecma-international.org/ecma-262/5.1/#sec-15.3.4.4
        return function() {
            return func.apply(context, arguments);
        };
    };
```
