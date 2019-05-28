## 关键字
在CSS中，有4个关键字理论上可以应用于任何的CSS属性，它们是initial(初始)、inherit(继承)、unset(未设置)、revert(还原)。而all的取值只能是以上这4个关键字。本文将介绍initial、inherit、unset、revert和all
﻿
## initial
表示元素属性的初始默认值(该默认值由官方CSS规范定义)
兼容性: IE不支持
﻿
```
div{display: initial;} // display初始默认值为inline
```
﻿
## inherit
表示元素的直接父元素对应属性的计算值
兼容性: IE7-不支持
﻿
```
<style>
.box_css1{
    border: 1px solid red;
    padding: 10px;
    width: 100px;
}
.test_css1{
    border: inherit;
    height: 30px;
}
</style>
<div class="box_css1">
    <div class="test_css1">测试一</div>
</div>
```
﻿
## unset
unset相对于initial和inherit而言，相对复杂一点。表示如果该属性默认可继承，则值为inherit；否则值为initial。实际上，设置unset相当于不设置
兼容性: IE不支持，safari9-不支持，ios9.2-不支持，android4.4.4-不支持
﻿
## revert
表示样式表中定义的元素属性的默认值。若用户定义样式表中显式设置，则按此设置；否则，按照浏览器定义样式表中的样式设置；否则，等价于unset
兼容性: 只有safari9.1+和ios9.3+支持
﻿
## all
表示重设除unicode-bidi和direction之外的所有CSS属性的属性值，取值只能是initial、inherit、unset和revert
兼容性: IE不支持，safari9-不支持，ios9.2-不支持，android4.4-不支持
﻿
```
<style>
.test{
    border: 1px solid black;
    padding: 20px;
    color: red;
}
.in{
/*  all: initial; //都取默认值 border:none;padding:0;color:black;
    all: inherit; // 都取父元素继承值 border:1px solid black;padding:20px;color:red;
    all: unset; // .in的所有属性都相当于不设置值，默认可继承的继承，不可继承的保持默认值border:none;padding:0;color:red;
    all: revert; // 等价于unset
*/
}
</style>
<div class="test">
    <div class="in">测试文字</div>
</div>
```