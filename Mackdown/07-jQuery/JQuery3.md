# jQuery事件

## 目标

> 1. 能够说出4种常见的事件注册方式
> 2. 能够说出on绑定事件的优势
> 3. 能够说出jQuery委派的方式及优点
> 4. 能够说出绑定事件和解绑事件

## jQuery事件注册

> 1. ##### 单个事件注册
>
>    ```js
>    语法：
>    	$('元素').事件名称(function(){});
>    ```
>
> 2. ##### 多个事件注册
>
>    ```js
>    语法：
>    	$('元素').on(事件名称, function(){});   ---注册一个事件
>    
>    	$('元素').on({
>              事件名称: function() {},
>              事件名称: function() {},
>              事件名称: function() {}
>        })
>        ---元素注册多个事件
>    
>    注意：
>    	如果注册的事件处理程序相同，那么可以合写：
>        $('元素').on('click  mouseenter mouseleave', function(){
>          // 代表当前元素在执行 click， mouseenter ， mouseleave 事件时候，执行的代码是一样的
>        })
>    ```
>
> 3. ##### 通过on方式实现事件委派（委托）
>
>    ```js
>    语法：
>    	$('元素').on('事件名称', '真正执行事件的子元素', function(){})
>    例如：
>    	//给ul注册点击事件，但是在执行的时候，是点击每一个li执行的点击事件，委托思想
>    	$('ul').on('click', 'li', function(){ console.log(123) });
>    ```
>
> 4. ##### 总结使用on给元素注册事件的有点
>
>    ```js
>     1. 可以通过on的方式给元素注册一个事件
>     2. 通过on的方式给元素注册多个事件
>     3. 通过on的方式注册事件可以实现委托的效果
>    ```
>

## jQuery解绑事件

> 1. ##### 解绑事件
>
>    ```js
>    语法：
>    	$('元素').off([事件名称],[执行事件委托元素])
>    
>    注意：
>    	1. 如果off()中没有设置任何参数，代表将该元素身上的所有事件都解除掉
>     2. 如果要解除对应的事件，可以设置off('事件名称')
>    	3. 如果要解除委托事件，可以通过off('事件名称', '执行事件的元素')
>    	   例如：
>           $('ul').on('click', 'li', function(){}) ---> 通过委托给li注册的点击事件
>      	   $('ul').off('click', 'li')  ---> 解除li委托的点击事件
>    
>    	4. 如果一个元素只执行一次事件可以通过 one('事件名称', function(){})实现
>    ```

## jQuery自动触发事件

> 1. ##### 自动触发事件
>
>    ```js
>    $('元素').事件名称();
>    $('元素').trigger('事件类型');
>    例如：
>    	$('div').mouseenter(function(){
>           $(this).css('background','pink');
>        })
>    
>        $('div').mouseenter();
>        $('div').trigger('mouseenter')
>    ```

## jQuery事件对象参数

> 1. ##### jQuery中事件对象参数与webAPI中事件对象参数操作方式一样
>
>    ```js
>    阻止事件冒泡：
>    event.stopPropagation()
>    ```

## jQuery其他部分

> 1. ##### jQuery拷贝对象
>
> ```js
> 希望将一个对象拷贝给另外一个对象使用
> 语法：
> 	$.extend([deep], target, object1, [objectn])
> 注意;
>    	1. deep，默认值是false，浅拷贝。true代表深拷贝
>  	   浅拷贝，如果遇到复杂数据类型，是将复杂数据类型的地址拷贝给目标对象的
>           深拷贝，拷贝的就是对象，没有拷贝地址
>     2. target，要将对象拷贝给哪个对象
>     
>     3. object1,当前要被拷贝的对象
> ```
>
> 2. ##### jQuery多库共存
>
> ```js
> 为了避免其他js文件中和jQuery文件中的 '$'符号冲突
> 
> 方式一：
> 	使用 jQuery 替代 '$'
> 
> 方式二：
>     用户完全自定义
>  var  test = jQuery.noConflict();
> 		 test.each()
> ```
>
> 3. ##### jQuery插件介绍
>
> ```js
>    1. 瀑布流插件   ( http://www.htmleaf.com/  )
>    2. 懒加载插件: EasyLazyload (http://www.jq22.com/jquery-info11629)
>    3. 全屏滚动插件 (https://browser.360.cn/ee/)
> 	（ gitHub： https://github.com/alvarotrigo/fullPage.js
>    中文翻译网站： http://www.dowebok.com/demo/2014/77/）
> 
> jQuery 插件库  http://www.jq22.com/     
> ```
>
> 4. todoList综合案例
>
> ```js
> http://www.todolist.cn/
> ```
>
> 1. ##### JSON.stringify()  转化为json格式的字符串
>
> 2. ##### JSON.parse()  将json格式的字符串转化为对象
>
> 3. ##### 删除数组中的值 ary.splice(startindex, length)；

