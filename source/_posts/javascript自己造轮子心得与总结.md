---

title: javascript造轮子心得与总结
date: 2017/9/10 16:35:05 
tags: javascript js 造轮子 插件 总结

---

> 造了一个忒简单的轮子。。。。。

<!-- more -->

```
<button type="button" id="btn">点击</button>
<script>
  /*
  *  本次只是写了一个基于jquery和bootbox的一个最简单的插件，学习领会自己造轮子的原理和其中的乐趣。
  *  总结：
  *  1、插件外面用自调用函数包裹，防止变量污染
  *  2、插件外面传入想要传入的对象，如jquery($)和全局window，以便可以采用$.xxx或window.xxx来进行调用(window可省略)
  *  3、如果要用javascript来造轮子，可以借鉴jquery的方法，方法内部返回该实例，这样调用时就不用实例化了(new关键字)
  *  4、在匿名函数前加!、+、-、()、=转换成函数表达式，然后就可以直接通过()进行调用了，最好用()转换，因为其他的几个运算符会对函数返回结果进行运算造成不可预料的结果
  *  5、通过jquery的extend方法设置参数的默认值，同时可以利用传入的同名参数对默认值进行覆盖，注意引用类型要深度克隆，合并后的对象（第二个参数）最好是空对象
  */
  +function($,win){
    var htmlStr = '<div style="color:blue">测试编写不能覆盖的插件模板</div>';
    $.demo = $.demo || function(options){
      var setting = $.extend(true,{}, {
        size: 'middle',
        backdrop: false,
        title: '测试插件的默认标题',
        message: '',
        buttons: {
          ok: {
            label:'确认',
            className:'btn-primary',
            callback:function(){bootbox.alert('测试默认的确认按钮回调')}
          },
          cancel:{
            label:'取消',
            className:'btn-default',
            callback:function(){bootbox.alert('测试默认的取消按钮回调')}
          }
        }
      },options);
      setting.message = htmlStr;//覆盖合并参数时的默认值
      var dlg = bootbox.dialog(setting);
      return dlg;
    }
  }(jQuery, window);

  $('#btn').click(function(){
    $.demo({
      size:'small',
      backdrop:true,
      title:'覆盖了插件的标题',
      message:'<div style="red">测试没有覆盖默认内容</div>',
      buttons:{
        ok:{
          label:'覆盖默认按钮',
          callback:function(){bootbox.alert('测试覆盖默认确认按钮')}
        }
      }
    });
  })
</script>
```