title: 笔试题
author: songxingguo
tags: 
  - 找工作
categories:
  - 备忘录
date: 2018-10-10 22:15:00
---
## 去哪儿

<!-- more -->

笔试一：

```js
var arr = "[1-3,6,8,12-15]";
var newArr = [];

var str="flight_node[1-3].qunar.com";

function getContent(str) {
    var regex2 = /\[(.+?)\]/g;   // [] 中括号
    // var tmp  = str.match(regex2);
    var newStr = str.match(regex2);
   retutrn newStr;
}

function getOther()



function removeBrackets(str)  {      
  if (str) {          
      // var reg = /^\$\[/gi;          
      // var reg2 = /\]$/gi;      
      var reg = "/\[.*\]/";    
      // str = str.replace(reg, '');   
      // str = str.replace("/\[.*\]/", '', str);       

      str = str.replace(/\[|]/g,'');
      // str = str.replace(reg2, '');      
  } 
          
  return str;  
}

var str1 = getContent(str);

console.log(str1[0]);

var newstr = removeBrackets(str1[0]);
console.log(newstr);

splitString(newstr);

function splitString(str) {
  var arr  = str.split(",");

  for(var i =0; i < arr.length; i++) {
    var strs = arr[i].toString().split("-");
    for(var j =0; j < strs.length; j++) {
      newArr.push(strs[j]);
    }
  }
}

function print(arr) {
  for(var i = 0; i < arr.length; i++) {
     console.log(newArr[i]);
  }
}
```

笔试二：

```js
function combination(arr, fgf) {
    if (fgf == undefined) fgf = " ";

    var len = arr.length;

    if (len >= 2) {
        var len1 = arr[0].length;
        var len2 = 0;
        try {
            len2 = arr[1].length;
        } catch (e) {
            var x = "";
        }

        var newlen = len1 * len2;
        var temp = new Array(newlen);
        var index = 0;
        for (var i = 0; i < len1; i++) {
            for (var j = 0; j < len2; j++) {
                temp[index] = arr[0][i] + fgf + arr[1][j];
                index++;
            }
        }

        var newArray = new Array(len - 1);
        for (var i = 2; i < len; i++) {
            newArray[i - 1] = arr[i];
        }
        newArray[0] = temp;

        return combination(newArray, fgf);
    }
    else {
        return arr[0];
    }
}
```
## 小米
