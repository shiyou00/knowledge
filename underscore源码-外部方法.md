### each方法

```
_.each = _.forEach = function(obj, iteratee, context) {
        // 根据 context 确定不同的迭代函数
        iteratee = optimizeCb(iteratee, context);
        // 如果没有this则返回原函数
        var i, length;

        // 如果是类数组
        // 默认不会传入类似 {length: 10} 这样的数据
        if (isArrayLike(obj)) {
            // 遍历
            for (i = 0, length = obj.length; i < length; i++) {
                iteratee(obj[i], i, obj);
            }
        } else { // 如果 obj 是对象
            // 获取对象的所有 key 值
            var keys = _.keys(obj);

            // 如果是对象，则遍历处理 values 值
            for (i = 0, length = keys.length; i < length; i++) {
                iteratee(obj[keys[i]], keys[i], obj); // (value, key, obj)
            }
        }

        // 返回 obj 参数
        // 供链式调用（Returns the list for chaining）
        // 应该仅 OOP 调用有效
        return obj;
    };
```

### map 方法的实现
```
	_.map = _.collect = function(obj, iteratee, context) {
        // 根据 context 确定不同的迭代函数
        iteratee = cb(iteratee, context);

        // 如果传参是对象，则获取它的 keys 值数组（短路表达式）
        var keys = !isArrayLike(obj) && _.keys(obj),
            // 如果 obj 为对象，则 length 为 key.length
            // 如果 obj 为数组，则 length 为 obj.length
            length = (keys || obj).length,
            results = Array(length); // 创建一个有相应length的空数组包含

        // 遍历
        for (var index = 0; index < length; index++) {
            // 如果 obj 为对象，则 currentKey 为对象键值 key
            // 如果 obj 为数组，则 currentKey 为 index 值
            var currentKey = keys ? keys[index] : index;
            results[index] = iteratee(obj[currentKey], currentKey, obj);
        }

        // 返回新的结果数组
        return results;
    };
```

## reduce方法的实现
```
	// Create a reducing function iterating left or right.
    // dir === 1 -> _.reduce
    // dir === -1 -> _.reduceRight
    function createReduce(dir) {
        // Optimized iterator function as using arguments.length
        // in the main function will deoptimize the, see #1991.
        function iterator(obj, iteratee, memo, keys, index, length) {
            for (; index >= 0 && index < length; index += dir) {
                var currentKey = keys ? keys[index] : index;
                // 迭代，返回值供下次迭代调用
                memo = iteratee(memo, obj[currentKey], currentKey, obj);
            }
            // 每次迭代返回值，供下次迭代调用
            return memo;
        }

        // _.reduce（_.reduceRight）可传入的 4 个参数
        // obj 数组或者对象
        // iteratee 迭代方法，对数组或者对象每个元素执行该方法
        // memo 初始值，如果有，则从 obj 第一个元素开始迭代
        // 如果没有，则从 obj 第二个元素开始迭代，将第一个元素作为初始值
        // context 为迭代函数中的 this 指向
        return function(obj, iteratee, memo, context) {
            iteratee = optimizeCb(iteratee, context, 4);
            var keys = !isArrayLike(obj) && _.keys(obj),
                length = (keys || obj).length,
                index = dir > 0 ? 0 : length - 1;

            // Determine the initial value if none is provided.
            // 如果没有指定初始值
            // 则把第一个元素指定为初始值
            if (arguments.length < 3) {
                memo = obj[keys ? keys[index] : index];
                // 根据 dir 确定是向左还是向右遍历
                index += dir;
            }
            return iterator(obj, iteratee, memo, keys, index, length);
        };
    }

    // **Reduce** builds up a single result from a list of values, aka `inject`,
    // or `foldl`.
    // 与 ES5 中 Array.prototype.reduce 使用方法类似
    // _.reduce(list, iteratee, [memo], [context])
    // _.reduce 方法最多可传入 4 个参数
    // memo 为初始值，可选
    // context 为指定 iteratee 中 this 指向，可选
    _.reduce = _.foldl = _.inject = createReduce(1);

    // The right-associative version of reduce, also known as `foldr`.
    // 与 ES5 中 Array.prototype.reduceRight 使用方法类似
    _.reduceRight = _.foldr = createReduce(-1);
```
