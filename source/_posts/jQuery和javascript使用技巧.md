---

title: jQuery和javascript使用技巧
date: 2017/9/15 22:17:43  
tags: javascript jQuery API 技巧

---

把平时不常用或很少用到的但是比较实用的方法收集整理出来，方便查阅回顾。

<!-- more -->

# jQuery实现全选和反选

> 该效果使用了prop方法，利用第二个参数是一个函数，返回一个布尔值，通过该值取反实现全选和反选功能。

```
$("input[type='button']").on('click',function(){
  $("input[type='checkbox']").prop("checked",function(i,v){
    console.log(i+','+v);
    return !v
  });
})
```


# jQuery 实现给元素切换不同类名

> 该效果使用了toggleClass方法，利用该方法的参数可以为一个函数，返回值作为每次切换的类名，此处利用index方法查到当前对象的索引并在函数返回值里调用来实现切换不同的类名。

```
$("input[type='button']").on('click',function(){
  $("input[type='checkbox']").toggleClass(function(){
    return 'item'+$(this).index();
  })
})
```


# jQuery 给元素添加样式并让其逐步变化

> 该效果使用了jQuery的css方法、width方法和height方法，后两种方法返回选择元素的宽高，然后通过css方法设置元素的宽高，其中width方法和height方法可以设置一个函数，返回元素的宽高，其中函数有两个参数，可选，一个是该元素的索引位置，一个是该元素的当前值。

```
$("input[type='button']").on('click', function() {
  $('div').css({
    width:function( index, oldvalue ){
      return parseFloat( oldvalue ) * 1.2;
    },
    height:function( index, v ){
      return parseFloat( oldvalue ) * 1.2;
    }
  })
});
```


# jQuery 替换元素的标签和内容为指定的标签和内容

> 该效果使用了jQuery的replaceWith方法，该方法有两个参数，一个是该元素的索引值，一个是该元素的text(input中为空)。

```
$('p').replaceWith(function( i, v ){
  return '<\b>'+ $(this).text() + i +',' +  v +'</\b><\br>'
});
```


# javascript中位运算符~的妙用

**按位非运算符，简单的理解就是改变运算数的符号并减去1，当然，这是只是简单的理解能转换成number类型的数据。**

> 项目中用到了indexOf的方法，该方法返回指定索引值，如[1,2,3].indexOf(2)返回的就是2在数组中的索引1，该方法返回值有一个规律，如果该元素存在于指定数组中，返回数组中该元素的索引值，如果不存在的话一律返回-1。该方法的应用场景很多，比如判断类中是否存在指定类，得出布尔值，根据该值写业务逻辑；再比如此次项目中遇到的，一个多选select标签，根据接口返回的数据（组）判断是否被选中，其中利用到了prop方法、attr方法、、text方法、indexof方法、map方法和html方法，同样也运用了~运算符，非常灵活地实现了项目需求。~~运算符可以简单的理解为去掉小数保留整数，不遵循四舍五入个规则。

```
<select style="width:200px" id="sel1">
  <option value="1">1</option>
</select>
<select style="width:200px;height: 200px;" id="sel2" multiple>
</select>
<select style="width:200px;height: 200px;" id="sel3" multiple>
</select>
<script>
  $('<option />').attr('value','a').text('测试').appendTo('#sel1');
  var arr = [1,2,3,4,5];
  var arr2 = [1,3,5,7,9,11,13,15];

  //the following codes have same effects
  //method one
  $('#sel2').html(arr.map(function(v,i){
    return $('<option/>').attr('value','v').text('测试'+v).prop('selected',~arr2.indexOf(v) ? false : true);
  }))
  //method two
  $('#sel3').html(arr.map(function(v,i){
    return $('<option/>').attr('value','v').text('测试'+v).prop('selected',arr2.indexOf(v) == -1 ? false : true);
  }))
</script>
```

# is()方法比你想象的更为强大
**根据选择器、DOM元素或 jQuery 对象来检测匹配元素集合，如果其中至少有一个元素符合这个给定的表达式就返回true。**

```
// First, cache the element into a variable:
var elem = $('#elem');

// Is this a div?
elem.is('div') && log("it's a div");

// Does it have the bigbox class?
elem.is('.bigbox') && log("it has the bigbox class!");

// Is it visible? (we are hiding it in this example)
elem.is(':not(:visible)') && log("it is hidden!");

// Animating
elem.animate({'width':200},1);

// is it animated?
elem.is(':animated') && log("it is animated!");
```

