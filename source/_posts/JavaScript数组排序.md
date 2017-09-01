---
title: JavaScript数组排序
date: 2017-09-01 13:09:43
tags: JavaScript
---
***
<font face="宋体">&ensp;&ensp;数组中已经存在两个重排序方法:reverse()和sort()
&ensp;&ensp;我们先来看下比较简单的reverse()方法： </font>
&ensp;&ensp;**1. reverse()将数组项的顺序进行反转**
```javascript
var numList1=[1,2,3,4,5,6];  
numList1.reverse();  
console.log(numList1);//[6, 5, 4, 3, 2, 1]
```
<font face="宋体">&ensp;&ensp;此方法简单明了,但也不够灵活.接下来我们看sort()方法:</font>
&ensp;&ensp;**2.sort()默认按照数组升序排列数组**
<font face="宋体">&ensp;&ensp;sort()方法会调用每个数组项的toString()转型方法,然后比较得到的字符串,以确定如何排序.</font>
```javascript
var numList1=[1,5,25,12,20,10];  
numList1.sort();  
console.log(numList1);//[1, 10, 12, 20, 25, 5]虽然数值5小于除1以外的其他数值,但是在进行字符串比较时，"5"则位于其他字符串值的后面   
```
<font face="宋体">
&ensp;&ensp;这种排序方法很不理想，但是sort()方法可以接受一个比较函数来自定义数组的排序方法。
&ensp;&ensp;比较函数接受两个参数，如果第一个参数应该位于第二个之前则返回一个负数，如果两个参数相等则返回0，如果第一个参数应该位于第二个之后则返回一个正数。
</font>
```javascript
function compare(a,b){  
   if(a<b){  
      return -1;  
   }else if(a>b){  
      return 1;  
   }else{  
      return 0;  
   }  
}  
var numList1=[1,5,25,12,20,10];  
numList1.sort(compare);  
console.log(numList1);//[1, 5, 10, 12, 20, 25] 嗯，完美!  
```
<font face="宋体">
&ensp;&ensp;我们也可以将该排序方法优化一下
</font>
```javascript
function compare(a,b){  
    return a-b;  
}  
var numList1=[1,5,25,12,20,10];  
numList1.sort(compare);  
console.log(numList1);//[1, 5, 10, 12, 20, 25]
```
<font face="宋体">&ensp;&ensp;由于比较函数通过返回一个小于零、等于零或大于零的值来影响排序结果，因此减法操作就可以适当地处理所有这些情况。接下来，我们来看下包含对象的数组排序：</font>
```javascript
var objectList=[{name:"北京",value:89},{name:"上海",value:96},{name:"深圳",value:67},{name:"广州",value:73}];
```
<font face="宋体">
&ensp;&ensp;如果需要按value值的大小进行升序排序呢？很简单，只需要将排序方法变化一下即可：
</font>
```javascript
function compare(a,b){  
    return a.value-b.value;  
}  
var objectList=[{name:"北京",value:89},{name:"上海",value:96},{name:"深圳",value:67},{name:"广州",value:73}];  
objectList.sort(compare);  
console.log(objectList);//[{name:"深圳",value:67},{name:"广州",value:73},{name:"北京",value:89},{name:"上海",value:96}]  
```
<font face="宋体">
&ensp;&ensp;但是如果此时出现脏数据，比如某个项的value值为null或者为 " " 呢？
</font>
```javascript
function compare(a,b){  
    return a.value-b.value;  
}  
var objectList=[{name:"北京",value:89},{name:"上海",value:96},{name:"深圳",value:67},{name:"广州",value:null}];  
objectList.sort(compare);  
console.log(objectList);//[{name:"广州",value:null},{name:"深圳",value:67},{name:"北京",value:89},{name:"上海",value:96}]  
```
<font face="宋体">
这里可以看出，在value值为null的情况下，sort在对其进行排序的时候，会调用函数Number()方法，先将其转换为数值再排序。
</font>






><font face="宋体">以下给出Number()函数的转换规则：</font>
  - <font face="宋体">1.如果是Boolean值，true和false将分别转换为1和0。</font>
  - <font face="宋体">2.如果是数字值，只是简单的传入和返回。</font>
  - <font face="宋体">3.如果是null值，返回0。</font>
  - <font face="宋体">4.如果是undefined，返回NaN(非数值)。</font>
  - <font face="宋体">5.如果是字符串，遵循以下规则：
    	&ensp;&ensp;a.如果字符串中只包含数字(包括前面带正号或负号的情况)，则将其转换为十进制数值，即"1"会变成1，"123"会变成123，而"011"会变成11(前导零被忽略)；
    	&ensp;&ensp;b.如果字符串中包含有效的浮点格式，如"1.1"则将其转换为对应的浮点数值(同样，也会忽略前导零)；
    	&ensp;&ensp;c.如果字符串中包含有效的十六进制格式，例如"0xf",则将其转换为相同大小的十进制整数值；
    	&ensp;&ensp;d.如果字符串是空的(不包含任何字符)，则将其转换为0；
    	&ensp;&ensp;e.如果字符串中包含除上述格式之外的字符，则将其转换为NaN；</font>
  - <font face="宋体">6.如果是对象，则调用对象的valueOf()方法，然后依照前面的规则转换返回的值。如果转换的结果是NaN，则调用对象的toString()方法，然后再次依照前面的规则转换返回的字符串值。</font>

  