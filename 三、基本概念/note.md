#3.1严格模式
在顶部添加如下代码：<br>
`"use strict";`<br>
它是一个编译指示，用于告诉支持的JavaScript引擎切换到严格模式。

#3.2变量
* 初始化变量**并不会**为它标记类型，初始化的过程就是给变量赋一个值。
* 如果在函数中使用`var`定义一个变量，那么这个变量在函数退出后就会被销毁。

#3.3数据类型
ECMAScript中有5中简单数据类型（基本数据类型）：`undefined`、`null`、`boolean`、`number`、`string`，还要一种复杂数据类型：`object`。

##3.3.1 typeof操作符
`typeof`操作符可能返回下列某个字符串：<br>

* `undefined`——未定义
* `boolean`——布尔值
* `string`——字符串
* `number`——数值
* `object`——对象或null
* `function`——函数

typeof是操作符，不是函数，所以以下两种用法均正确：<br>
`console.log(typeof message);`<br>
`console.log(typeof (message));`<br>
Chrome7及之前版本在对正则表达式调用`typeof`操作符时会返回`function`，而其他浏览器会返回`object`。

##3.3.2 Undefined类型
使用`var`声明变量，但未对其加以初始化时，这个变量的值就是`undefined`。<br>
已经定义但未初始化的变量，与尚未定义的变量是不一样的：<br>
```
    var message;
    console.log(message);       //undefined
    console.log(age);           //报错：age is not defined
``` 

对已经定义但未初始化的变量，与尚未定义的变量执行typeof操作符会返回`undefined`值：
```
    var message;
    console.log(typeof message);       //undefined
    console.log(typeof age);           //undefined
```

##3.3.3 Null类型
* null表示一个空对象指针。<br>
* 只要意在保存对象的变量还没有保存对象，就应该明确地让该变量保存null值。这样有助于区分null和undefined。
* `console.log(null == undefined);          //true`

##3.3.5 Boolean类型
ECMAScript中所以类型的值都有与这两个Boolean值等价的值，调用转型函数Boolean()转换。
```
    var message = "Hello World!";
    var messageAsBoolean = Boolean(message);
```

##3.3.5 Number类型
###3.3.5.1浮点数值
* 八进制字面量值得第一位必须是（0），十六进制字面值的前两位必须是0x。*在进行算术计算时，所有以八进制和十六进制表示的数值最终都将被转换成十进制数值。*
* 不推荐下面这种写法：
`var floatNum = .1;`
* 对于极大或极小的数值，可以用e（科学计数法）表示。ECMAScript将小数点后面带有6个零以上的浮点数值转换为以e表示法表示的数值。
```
    var floatNum1 = 3.12e7;         //等于31200000
    var floatNum2 = 0.00000003;     //等于3e-8
```
* 浮点数最高精度是17位小数。

###3.3.5.2数值范围
* 最小数值：`Number.MIN_VALUE`
* 最大值：`Number.MAX_VALUE`
数值超出则转换为`Infinity`（正无穷）、`-Infinity`（负无穷）。<br>
确定一个数值是不是有穷的，可以使用 `isFinite()`函数，位于最大值与最小值之间时返回`true`。

###3.3.5.3 NaN
Not a Number.表示一个本来要返回数值的操作数未返回数值的情况。<br>
`isNaN()`  是数字或能转换成数字，返回`false`，否则返回`true`。

###3.3.5.4数值转换
1. `Number()`:<br>
    * 如果字符串中只包含数字，则将其转换为十进制。所以八进制的数值删去前面的0，而不是转换成十进制。<br>
    `Number(012);       //12`
    * 如果字符串包含十六有效的十六进制格式，则转换为十进制整数值。<br>
    `Number(oxf);       //15`
2. `parseInt()`:<br>
    * 如果字符串以0开头，且后面跟着数字字符，则会将其当做一个八进制数来解析。（`ECMAScript 3`支持，`ECMAScript 5`已经不支持），为避免错误的解析，**建议无论在什么情况下都明确指定基数**。
    ```
        parseInt("10",2);
        parseInt("10",8);
        parseInt("10",10);
        parseInt("10",16);
    ```
3. `parseFloat()`：
    * 只解析十进制值，没有第二个参数。它始终都会忽略前导的零。<br>
    `parseFloat("0x9");         //0`
    `parseFloat("3.125e7");     //31250000`

##3.3.6 String类型
1. `toString()`：
    * 数值、布尔值、对象和字符串值都有这个方法，返回相应值的字符串表现。`undefined`、`null`没有这个方法。
    * 数值的`toString()`可以传递一个参数，规定以某个进制格式表示的字符串。
    ```
        var num = 10;
        num.toString();     //不写参数默认十进制  "10"
        num.toString(2);    // "1010"
        num.toString(8);    // "12"
    ```
2. `String()`：
    * 能将任何类型的值转换为字符串，包括`null`、`undefined`。
    ```
        var value1 = 10;
        var value2 = true;
        var value3 = null;
        var value4;
        console.log(String(value1));    //"10"
        console.log(String(value2));    //"true"
        console.log(String(value3));    //"null"
        console.log(String(value4));    //"undefined"
    ```
##3.3.7 Object类型
`object`的每个实例都具有下列属性和方法：<br>

* `Constructor`：保存着用于创建当前对象的函数。
* `hasOwnProperty(propertyName)`:用于检查给定的属性在当前对象实例中是否存在。*properName*必须以字符串形式指定（o.hasOwnProperty("name")）。
* `isPrototypeOf(object)`:用于检查传入的对象是否是另一个对象的原型。
* `propertyIsEnumerable(properName)`：用于检查给定的属性是否能够使用`for-in`语句，参数必须是字符串。
* `'toString()`：返回对象的字符串表示。
* `valueOf()`：返回对象的字符串、数值或布尔值表示。通常与`toString()`返回值相同。

#3.4操作符
##3.4.1一元操作符
>只能操作一个值的操作符叫做一元操作符。<br>

1. 递增递减操作符 `++ --`
    * 它们不仅适用于整数，还可以用于字符串、布尔值、浮点数值和对象。
    * 对于非数值的递增递减，先转换为数值，再执行递增递减操作。
2. 一元加和减操作符
    * 在对非数值应用*一元加*操作符时，该操作符会像`Number()`一样对这个值执行转换。对象是先调用`valueOf()`和`tuString()`，再转换得到的值。
    ```
        var a = false;
        var o = {
            valueOf: function(){
                return -1;
            }
        };
        console.log(+a);        //0
        console.log(+0);        //-1
    ```
