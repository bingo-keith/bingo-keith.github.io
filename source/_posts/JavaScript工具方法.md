---

title: JavaScript工具方法
date: 2017/8/5 17:53:48
tags: javascript　js　工具 方法 前端

---

### 工作和平时收集整理的一些实用的工具方法，保存下来，以备不时之需。
<!-- more -->

```js
<script type="text/javascript">
var myTools = {
  /**
   * [trim 去除空格]
   * @param  {[string]} str  [原始字符串]
   * @param  {[number]} type [1-所有空格  2-前后空格  3-前空格 4-后空格]
   * @return {[string]}      [返回处理后的值]
   */
  trim: function(str, type) {
    switch (type) {
      case 1:
        return str.replace(/\s+/g, "");
      case 2:
        return str.replace(/(^\s*)|(\s*$)/g, "");
      case 3:
        return str.replace(/(^\s*)/g, "");
      case 4:
        return str.replace(/(\s*$)/g, "");
      default:
        return str;
    }
  }
  /**
   * [changeCase 字母大小写切换]
   * @param  {[string]} str  [原始字符串]
   * @param  {[number]} type [1-首字母大写  2-首页母小写  3-大小写转换  4-全部大写  5-全部小写]
   * @return {[string]}      [返回处理后的值]
   */
  ,changeCase: function(str, type) {
    function ToggleCase(str) {
      var itemText = ""
      str.split("").forEach(
        function(item) {
          if (/^([a-z]+)/.test(item)) {
            itemText += item.toUpperCase();
          } else if (/^([A-Z]+)/.test(item)) {
            itemText += item.toLowerCase();
          } else {
            itemText += item;
          }
        });
      return itemText;
    }
    switch (type) {
      case 1:
        return str.replace(/^(\w)(\w+)/, function(v, v1, v2) {
          return v1.toUpperCase() + v2.toLowerCase();
        });
      case 2:
        return str.replace(/^(\w)(\w+)/, function(v, v1, v2) {
          return v1.toLowerCase() + v2.toUpperCase();
        });
      case 3:
        return ToggleCase(str);
      case 4:
        return str.toUpperCase();
      case 5:
        return str.toLowerCase();
      default:
        return str;
    }
  }
  /**
   * [repeatStr 字符串循环复制]
   * @param  {[string]} str   [原始字符串]
   * @param  {[number]} count [次数]
   * @return {[string]}       [返回处理后的值]
   */
  ,repeatStr: function(str, count) {
    var text = '';
    for (var i = 0; i < count; i++) {
        text += str;
    }
    return text;
  }
  //字符串替换
  /**
   * [replaceAll 字符串替换]
   * @param  {[string]} str              [原始字符串]
   * @param  {[string RegExp]} AFindText [查找字符串]
   * @param  {[string]} ARepText         [替换字符串]
   * @return {[string]}                  [返回处理后的值]
   */
  ,replaceAll: function(str, AFindText, ARepText) {　　　
    raRegExp = new RegExp(AFindText, "g");　　　
    return str.replace(raRegExp, ARepText);
  }
  /**
   * [replaceStr 替换指定字符串]
   * @param  {[string]} str             [字符串]
   * @param  {[array]} regArr           [字符格式]
   * @param  {[number]} type            [替换方式]
   * @param  {[string]} ARepText        [替换的字符(默认*)]
   * @return {[string]}                 [返回处理后的值]
   */
  ,replaceStr: function(str, regArr, type, ARepText) {
    var regtext = '',
        Reg = null,
        replaceText = ARepText || '*';
    //replaceStr('18819322663',[3,5,3],0)
    //188*****663
    //repeatStr是在上面定义过的（字符串循环复制），大家注意哦
    if (regArr.length === 3 && type === 0) {
      regtext = '(\\w{' + regArr[0] + '})\\w{' + regArr[1] + '}(\\w{' + regArr[2] + '})'
      Reg = new RegExp(regtext);
      var replaceCount = this.repeatStr(replaceText, regArr[1]);
      return str.replace(Reg, '$1' + replaceCount + '$2')
    }
    //replaceStr('asdasdasdaa',[3,5,3],1)
    //***asdas***
    else if (regArr.length === 3 && type === 1) {
      regtext = '\\w{' + regArr[0] + '}(\\w{' + regArr[1] + '})\\w{' + regArr[2] + '}'
      Reg = new RegExp(regtext);
      var replaceCount1 = this.repeatStr(replaceText, regArr[0]);
      var replaceCount2 = this.repeatStr(replaceText, regArr[2]);
      return str.replace(Reg, replaceCount1 + '$1' + replaceCount2)
    }
    //replaceStr('1asd88465asdwqe3',[5],0)
    //*****8465asdwqe3
    else if (regArr.length === 1 && type == 0) {
      regtext = '(^\\w{' + regArr[0] + '})'
      Reg = new RegExp(regtext);
      var replaceCount = this.repeatStr(replaceText, regArr[0]);
      return str.replace(Reg, replaceCount)
    }
    //replaceStr('1asd88465asdwqe3',[5],1,'+')
    //"1asd88465as+++++"
    else if (regArr.length === 1 && type == 1) {
      regtext = '(\\w{' + regArr[0] + '}$)'
      Reg = new RegExp(regtext);
      var replaceCount = this.repeatStr(replaceText, regArr[0]);
      return str.replace(Reg, replaceCount)
    }
  }
  /**
   * [checkType 检测字符串类型]
   * @param  {[string]} str  [检测的字符串]
   * @param  {[string]} type [检测类型:email,phone,tel,number,english,chinese,lower,upper]
   * @return {[boolean]}     [返回布尔值]
   */
  ,checkType: function(str, type) {
    switch (type) {
      case 'email':
        return /^[\w-]+(\.[\w-]+)*@[\w-]+(\.[\w-]+)+$/.test(str);
      case 'phone':
        return /^1[3|4|5|7|8][0-9]{9}$/.test(str);
      case 'tel':
        return /^(0\d{2,3}-\d{7,8})(-\d{1,4})?$/.test(str);
      case 'number':
        return /^[0-9]$/.test(str);
      case 'english':
        return /^[a-zA-Z]+$/.test(str);
      case 'chinese':
        return /^[\u4E00-\u9FA5]+$/.test(str);
      case 'lower':
        return /^[a-z]+$/.test(str);
      case 'upper':
        return /^[A-Z]+$/.test(str);
      default:
        return false;
    }
  }
  /**
   * [checkPwd 检测密码强度]
   * @param  {[string]} str [检测的密码]
   * @return {[number]}     [返回密码强度，最大为4]
   */
  ,checkPwd: function(str) {
    var nowLv = 0;
    if (str.length < 6) {
      return nowLv
    };
    if (/[0-9]/.test(str)) {
      nowLv++
    };
    if (/[a-z]/.test(str)) {
      nowLv++
    };
    if (/[A-Z]/.test(str)) {
      nowLv++
    };
    if (/[\.|-|_]/.test(str)) {
      nowLv++
    };
    return nowLv;
  }
  /**
   * [countStr 查找字符串]
   * @param  {[string]} str      [原始字符串]
   * @param  {[string]} strSplit [查询的字符串]
   * @return {[number]}          [出现的次数]
   */
  ,countStr: function(str, strSplit) {
    return str.split(strSplit).length-1;
  }
  /**
   * [unique 数组去重]
   * @param  {[array]}  array       [原始数组]
   * @param  {Boolean} isSorted     [是否已排过序]
   * @param  {[function]}  iteratee [重新计算，根据结果去重]
   * @return {[array]}              [新数组]
   */
  ,unique: function(array, isSorted, iteratee) {
    var res = [];
    var seen = [];
    for (var i = 0, len = array.length; i < len; i++) {
        var value = array[i];
        var computed = iteratee ? iteratee(value, i, array) : value;
        if (isSorted) {
            if (!i || seen !== value) {
                res.push(value)
            }
            seen = value;
        }
        else if (iteratee) {
            if (seen.indexOf(computed) === -1) {
                seen.push(computed);
                res.push(value);
            }
        }
        else if (res.indexOf(value) === -1) {
            res.push(value);
        }
    }
    return res;
  }
  /**
   * [sortUnique 数字类型数组排序去重 ES5]
   * @param  {[array]} arr [原始数组]
   * @return {[array]}     [新数组]
   */
  ,sortUnique: function(arr) {
    return arr.concat().sort(function( a, b ){
      return a - b;
    }).filter(function( v, i, arr ){
      return !i || v != arr[i - 1];
    })
  }
  /**
   * [unique2 数组去重 ES6]
   * @param  {[array]} arr [原始数组]
   * @return {[array]}     [新数组]
   */
  ,unique2: function(arr) {
    return [...new Set(arr)];
  }
  /**
   * [upsetArr 数组乱序]
   * @param  {[array]} arr [原始数组]
   * @return {[array]}     [新数组]
   */
  ,upsetArr: function(arr) {
    return arr.sort(function(){ return Math.random() - 0.5});
  }
  /**
   * [getMax 数组求最大值]
   * @param  {[array]} arr         [原始数组]
   * @return {[string number]}     [最大的元素]
   */
  ,getMax: function(arr) {
    return Math.max.apply(null,arr);
  }
  /**
   * [getMax 数组求最小值]
   * @param  {[array]} arr         [原始数组]
   * @return {[string number]}     [最小的元素]
   */
  ,getMin: function(arr) {
    return Math.min.apply(null,arr);
  }
  /**
   * [getSum 数组求和]
   * @param  {[array]} arr [原始数组]
   * @return {[num]}       [和]
   */
  ,getSum: function(arr) {
    var sum = 0;
    for (var i = 0, len = arr.length; i < len; i++) {
      sum += arr[i];
    }
    return sum;
  }
  /**
   * [getAverage 数组求平均值]
   * @param  {[array]} arr [原始数组]
   * @return {[number]}    [平均值]
   */
  ,getAverage: function(arr) {
    var sum = this.getSum(arr);
    var average = sum / arr.length;
    return average;
  }
  /**
   * [getRandom 数组中随机获取元素]
   * @param  {[array]} arr         [原始数组]
   * @return {[string number]}     [随机元素]
   */
  ,getRandomElement: function(arr) {
    return arr[Math.floor(Math.random() * arr.length)];
  }
  /**
   * [getEleCount 数组(字符串)元素出现的次数]
   * @param  {[array string]} obj   [原始数组或字符串]
   * @param  {[string number]} ele  [指定元素]
   * @return {[number]}             [出现的次数]
   */
  ,getEleCount: function(obj, ele) {
    var num = 0;
    for (var i = 0, len = obj.length; i < len; i++) {
      if (ele == obj[i]) {
        num++;
      }
    }
    return num;
  }
  /**
   * [getCount 数组(字符串)出现最多的几次元素和出现次数]
   * @param  {[array string]} arr      [原始数组或字符串]
   * @param  {[number]}       rank     [长度，默认为数组长度]
   * @param  {[number]}       ranktype [排序方式，默认降序]
   * @return {[array]}                 [元素和次数的数组对象]
   */
  ,getCount: function(arr, rank, ranktype) {
    var obj = {}, k, arr1 = [];
    //记录每一元素出现的次数
    for (var i = 0, len = arr.length; i < len; i++) {
      k = arr[i];
      if (obj[k]) {
          obj[k]++;
      }
      else {
          obj[k] = 1;
      }
    }
    //保存结果{el-'元素'，count-出现次数}
    for (var o in obj) {
      arr1.push({el: o, count: obj[o]});
    }
    //排序（降序）
    arr1.sort(function (n1, n2) {
      return n2.count - n1.count
    });
    //如果ranktype为1，则为升序，反转数组
    if (ranktype === 1) {
      arr1 = arr1.reverse();
    };
    var rank1 = rank || arr1.length;
    return arr1.slice(0,rank1);
  }
  /**
   * [upDigit 现金额大写转换函数]
   * @param  {[number]} n [数字金额]
   * @return {[string]}   [大写金额]
   */
  ,upDigit: function(n) {
    var fraction = ['角', '分', '厘'];
    var digit = ['零', '壹', '贰', '叁', '肆', '伍', '陆', '柒', '捌', '玖'];
    var unit = [['元', '万', '亿'],['', '拾', '佰', '仟']];
    var head = n < 0 ? '欠人民币' : '人民币';
    n = Math.abs(n);
    var s = '';
    for (var i = 0; i < fraction.length; i++) {
      s += (digit[Math.floor(n * 10 * Math.pow(10, i)) % 10] + fraction[i]).replace(/零./, '');
    }
    s = s || '整';
    n = Math.floor(n);
    for (var i = 0; i < unit[0].length && n > 0; i++) {
      var p = '';
      for (var j = 0; j < unit[1].length && n > 0; j++) {
        p = digit[n % 10] + unit[1][j] + p;
        n = Math.floor(n / 10);
      }
      //s = p.replace(/(零.)*零$/, '').replace(/^$/, '零')+ unit[0][i] + s;
      s = p + unit[0][i] + s;
    }
    return head + s.replace(/(零.)*零元/, '元').replace(/(零.)+/g, '零').replace(/^整$/, '零元整');
  }
  /**
   * [getUrlPrmt 获取url参数]
   * @param  {[string]} url [url地址]
   * @return {[object]}     [json数据]
   */
  ,getUrlPrmt: function(url) {
    url = url ? url : window.location.href;
    let _pa = url.substring(url.indexOf('?') + 1),
        _arrS = _pa.split('&'),
        _rs = {};
    for (let i = 0, _len = _arrS.length; i < _len; i++) {
      let pos = _arrS[i].indexOf('=');
      if (pos == -1) {
        continue;
      }
      //对 encodeURIComponent() 函数编码的 URI 进行解码。
      let name = _arrS[i].substring(0, pos),
          value = window.decodeURIComponent(_arrS[i].substring(pos + 1));
      _rs[name] = value;
    }
    return _rs;
  }
  /**
   * [setUrlPrmt 设置url参数]
   * @param {[object]} obj [json对象]
   */
  ,setUrlPrmt: function(obj) {
    let _rs = [];
    for (let p in obj) {
      if (obj[p] != null && obj[p] != '') {
        _rs.push(p + '=' + obj[p])
      }
    }
    return _rs.join('&');
  }
  /**
   * [randomNumber 随机返回一个范围的数字]
   * @param  {[number]} n1 [起始值]
   * @param  {[number]} n2 [目标值]
   * @return {[number]}    [随机值]
   */
  ,getRandomNumber: function(n1, n2) {
    if (arguments.length === 2) {
      return Math.round(n1 + Math.random() * (n2 - n1));
    } else if (arguments.length === 1) {
      return Math.round(Math.random() * n1)
    } else {
      return Math.round(Math.random() * 255)
    }
  }
  /**
   * [getRandomColor 随机颜色]
   * @return {[type]} [色值]
   */
  ,getRandomColor: function() {
    return '#'+Math.random().toString(16).substring(2).substr(0,6);
  }
  /**
   * [getEndTime 时间倒计时]
   * @param  {[string]} endTime [目标时间如'2018/7/22 16:0:0']
   * @return {[string]}         [剩余时间]
   */
  ,getEndTime: function(endTime) {
    var startDate = new Date(); //开始时间，当前时间
    var endDate = new Date(endTime); //结束时间，需传入时间参数
    var t = endDate.getTime() - startDate.getTime(); //时间差的毫秒数
    var d = 0,h = 0,m = 0,s = 0;
    if (t >= 0) {
      d = Math.floor(t / 1000 / 3600 / 24);
      h = Math.floor(t / 1000 / 60 / 60 % 24);
      m = Math.floor(t / 1000 / 60 % 60);
      s = Math.floor(t / 1000 % 60);
    }
    return "剩余时间" + d + "天 " + h + "小时 " + m + " 分钟" + s + " 秒";
  }
  /**
   * [getFontSize 适配rem]
   * @return {[type]} [比如在iphone6(屏幕宽度：375)上，375/750*100=50px;就是1rem=50px]
   */
  ,getFontSize: function() {
    var doc=document,win=window;
    var docEl = doc.documentElement,
    resizeEvt = 'orientationchange' in window ? 'orientationchange' : 'resize',
    recalc = function () {
        var clientWidth = docEl.clientWidth;
        if (!clientWidth) return;
        //如果屏幕大于750（750是根据我效果图设置的，具体数值参考效果图），就设置clientWidth=750，防止font-size会超过100px
        if(clientWidth>750){ clientWidth = 750 }
        //设置根元素font-size大小
        docEl.style.fontSize = 100 * ( clientWidth / 750 ) + 'px';
    };
    //屏幕大小改变，或者横竖屏切换时，触发函数
    win.addEventListener(resizeEvt, recalc, false);
    //文档加载完成时，触发函数
    doc.addEventListener('DOMContentLoaded', recalc, false);
  }
}
  /**
  * 功能：序列化form表单元素
  * 1.同名的name属性，值会被序列化为数组，例如checkbox等控件
  * 2.可以嵌套对象，name和value会被序列化为嵌套的json对象格式
  * 3.可以嵌套对象列表，name和value会被序列化成嵌套的json数组对象
  */
  $.fn.serializeNestedObject = function() {
    var json = {};
    var arrObj = this.serializeArray();
    $.each(arrObj, function() {
      // 对重复的name属性，会将对应的众多值存储成json数组
      if (json[this.name]) {
        if (!json[this.name].push) {
          json[this.name] = [json[this.name]];
        }
        json[this.name].push(this.value || '');
      } else {
        // 有嵌套的属性，用'.'分隔的
        if (this.name.indexOf('.') > -1) {
          var pos = this.name.indexOf('.');
          var key = this.name.substring(0, pos);
          // 判断此key是否已存在json数据中，不存在则新建一个对象出来
          if (!existKeyInJSON(key, json)) {
            json[key] = {};
          }
          var subKey = this.name.substring(pos + 1);
          json[key][subKey] = this.value || '';
        }
        // 普通属性
        else {
          json[this.name] = this.value || '';
        }

      }
    });
    // 处理那些值应该属于数组的元素，即带'[number]'的key-value对
    var resultJson = {};
    for (var key in json) {
      // 数组元素
      if (key.indexOf('[') > -1) {
        var pos = key.indexOf('[');
        var realKey = key.substring(0, pos);
        // 判断此key是否已存在json数据中，不存在则新建一个数组出来
        if (!existKeyInJSON(realKey, resultJson)) {
          resultJson[realKey] = [];
        }
        resultJson[realKey].push(json[key]);

      } else { // 单元素
        resultJson[key] = json[key];
      }
    };
    /**
     * 功能：判断key在Json结构中是否存在
     * 存在，返回true; 否则，返回false.
     */
    function existKeyInJSON(key, json) {
      if (key == null || key == '' || $.isEmptyObject(json)) {
        return false;
      }
      var exist = false;
      for (var k in json) {
        if (key === k) {
          exist = true;
        }
      }
      return exist;
    }
    return resultJson;
  };
console.log(myTools.trim('  fsggdagdafd  fdsafsa  ', 1));//fsggdagdafdfdsafsa
console.log(myTools.changeCase('gdagrtew11', 1));//Gdagrtew11
console.log(myTools.repeatStr('asd', '2'));//asdasd
console.log(myTools.replaceAll('asdfghjkl', 'asd', 'ssss'));//ssssfghjkl
console.log(myTools.replaceStr('18819322663',[3,5,3],0));//188*****663
console.log(myTools.checkType('11@qq.com','email'));//true
console.log(myTools.checkPwd('1111111aA.'));//4
console.log(myTools.countStr('fewqtreytreiynhtdagdsacsrqewgtrjytjngfbffdgfdgfdagd', 'f'));//6
console.log(myTools.unique([1, 1, 'a', 'A', 2, 2], false, function(item){
    return typeof item == 'string' ? item.toLowerCase() : item
})); // [1, "a", 2]
console.log(myTools.sortUnique([3,2,6,8,0,6,1,3,5,4,2,7,8,9]));//[0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
console.log(myTools.unique2([3,2,6,8,0,6,1,3,5,4,2,7,8,9]));//[3, 2, 6, 8, 0, 1, 5, 4, 7, 9]
console.log(myTools.upsetArr([3,2,6,8,0,6,1,3,5,4,2,7,8,9]));//[1, 6, 0, 2, 6, 3, 4, 8, 2, 7, 8, 9, 5, 3]
console.log(myTools.getMax([3,2,6,8,0,6,1,3,5,4,2,7,8,9]));//9
console.log(myTools.getMin([3,2,6,8,0,6,1,3,5,4,2,7,8,9]));//0
console.log(myTools.getSum([3,2,6,8,0,6,1,3,5,4,2,7,8,9]));//64
console.log(myTools.getAverage([3,2,6,8,0,6,1,3,5,4,2,7,8,9]));//4.571428571428571
console.log(myTools.getEleCount([3,2,6,8,0,6,1,3,5,4,2,7,8,9],3));//2
console.log(myTools.getCount([3,2,6,8,0,6,1,3,5,4,2,7,8,9,2,3,4,5,6,7,8,0,11,4,5,6,7,8,3,4,3,7,5,4,3]));//array[object]
console.log(myTools.upDigit(432153132.432));//人民币肆亿叁仟贰佰壹拾伍万叁仟壹佰叁拾贰元肆角叁分贰厘
console.log(myTools.getUrlPrmt('segmentfault.com/write?draftId=122&id=4314'));//json格式参数
console.log(myTools.setUrlPrmt({'a':1,'b':2}));//a=1&b=2
console.log(myTools.getRandomNumber(2,20));//15
console.log(myTools.getRandomColor());//#6728f2
console.log(myTools.getEndTime('2018/7/22 16:0:0'));//剩余时间350天 22小时 39 分钟12 秒
console.log(myTools.getFontSize());//clientWidth > 750px ==> 1rem = 100px
</script>
<div style="width:1rem;height:2rem;background:red;">
  11111
</div>
```