# jQuery方法$()实际上是拥有两个参数的，你知道第二个参数的作用吗？
**注意：`$('selector1,selector2')`和`$('selector',contextmenu || attribute Object)`的区别**

```
// Select an element. The second argument is context to limit the search
// You can use a selector, jQuery object or dom element

$('li','#firstList').each(function(){
    log($(this).html());
});

log('-----');

// Create an element. The second argument is an
// object with jQuery methods to be called

var div = $('<div>',{
    "class": "bigBlue",
    "css": {
        "background-color":"purple"
    },
    "width" : 20,
    "height": 20,
    "animate" : {   // You can use any jQuery method as a property!
        "width": 200,
        "height":50
    }
});

div.appendTo('#result');
```

# 禁用网页右键菜单

```
<script>
    // Prevent right clicking on this page
    $(function(){
        $(document).on("contextmenu",function(e){
            e.preventDefault();
        });
    });
</script>
```

# 更方便的分解URL

**也许你会使用正则表达式来解析URL，但这绝对不是一种好的方法，我们可以借用a标签来实现它。**

```

// You want to parse this address into parts:
var url = 'http://tutorialzine.com/books/jquery-trickshots?trick=12#comments';

// The trickshot:
var a = $('<a>',{ href: url });

log('Host name: ' + a.prop('hostname'));
log('Path: ' + a.prop('pathname'));
log('Query: ' + a.prop('search'));
log('Protocol: ' + a.prop('protocol'));
log('Hash: ' + a.prop('hash'));

```

# 当做复杂的修改时要分离元素

** 修改一个dom元素要求网页重绘，这个代价是高昂的，所以如果你想要再提高性能，就可以尝试着当对一个元素进行大量修改时先从页面中分离这个元素，修改完之后再添加到页面。 **

```
// Modifying in place
var elem = $('#elem');

timer('In place');

for(i=0; i &lt; iterations; i++){

    elem.width(Math.round(100*Math.random()));
    elem.height(Math.round(100*Math.random()));

}

timer_result('In place');

var parent = elem.parent();

// Detaching first
timer('Detached');

elem.detach();

for(i=0; i &lt; iterations; i++){

    elem.width(Math.round(100*Math.random()));
    elem.height(Math.round(100*Math.random()));

}

elem.appendTo(parent);

timer_result('Detached');
```
# 创建自己习惯的事件

** 你可以使用on()方法创建自己喜欢的事件名称，然后通过trigger来触发。举例如下： **

```
var button1 = $('#button1'),
    button2 = $('#button2'),
    button3 = $('#button3'),
    clear = $('#clear'),
    div = $('#eventDiv');

div.on({
    jump : function(){
        alert('Jumped!');
    },

    punch : function(e,data){
        alert('Punched '+data+'!');
    },

    click : function(){
        alert('Simulated click!');
    }

});

button1.click(function(){
    div.trigger('jump');
});

button2.click(function(){
    // Pass data along with the event
    div.trigger('punch',['hard']);
});

button3.click(function(){
    div.trigger('click');
});

clear.click(function(){
    //some clear code
});
```

# 使用延迟简化你的Ajax请求

** 延迟(deferreds)是一个强大的工具。jQuery对于每一个Ajax请求都会返回一个deferred对象。 `deferred.done()`方法接受一个或多个参数，所有这些都参数可以是一个单一的函数或一个函数数组。当Deferred（延迟）解决时，doneCallbacks被调用。回调是依照他们添加的顺序执行。一旦`deferred.done()`返回Deferred（延迟）对象，Deferred（延迟）可以链接其它的延迟对象，包括增加额外的`.done()`方法。下面这样就会使你的代码更易读： **

```
// This is equivalent to passing a callback as the
// second argument (executed on success):

$.get('assets/misc/1.json').done(function(r){
    log(r.message);
});

// Requesting a file that does not exist. This will trigger
// the failure response. To handle it, you would normally have to
// use the full $.ajax method and pass it as a failure callback,
// but with deferreds you can can simply use the fail method:

$.get('assets/misc/non-existing.json').fail(function(r){
    log('Oops! The second ajax request was "' + r.statusText + '" (error ' + r.status + ')!');
});
```

# 平行的运行多个Ajax请求

** 当我们需要发送多个Ajax请求时，相反于等待一个发送结束再发送下一个，我们可以平行地发送来加速Ajax请求发送。 **

```
// The trick is in the $.when() function:

$.when($.get('assets/misc/1.json'), $.get('assets/misc/2.json')).then(function(r1, r2){
    log(r1[0].message + " " + r2[0].message);
});
```

