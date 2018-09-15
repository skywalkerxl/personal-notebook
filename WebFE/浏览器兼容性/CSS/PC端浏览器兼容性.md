# PC端浏览器兼容性

## 1. 不同浏览器的标签默认的外补丁和内补丁不同

问题症状：随便写几个标签，不加样式控制的情况下，各自的margin 和padding差异较大。

解决方案：CSS里加 `*{margin:0;padding:0;}`

备注：几乎所有的CSS文件开头都会用通配符*来设置各个标签的内外补丁是0。

## 2. 块属性标签float后，又有横行的margin情况下，在IE6显示margin比设置的大（即双倍边距bug）

问题症状：常见症状是IE6中后面的一块被顶到下一行

解决方案：在float的标签样式控制中加入 display:inline;将其转化为行内属性

## 3. 设置较小高度标签（一般小于10px），在IE6，IE7，遨游中高度超出自己设置高度

问题症状：IE6、7和遨游里这个标签的高度不受控制，超出自己设置的高度

解决方案：给超出高度的标签设置overflow:hidden;或者设置行高line-height 小于你设置的高度。

备注：这种情况一般出现在我们设置小圆角背景的标签里。出现这个问题的原因是IE8之前的浏览器都会给标签一个最小默认的行高的高度。即使你的标签是空的，这个标签的高度还是会达到默认的行高。

## 4. 行内属性标签，设置display:block后采用float布局，又有横行的margin的情况，IE6间距bug

问题症状：IE6里的间距比超过设置的间距

解决方案：在display:block;后面加入display:inline;display:table;

备注：行内属性标签，为了设置宽高，我们需要设置display:block;(除了input标签比较特殊)。在用float布局并有横向的margin后，在IE6下，他就具有了块属性float后的横向margin的bug。不过因为它本身就是行内属性标签，所以我们再加上display:inline的话，它的高宽就不可设了。这时候我们还需要在display:inline后面加入display:table。

## 5. 图片默认有间距 *

问题症状：几个img标签放在一起的时候，有些浏览器会有默认的间距，加了问题一中提到的通配符也不起作用。

解决方案：使用float属性为img布局

备注：因为img标签是行内属性标签，所以只要不超出容器宽度，img标签都会排在一行里，但是部分浏览器的img标签之间会有个间距。使用float是正道。

## 6. 标签最低高度设置min-height不兼容

问题症状：因为min-height本身就是一个不兼容的CSS属性，所以设置min-height时不能很好的被各个浏览器兼容

解决方案：如果我们要设置一个标签的最小高度200px，需要进行的设置为：{min-height:200px; height:auto !important; height:200px; overflow:visible;}

备注：在B/S系统前端开时，有很多情况下我们又这种需求。当内容小于一个值（如300px）时。容器的高度为300px；当内容高度大于这个值时，容器高度被撑高，而不是出现滚动条。这时候我们就会面临这个兼容性问题。

# 附：IE6中的常见BUG与相应的解决办法

## 一、IE6双倍边距bug

例如“margin-left:10px” 在IE6中，该值就会被解析为20px。

解决方案：需要在该元素中加入display:inline 或 display:block 明确其元素类型

##二、IE6中3像素问题及解决办法

当元素使用float浮动后，元素与相邻的元素之间会产生3px的间隙。诡异的是如果右侧的容器没设置高度时3px的间隙在相邻容器的内部，当设定高度后又跑到容器的相反侧了。

解决方案：需要使布局在同一行的元素都加上float浮动。

##三、IE6中奇数宽高的BUG

IE6中奇数的高宽显示大小与偶数高宽显示大小存在一定的不同。其中要问题是出在奇数高宽上。

解决方案：需要尽量将外部定位的div高宽写成偶数即可。

##四、IE6中图片链接的下方有间隙

IE6中图片的下方会存在一定的间隙，尤其在图片垂直挨着图片的时候，即可看到这样的间隙。

解决方案：需要将img标签定义为display:block 或定义vertical-align对应的属性。也可以为img对应的样式写入font-size:0

##五、IE6下空元素的高度BUG

如果一个元素中没有任何内容，当在样式中为这个元素设置了0-19px之间的高度时。此元素的高度始终为19px。

解决的方法如下:

1. 在元素的css中加入：overflow:hidden
2. 在元素中插入html注释：<!– >
3. 在元素中插入html的空白符：&nbsp;
4. 在元素的css中加入：font-size:0

##六、重复文字的BUG

在某些比较复杂的排版中，有时候浮动元素的最后一些字符会出现在clear清除元素的下面。

解决方法如下：

1. 确保元素都带有display:inline
2. 在最后一个元素上使用“margin-right:-3px
3. 为浮动元素的最后一个条目加上条件注释，<!–[if !IE]>xxx<![endif]–>
4. 在容器的最后元素使用一个空白的div，为这个div指定不超过容器的宽度。

##七、IE6中 z-index失效

具体BUG为，元素的父级元素设置的z-index为1，那么其子级元素再设置z-index时会失效，其层级会继承父级元素的设置，造成某些层级调整上的BUG

原因：z-index起作用有个小小前提，就是元素的position属性要 是relative，absolute或是fixed。

解决方案：

1. position:relative改为position:absolute；
2. 去除浮动；
3. 浮动元素添加position属性（如relative，absolute等）。

**IE6结语：实际上中，很多BUG的解决方法都可以使用display:inline、font-size:0、float解决。因此我们在书写代码时要记住，一旦使用了float浮动，就为元素增加一个display:inline样式，可以有效的避免浮动造成的样式错乱问题。使用空DIV时，为了避免其高度影响布局美观，也可以为其加上font-size:0 这样就很容易避免一些兼容上的问题。**

##八、子元素不会继承透明效果

``` css
.opacity{

    background-color: #000000;

	filter: alpha(opacity=50);

	background-color: rgba(0, 0, 0, 0.5);

}

```

