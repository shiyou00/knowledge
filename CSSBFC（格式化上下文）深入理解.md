## 什么是BFC
> 在解释BFC之前，先说一下文档流。我们常说的文档流其实分为定位流、浮动流和普通流三种。而普通流其实就是指BFC中的FC。FC是formatting context的首字母缩写，直译过来是格式化上下文，它是页面中的一块渲染区域，有一套渲染规则，决定了其子元素如何布局，以及和其他元素之间的关系和作用。常见的FC有BFC、IFC，还有GFC和FFC。BFC是block formatting context，也就是块级格式化上下文，是用于布局块级盒子的一块渲染区域

## 触发条件
满足下列条件之一就可触发BFC

1、根元素，即HTML元素  
2、float的值不为none  
3、overflow的值不为visible  
4、display的值为inline-block、table-cell、table-caption  
5、position的值为absolute或fixed

## 作用
BFC是页面上的一个隔离的独立容器，容器里面的子元素不会影响到外面元素，反之亦然。它与“普通的块框”类似，但不同之处在于:

1、可以阻止元素被浮动元素覆盖  
2、可以包含浮动元素  
3、属于同一个BFC的两个相邻块级子元素的上下margin会发生重叠，(设置writing-mode:tb-rl时，水平margin会发生重叠)。所以当两个相邻块级子元素分属于不同的BFC时可以阻止margin重叠

【1】可以阻止元素被浮动元素覆盖

效果展示：  
通过改变内容为BFC，背景为浅绿色的盒子的属性，使其成为BFC，以此阻止被深绿色的浮动盒子覆盖
<iframe style="width: 100%; height: 364px;" src="https://shiyou00.github.io/lion/dist/html/css-bfc/bfc.html?case=f1" frameborder="0"></iframe>

【2】可以包含浮动元素

效果展示：  
操作天蓝色区域的属性，使其成为BFC。就可以包含住浮动元素
<iframe style="width: 100%; height: 364px;" src="https://shiyou00.github.io/lion/dist/html/css-bfc/bfc.html?case=f2" frameborder="0"></iframe>

【3】属于同一个BFC的两个相邻块级子元素的上下margin会发生重叠，(设置writing-mode:tb-rl时，水平margin会发生重叠)。所以当两个相邻块级子元素分属于不同的BFC时可以阻止margin重叠

效果展示：  
深红色背景的块级盒子二的外面包一个div，通过改变此div的属性使红色盒子与绿色盒子分属于两个不同的BFC，以此来阻止margin重叠
<iframe style="width: 100%; height: 364px;" src="https://shiyou00.github.io/lion/dist/html/css-bfc/bfc.html?case=f3" frameborder="0"></iframe>
