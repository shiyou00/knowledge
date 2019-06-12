## 前言
之所以要编写插件，也是想把一些把一些通用的功能抽取出来，整个团队可以直接使用，而无需更多的关心该如何实现它，大大的节约了重复开发时间。本文将详细讲解如何开发一个jquery插件。

## jquery插件开发模式

jQuery插件开发方式主要有三种：

1. 通过`$.extend()`来扩展jQuery
2. 通过`$.fn` 向jQuery添加新的方法
3. 通过`$.widget()`应用jQuery UI的部件工厂方式创建

今天主要讲解`$.extend()` 以及 `$.fn`

### `$.extend()`
该形式的方法适合在jquery身上添加一个静态方法，并不适合开发更加复杂的插件

插件定义：
```
$.extend({
    hello: function(name){
        console.log('hello:',name);
    }
})
```

插件调用：
```
$.hello('jack');
```

上面代码中，通过`$.extend()`向jQuery添加了一个`hello`函数，然后通过`$`直接调用。到此你可以认为我们已经完成了一个简单的jQuery插件了。

这种方式无法利用jQuery强大的选择器带来的便利，要处理DOM元素以及将插件更好地运用于所选择的元素身上，还是需要使用第二种开发方式。你所见到或使用的插件也大多是通过此种方式开发。


### `$.fn`

基本格式
```
$.fn.pluginName = function(){}

or

(function($){
    $.fn.pluginName = function(){}
})(jQuery)
```

插件使用：
```
$('div').myPlugin();
```

插件定义：
```
$.fn.myPlugin = function() {
    其中的this 就是调用插件的元素 $('div')
    this.css('color', 'red');
}
```

[重要]this就是调用插件的元素，这也是种形式的强大之处，可以获取到调用元素的上下文，有了这个我们能做到的事情就很多了。

### 编写一个更加复杂的插件
先来看看效果：
![](./image/231.gif)

这是一个轮播图的效果。我们开始实现吧。

html
```
<div id="marquee" class="marquee">
  <div class="marquee-wrap">
    <div class="marquee-inner promo">
    </div>
    <div class="marquee-ft">
      <button class="prev">上一步</button>
      <button class="next">下一步</button>
    </div>
  </div>
</div>
```

css
```
.promo:after, .promo:before{
    content: " ";
    display: table;
}
.promo:after{
    height: 0;
    line-height: 0;
    visibility: hidden;
    clear: both;
}
html,body{
    width:100%;
}
.marquee {
    overflow: hidden;
    margin: 20px auto;
}
.marquee .marquee-wrap {
    width: 100%;
    overflow: hidden;
    height: 100%;
    position: relative;
}

.marquee .marquee-inner{
    overflow: hidden;
    transition-duration: 0.3s;
    transform: translate3d(0px, 0px, 0px);
    backface-visibility: hidden;
    left: 0;
    opacity: 1;
}
.marquee .marquee-thumbnail{
    float: left;
    overflow: hidden;
    width: 520px;
    display: block;
    visibility: visible;
}

.marquee .marquee-ft{
    width: 100%;
    display: flex;
    justify-content: space-between;
    position: absolute;
    top:50%;
    left:0;
}
```

调用插件：
```
$(function () {
    $('#marquee').marquee({
        imgList:[
            "https://img.alicdn.com/tps/i4/TB1f8_hbMKG3KVjSZFLSuuMvXXa.jpg_q90_.webp",
            "https://img.alicdn.com/tps/i4/TB18LGBbMaH3KVjSZFpSuvhKpXa.jpg_q90_.webp",
            "https://img.alicdn.com/simba/img/TB1gCYda25G3KVjSZPxSuvI3XXa.jpg"
        ],
        width: '520',
        height: '280',
        interval: 5000,
        auto: true
    });
});
```

定义插件：
```
$.fn.marquee = function(params) {
    var _params = params || {}; // 获取传来的参数
    var $marquee = this; // 获取调用元素对象
    var $inner = $marquee.find('.marquee-inner');
    var imgList = _params.imgList || []; // 获取传来的图片数组
    var WIDTH = _params.width || 520; // 参数
    var HEIGHT = _params.height || 280;// 参数
    var $prev = $marquee.find(".prev"); // 获取上一步按钮
    var $next = $marquee.find(".next"); // 获取下一步按钮
    var target = 0; // 定义图片展示的页数
    var _auto = _params.auto || false; // 是否自动轮播
    var interval = _params.interval === void 0 ||  _params.interval< 1000 ? 3000 : _params.interval; // 定义自动轮播时间
    var timer = null;
    // 根据imgList 生成相应的DOM结构，插入到页面中
    function createImg(){
        let str = '';
        imgList.forEach((item)=>{
            str += `<a class="marquee-thumbnail" href=${item} data-fancybox>
                <img src=${item}>
             </a>`
        });
        $inner.html(str)
    }
    // 根据参数初始化DOM结构的css属性值
    function initSize(){
        $marquee.css({
            width: WIDTH + 'px',
            height: HEIGHT + 'px'
        });

        $marquee.find(".marquee-thumbnail").css({
            width: WIDTH + 'px'
        });

        $inner.css({
            width: imgList.length * WIDTH + 'px'
        });
    }
    // 图片移动功能函数
    function move(){
        $inner.css({
            transform: `translate3d(${ -target*WIDTH }px, 0px, 0px)`
        })
    }
    // 改变target值功能函数
    function calculate(num){
        target += num ;
        target = target < 0 ? imgList.length-1 : target ;
        target = target > imgList.length-1 ? 0 : target ;
    }   
    // 自动轮播功能函数
    function automatic(){
        timer && clearTimeout(timer);
        timer = setTimeout(function(){
            next();
            automatic();
        },interval);
    }
    // 下一页功能函数
    function next (){
        calculate(1);
        move();
    }
    // 上一页功能函数
    function prev (){
        calculate(-1);
        move();
    }
    // 停止轮播
    function stop(){
        timer && clearTimeout(timer);
    }
    // 为元素添加上事件
    function addClickEvent(){

        $prev.on("click",function () {
            prev();
        });

        $next.on("click",function () {
            next();
        });

        $marquee.on("mouseover",function () {
            stop();
        });

        $marquee.on("mouseout",function () {
            automatic();
        });
    }
    // 初始化函数
    function render(){
        createImg();
        initSize();
        addClickEvent();
        if(_auto){
            automatic();
        }
    }

    render();
    // 返回this对象，让链式调用继续
    return $marquee;
};
```

如此我们就实现了一个常用的图片轮播的插件，其它小伙伴使用起来也是非常方便的。

该插件可优化的地方还是非常多的，例如可以把这些方法使用面向对象的形式编写，等等。
