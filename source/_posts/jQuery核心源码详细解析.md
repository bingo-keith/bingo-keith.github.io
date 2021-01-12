---

title: jquery核心代码详细解析
date: 2018/2/20 21:10:08
tags: jquery 源码 框架 库 源代码 学习

---

一直以来，工作中用到的工具就是jquery，之前也看过源码，但是没有对jquery系统地学习、总结过，虽然现在前端新的技术层出不穷，但jquery里的设计思想和编码技巧还是需要学习、借鉴的，这次趁着过年的时间抽空把jquery源码系统地学习了一遍，由于时间不是很充裕，所以只是把核心源码分析了一下：

<!-- more -->

```
( function ( window, undefined ){//沙箱模式
  //在ie中undefined可以被重写，所以形参把undefined传进来实参不传，以保证undefined本身不会被重写

  var  arr = [], push = arr.push, slice = arr.slice;

  //调用原型的init方法，返回DOM元素伪数组
  function $ ( html ){
    return new $.fn.init( html );
  }

  //给原型添加init方法，伪数组
  //给prototype取个别名fn
  $.fn = $.prototype = {
    constructor: $,//替换原型要手动添加constructor
    length: 0,
    selector: '',
    type: '$',
    init: function( html ){//初始化
      //如果参数为空  null undefined
      if ( html == null || html === '' ) {
        this.events = {};
        return;
      };

      //如果参数是函数(入口函数)
      if ( typeof html === 'function' ) {
        var oldFn = window.onload;
        if ( typeof oldFn === 'function' ) {
          window.onload = function(){
            oldFn();
            html();
          }
        }else{
          window.onload = html;
        }
        return;
      };

      //如果参数是字符串
      if ( $.isString( html )) {
        if ( /^</.test( html )) {
          push.apply( this, parseHTML( html ));
        }else{//如果是标签选择器
          push.apply( this, $.select( html ));
          this.selector = html;
        }
      }

      //如果是$对象,将传入的$对象所有元素都加到this中
      if ( html && html.type === '$' ) {
        push.apply( this, html );
        this.selector = html.selector;
        this.events = html.events;
        return this;
      };

      //如果是DOM对象
      if ( html.nodeType ) {
        this[ 0 ] = html;
        push.call( this[ 0 ], html );
      };
      this.events = {};
    },
    toArray: function(){
      return slice.call( this, 0 );
    },
    get: function( index ){
      if ( index === undefined ) {
        return this.toArray();
      };
      return this[ index ];
    },
    eq: function( num ){
      var dom;
      if ( num >= 0 ) {
        dom = this.get( num );
      }else{
        dom = this.get( this.length + num );
      }
      return $( dom );
      // return this.constructor( dom );//包装成一个$对象
    },
    each: function( func ){
      return $.each( this, func );
    },
    map: function( func ){
      return $.map( this, func );
    }
  };
  //让init拥有$.prototype的方法，就让原型与$.prototype的指向相同
  //这样就有appendTo方法
  $.fn.init.prototype = $.fn;

  //添加自定义可扩展的方法extend(混入式继承)
  $.extend = $.fn.extend = function ( obj ){
    for ( var k in obj ){
      this[ k ] = obj[ k ];
    }
  };

  //自己封装的基本、并集、后代选择器引擎
  var select3 = (function(){
    //尝试用数组自带push方法，如果没有的话就自己定义一个
    try {
      var div = document.createElement( 'div' );
      div.innerHTML = '<p></p>';
      var arr = [];
      push.apply( arr, div.getElementsByTagName( 'p' ));
    }catch ( e ){//如果不支持push方法就自己定义
      push = {
        apply: function ( array1, array2 ){
          for (var i = 0; i < array2.length; i++) {
            //把array2添加到array1中
            array1[ array1.length++ ] = array2[ i ];
          };
        }
      }
    }

    //正则表达式
    var rnative = /\{\s*\[native/;//匹配浏览器支持就返回{ [native code] }
    var rtrim = /^\s+|\s+$/;//匹配字符串两端空格
    //        ?:匹配但不捕获      id:1      class:2   *:3  tag:4
    var rbaseselector = /^(?:\#([\w\-]+)|\.([\w\-]+)|(\*)|(\w+))$/;

    //浏览器能力检测
    var support = {};
    support.qsa = rnative.test( document.querySelectorAll + '');//转换字符串
    support.getElementsByClassName = rnative.test( document.getElementsByClassName + '');
    support.trim = rnative.test( String.prototype.trim + '');

    //通过类名获取元素的兼容性写法
    function getByClassName( className, node ){
      node = node || document;
      var allElem, res = [], i;
      if ( support.getElementsByClassName ){//如果浏览器支持
        return node.getElementsByClassName( className );
      }else{//不支持
        allElem = document.getElementsByTagName( '*' );
        for (i = 0; i < allElem.length; i++) {
          if (( ' ' + allElem[ i ].className + ' ' ).indexOf( ' ' + className + ' ' ) > -1 ) {
            res.push( allElem[ i ] );
          };
        };
        return res;
      }
    }

    //自定义实现trim方法(去两端空格)
    function myTrim( str ){
      if ( support.trim ) {
        return str.trim();
      }else{
        return str.replace( rTrim, ' ' );
      }
    }

    //自定义indexOf方法
    function myIndexOf ( array, search, startIndex ){
      startIndex = startIndex || 0;//设置开始查找索引默认值
      if ( support.indexOf ) {
        return array.IndexOf( search, startIndex );
      }else{
        for (var i = 0; i < array.length; i++) {
          if ( array[ i ] === search ) {
            return i;
          };
        };
        return -1;
      }
    }

    //数组去重
    var unique = function( array ){
      var resArray = [], i = 0;
      for (; i < array.length; i++) {
        if ( myIndexOf( resArray, array[ i ]) === -1 ) {
          resArray.push( array[ i ]);
        };
      };
      return resArray;
    }

    //基本选择器
    function basicSelect( selector, node ){
      node = node || document;
      var m, res;
      if ( m = rbaseselector.exec( selector )) {
        if ( m[ 1 ]) {//id
          res = document.getElementById( m[ 1 ] );
          return res ? [ res ] : [];//如果没获取到就返回空数组
        }else if ( m[ 2 ]) {//class
          return getByClassName( m[ 2 ], node );
        }else if ( m[ 3 ]) {//*
          return node.getElementsByTagName( m[ 3 ] );
        }else{//tag
          return node.getElementsByTagName( m[ 4 ] );
        }
      };
    }

    //后代选择器
    function select2( selector, results ){
      results = results || [];//如果没传该参数就默认为[]
      var selectors = selector.split( ' ' );//分割成数组
      var arr = [], node = [ document ], i, j;
    /**假设'div p .c' 外层循环控制选择器的迭代次数(3次),node = arr控制内层循环的次数
     *第一次在document下查找div并把数组添加到node中
     *arr.push.apply(arr,basicSelect('div','document'));
     *==>arr:['div']==>node:['div']==>arr:[]
     *第二次在node成员div下查找p并把p添加到node中
     *arr.push.apply(arr,basicSelect('p','div'));
     *==>arr:['p']==>node:['p']==>arr:[]
     *第三次在node成员div或者p下查找.c并把.c添加到node中
     *arr.push.apply(arr,basicSelect('.c','p'));
     *==>arr:['.c']==>node:['.c']==>arr:[]
     */
      for (i = 0; i < selectors.length; i++) {
        for (j = 0; j < node.length; j++) {
          arr.push.apply( arr, basicSelect( selectors[ i ], node[ j ] ));
        };
        //下面重置的两句代码的位置影响到性能问题
        node = arr;//把当前获取的元素保存到node中
        arr = [];//初始化arr
      };
      results.push.apply( results, node );//把获取的元素添加到结果中
      return results;
    }

    //核心功能
    function select( selector, results ){
      results = results || [];
      var m, temp, selectors, subSelector, i;
      //如果实参不是字符串
      if ( typeof selector != 'string' ) return results;
      //判断是否支持qsa
      if ( support.qsa ) {
        results.push.apply( results, document.querySelectorAll( selector ) );
      }else{
        selectors = selector.splitL( ',' );//用逗号分割传入的参数
        for (i = 0; i < selectors.length; i++) {//遍历选择器
          subSelector = myTrim( selectors[ i ]);//去掉两端空格
          if ( rbaseselector.test( subSelector )) {//如果参数是基本选择器
            //通过匹配捕获把获取到的元素添加到数组中
            results.push.apply( results, basicSelect( subSelector ));
          }else{//如果是后代选择器
            select2( subSelector, results );//执行后代选择器
          }
        };
      }
      return unique( results );//把获取的重复的元素去掉
    }
    return select;//返回select方法体，供函数外部使用
  })();

  //把选择器引擎里的核心函数API暴露出来，在需要的时候可以使用第三方的选择器引擎
  //给$对象添加自定义属性select指向已声明的选择器引擎
  $.select = select3;

  //给构造函数$添加静态方法。 实例方法
  $.extend({
    getStyle: function(){
      if ( o.currentStyle ) {
        return o.currentStyle[ name ];
      }else{
        return window.getComputedStyle( o )[ name ];
      }
    },
    isString: function( data ){
      return typeof data === 'string';
    },
    each: function( arr, func ){
      var i;
      //在es5中还引入了Array.isArray的方法专门来判断数组
      if ( arr instanceof Array || arr.length >= 0 ) {
        for ( i = 0; i < arr.length; i++ ) {
          func.call( arr[ i ], i, arr[ i ] );
        };
      }else{
        for ( i in arr ){
          func.call( arr[ i ], i, arr[ i ] );
        }
      }
      return arr;//返回，链式编程
    },
    map: function( arr, func ){
      var res = [], i, tmp;
      if ( arr instanceof Array || arr.length >= 0 ) {
         for (i = 0; i < arr.length; i++) {
            tmp = func.call( arr[ i ], arr[ i ], i );
            if ( tmp != null ) {
               res.push( tmp );
            };
         };
      }else{
         for ( i in arr ){
            tmp = func.call( arr[ i ], arr[ i ], i );
            if ( tmp != null ) {
               res.push( tmp );
            };
         };
      };
    return res;
    },
    prependChild: function ( parent, element ){
      parent.insertBefore( element, parent.firstChid );
    }
  });

  //DOM操作的方法，将字符串转换为DOM对象的函数
  var parseHTML = (function(){
    var div = document.createElement( 'div' );
    function parseHTML ( html ){
      div.innerHTML = html;
      var res = [], i;
      for ( i = 0; i < div.childNodes.length; i++ ) {
        res.push( div.childNodes[ i ] );
      };
      div.innerHTML = '';
      return res;
    }
    return parseHTML;
  })();

  //给构造函数$添加静态方法
  $.extend({
    isString: function( data ){
      return typeof data === 'string';
    },
    each: function( arr, func ){
      var i;
      // 在 ES5 中还引入了 Array.isArray 的方法专门来判断数组
      if ( arr instanceof Array || arr.length >= 0 ) {
         for ( i = 0; i < arr.length; i++ ) {
            func.call( arr[ i ], i, arr[ i ] );
         }
      }else{
         for ( i in arr ){
            func.call( arr[ i ], i, arr[ i ] );
         }
      }
      return arr;//返回原数组/对象
    },
    map: function( arr, func ){
      var res = [], i, tmp;
      if ( arr instanceof Array || arr.length >= 0 ) {
         for (i = 0; i < arr.length; i++) {
            tmp = func.call( arr[ i ], arr[ i ], i );
            if ( tmp != null ) {
               res.push( tmp );
            };
         };
      }else{
         for ( i in arr ){
            tmp = func.call( arr[ i ], arr[ i ], i );
            if ( tmp != null ) {
               res.push( tmp );
            };
         };
      };
      return res;
    },
    prependChild: function ( parent, element ){
      parent.insertBefore( element, parent.firstChild );
    },
    getStyle: function( o, name ){
      if ( o.currentStyle ) {
        return o.currentStyle[ name ];
      }else{
        return window.getComputedStyle( o )[ name ];
      }
    }
  });

  //DOM操作的方法，将字符串转换为DOM对象的函数
  var parseHTML = (function (){
    var div = document.createElement( 'div' );
    function parseHTML ( html ) {
      div.innerHTML = html;
      var res = [];//保存到数组中方便调取各个节点
      for (var i = 0; i < div.childNodes.length; i++) {
        res.push( div.childNodes[ i ] );
      };
      div.innerHTML = '';
      return res;
    }
    return parseHTML;
  })();

  //实例方法
  $.fn.extend({
    appendTo: function ( selector ){
      //把selector包装成$对象（伪数组）
      var iObj = this.constructor( selector );
      //利用新对象保存数据然后改变this指向
      var newObj = this.constructor();
      console.log(this);//指向前面添加的的span元素(伪数组)
      // A appendTo B     this指向A
      for ( var i = 0; i < this.length; i++ ) {
        for ( var j = 0; j < iObj.length; j++ ) {
          //最后一次会多克隆一次，所以最后一次就不克隆
          var temp =
            i == this.length - 1 && j == iObj.length - 1
              ? this[ i ]
              : this[ i ].cloneNode( true );
          //改变this指向实现链式编程
          [].push.call( newObj, temp );
          iObj[ j ].appendChild( temp );
        };
      };
      return newObj;
    },
    append: function ( selector ){
      //$( selector ).appendTo( this );//简化版
      var iObj = this.constructor( selector );
      // B append A this 的指向B
      for (var i = 0; i < iObj.length; i++) {
        for (var j = 0; j < this.length; j++) {
          var temp =
            i == this.length - 1 && j == iObj.length - 1
              ? iObj[ i ]
              : iObj[ i ].cloneNode( true );
          this[ j ].appendChild( temp );
        };
      };
      return this;
    },
    prependTo: function ( selector ){
      //把selector包装成$对象（伪数组）
      var iObj = this.constructor( selector );
      //利用新对象保存数据然后改变this指向
      var newObj = this.constructor();//$.fn.constructor也行
      for (var i = 0; i < this.length; i++) {
        for (var j = 0; j < iObj.length; j++) {
          //最后一次会多克隆一次，所以最后一次就不克隆
          var temp =
            i == this.length - 1 && j == iObj.length - 1
              ? this[ i ]
              : this[ i ].cloneNode( true );
          //改变this指向实现链式编程
          [].push.call( newObj, temp );
          prependChild( iObj[ j ], temp );
        };
      };
      return newObj;
    },
    prepend: function ( selector ){
      this.constructor( selector ).prependTo( this );
      return this;
    },
    on: function( type, fn ){
      if ( !this.events[ type ]) {//如果事件对象不存在就置空
        this.events[ type ] = [];
        var that = this;//指向的是该数组
        this.each( function() {
          var self = this;//this指向每一个DOM对象
          var f = function ( e ){
            //把事件对象e传进每个绑定的事件里
            for (var i = 0; i < that.events[ type ].length; i++) {
              that.events[ type ][ i ].call( self, e );
            };
          }
          if ( this.addEventListener ) {
            this.addEventListener( type, f );
          }else{
            this.attachEvent( 'on' + type, f );//兼容ie6，7，8浏览器
          }
        });
      };
      this.events[ type ].push( fn );//给events对象type属性(数组)添加传入的事件
      return this;
    },
    off: function( type, fn){
      var arr = this.events[ type ];//事件数组
      if ( arr ) {
        for (var i = 0; i < arr.length; i++) {
          if ( arr[ i ] == fn ) {
            break;
          };
        };
        if ( i != arr.length ) {
          arr.aplice( i, 1 );
        };
      };
      return this;
    },
    css: function( option ){
      var len = arguments.length, args = arguments;
      if ( len === 2 ) {//两个参数
        if ( $.isString( args[ 0 ] ) && $.isString( args[ 1 ] ) ) {
          return this.each( function(){
            this.style[ args[ 0 ] ] = args[ 1 ];
          })
        };
      }else if ( len === 1 ) {//一个参数
        if ( $.isString( option ) ) {//参数为字符串
          return this[ 0 ].style[ option ] || $.getStyle( this[ 0 ], option );
        }else if ( typeof option == 'object') {//参数为对象
          return this.each( function(){
            for ( var k in option ){
              this.style[ k ] = option[ k ];
            }
          })
        };
      };
      return this;
    },
    addClass: function( name ){
      return this.each( function (){
        var classTxt = this.className;
        if ( classTxt ) {
          if ( ( ' ' + classTxt + ' ' ).indexOf( ' ' + name + ' ' ) === -1 ) {
            this.className += ' ' + name;
          }else{

          }
        }else{
          this.className = name;
        }
      })
    },
    removeClass: function ( name ){
      return this.each( function(){
        var classTxt = ' ' + this.className + ' ';
        var rclassName = new RegExp( ' ' + name + ' ', 'g' );
        this.className = classTxt.replace( rclassName, ' ' ).replace( /\s+/g, ' ' ).trim();
      });
    },
    hasClass: function( name ){
      for ( var i = 0; i < this.length; i++ ) {
        if ( ( ' ' + this[ i ].className + ' ' ).indexOf( ' ' + name + ' ' ) != -1 ) {
          return true;
        };
      };
      return false;
    },
    toggleClass: function( name ){
      if ( this.hasClass( name )) {
        this.removeClass( name );
      }else{
        this.addClass( name );
      }
      return this;
    },
    attr: function ( name, value ){
      if ( value ) {//设置属性
        if ( $.isString( name ) && $.isString( value ) ) {
          return this.each( function(){
            this.setAttribute( name, value );
          })
        };
      }else{//获取属性
        if ( $.isString( name ) ) {
          return this[ 0 ].getAttribute( name );
        };
      }
      return this;
    },
    prop: function ( name, value ){
      if ( value ) {
        if ( $.isString( name ) && $.isString( value ) ) {
          return this.each( function(){
            this[ name ] = value;
          });
        };
      }else{
        if ( $.isString( name ) ) {
          return this[ 0 ][ name ];
        };
      }
      return this;
    },
    val: function( value ){
      return this.attr( 'value', value);
    },
    html: function( html ){
      return this.prop( 'innerHTML', html );
    },
    text: function( txt ){
      if ( txt ) {
        return this.each( function(){
          this.innerHTML = '';
          this.appendChild( document.createTextNode( txt + '' ));
        })
      }else{
        var arr = [];
        getTxt( this[ 0 ], arr );
        return arr.join( ' ' );
      }
      return this;

      function getTxt( node, list ){
        var arr = node.childNodes;
        for (var i = 0; i < arr.length; i++) {
          if ( arr[ i ].nodeType === 3 ) {
            list.push( arr[ i ].nodeValue );
          };
          if ( arr[ i ].nodeType === 1 ) {
            getTxt( arr[ i ], list );
          };
        };
      }
    }
  });

  //直接绑定事件的封装
  $.each(( "blur focus focusin focusout load resize scroll unload click dblclick " +
  "mousedown mouseup mousemove mouseover mouseout mouseenter mouseleave " +
  "change select submit keydown keypress keyup error contextmenu" ).split( ' ' ), function( i, v ){//v表示事件的名称
    $.fn[ v ] = function( fn ){
      this.on( v, fn );
      return this;
    }
  });


window.$ = $;//暴露$

} )( window );
```