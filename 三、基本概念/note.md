# 3.1严格模式
在顶部添加如下代码：<br>
`"use strict";`<br>
它是一个编译指示，用于告诉支持的JavaScript引擎切换到严格模式。

# 3.2变量
* 初始化变量**并不会**为它标记类型，初始化的过程就是给变量赋一个值。
* 如果在函数中使用`var`定义一个变量，那么这个变量在函数退出后就会被销毁。

# 3.3数据类型
ECMAScript中有5种简单数据类型（基本数据类型）：`undefined`、`null`、`boolean`、`number`、`string`，还有一种复杂数据类型：`object`。

## 3.3.1 typeof操作符
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

## 3.3.2 Undefined类型
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

## 3.3.3 Null类型
* null表示一个空对象指针。<br>
* 只要意在保存对象的变量还没有保存对象，就应该明确地让该变量保存null值。这样有助于区分null和undefined。
* `console.log(null == undefined);          //true`

## 3.3.4 Boolean类型
ECMAScript中所有类型的值都有与这两个Boolean值等价的值，调用转型函数Boolean()转换。
```
    var message = "Hello World!";
    var messageAsBoolean = Boolean(message);
```

## 3.3.5 Number类型
### 3.3.5.1浮点数值
* 八进制字面量值的第一位必须是（0），十六进制字面值的前两位必须是0x。*在进行算术计算时，所有以八进制和十六进制表示的数值最终都将被转换成十进制数值。*
* 不推荐下面这种写法：
`var floatNum = .1;`
* 对于极大或极小的数值，可以用e（科学计数法）表示。ECMAScript将小数点后面带有6个零以上的浮点数值转换为以e表示法表示的数值。
```
    var floatNum1 = 3.12e7;         //等于31200000
    var floatNum2 = 0.00000003;     //等于3e-8
```
* 浮点数最高精度是17位小数。

### 3.3.5.2数值范围
* 最小数值：`Number.MIN_VALUE`
* 最大值：`Number.MAX_VALUE`<br>

数值超出则转换为`Infinity`（正无穷）、`-Infinity`（负无穷）。<br>
确定一个数值是不是有穷的，可以使用 `isFinite()`函数，位于最大值与最小值之间时返回`true`。

### 3.3.5.3 NaN
Not a Number.表示一个本来要返回数值的操作数未返回数值的情况。<br>
`isNaN()`  是数字或能转换成数字，返回`false`，否则返回`true`。

### 3.3.5.4数值转换
1. `Number()`:<br>
    * 如果字符串中只包含数字，则将其转换为十进制。所以八进制的数值删去前面的0，而不是转换成十进制。<br>
    `Number(012);       //12`
    * 如果字符串包含十六有效的十六进制格式，则转换为十进制整数值。<br>
    `Number(0xf);       //15`
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

## 3.3.6 String类型
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
## 3.3.7 Object类型
`object`的每个实例都具有下列属性和方法：<br>

* `Constructor`：保存着用于创建当前对象的函数。
* `hasOwnProperty(propertyName)`:用于检查给定的属性在当前对象实例中是否存在(不包括原型中的属性)。*propertyName*必须以字符串形式指定（o.hasOwnProperty("name")）。
* `isPrototypeOf(object)`:用于检查传入的对象是否是另一个对象的原型。
* `propertyIsEnumerable(properName)`：用于检查给定的属性是否能够使用`for-in`语句，参数必须是字符串。
* `toString()`：返回对象的字符串表示。
* `valueOf()`：返回对象的字符串、数值或布尔值表示。通常与`toString()`返回值相同。

# 3.4操作符
## 3.4.1一元操作符
>只能操作一个值的操作符叫做一元操作符。<br>

1. 递增递减操作符 `++ --`
    * 它们不仅适用于整数，还可以用于字符串、布尔值、浮点数值和对象。
    * 对于非数值的递增递减，先转换为数值，再执行递增递减操作。
2. 一元加和减操作符
    * 在对非数值应用*一元加*操作符时，该操作符会像`Number()`一样对这个值执行转换。对象是先调用`valueOf()`和`toString()`，再转换得到的值。
    ```
        var a = false;
        var o = {
            valueOf: function(){
                return -1;
            }
        };
        console.log(+a);        //0
        console.log(+o);        //-1
    ```
    * 在对非数值应用*一元减*操作符时，遵循与一元加相同的规则，最后再将得到的数值转换为负数。

## 3.4.2位操作符
>`ECMAScript`中的所有数值都以`IEEE-754` 64位格式存储，但位操作并不直接操作64位的值。对数值应用位操作符时，后台转换过程如下：64位的数值被转换成32位数值，然后执行位操作，最后再讲32位的结果转回64位数值。<br>

* 对于有符号的整数，32位中的前31位（从右往左数）用于表示整数的值，第32位用于表示数值的符号：0表示正数，1表示负数。<br>
* 负数使用二进制补码表示。所以对于负数的二进制表示，先转换成二进制码再求值。<br>
* 在对特殊的`NaN`和`Infinity`值应用位操作是，这两个值都会被当成0来处理。<br>
* 对非数值应用位操作符，会先使用`Number()`函数将该值转换为一个数值（自动完成），然后再应用位操作，得到的结果将是一个数值。<br>

计算一个数值二进制补码的3个步骤：<br>
    1. 求这个数值绝对值的二进制码（例如，要求-18的二进制补码，先求18的二进制码）；<br>
    2. 求二进制反码，即将0换成1,1换成0。<br>
    3. 得到的二进制反码加1。

1. 按位非(NOT)
>按位非用波浪线(~)表示，执行结果就是返回数值的反码。<br>
对位取反，1变0，0变1。<br>
按位非的本质：操作数的负值减1

2. 按位与(AND)
>按位与用和号字符(&)表示。<br>
对应位都为1取1，其他均取0。<br>

3. 按位或(OR)
>按位或用一个竖线符号(|)表示。<br>
对应位有一个是1取1，两个都是0才取0。

4. 按位异或(XOR)
>按位异或用一个插入符号(^)表示。<br>
对应位相同取0，不同取1。<br>

5. 左移
>左移用两个小于号(<<)表示，将数值的所有位向左移动指定的位数，用0填充右边的空位。<br>
**左移不会影响操作数的符号位。**

6. 有符号的右移
>有符号的右移用两个大于号(>>)表示，将数值向右移动，但**保留符号位**，用**符号位**填充左边的空位。

7. 无符号右移
>无符号右移用3个大于号(>>>)表示，将数值的所有32位**(包括符号位)**都向右移动，用0填充左边的空位。<br>
对正数来说，无符号右移与有符号右移结果相同。<br>
对负数来说，符号位1会被当成二进制1，导致右移后结果非常大。

## 3.4.3 布尔操作符
> 逻辑非(!)、逻辑与(&&)、逻辑或(||)

1. 逻辑与。在**有一个操作数不是布尔值**的情况下， 逻辑与操作就不一定返回布尔值；此时，它遵循下列规则：
    * 如果第一个操作数是对象，则返回第二个操作数；
    * 如果第二个操作数是对象，则只有在第一个操作数求值结果为`true`的情况下才会返回该对象；
    * 如果两个操作数都是对象，则返回第二个操作数；
    * 如果有一个操作数是`null`、`NaN`、`undefined`，则返回`null`、`NaN`、`undefined`。<br>

    逻辑与操作属于短路操作，即如果第一个操作数能够决定结果，那么就不会再对第二个操作数求值。

2. 逻辑或。如果**有一个操作数不是布尔值**，与逻辑与操作相似。

    利用逻辑或的这一行为来避免为变量赋`null`或`undefined`。<br>
    `var myObject = preferredObject || backupObject;`

## 3.4.4 乘性操作符
>3个乘性操作符：乘法、除法和求模。<br>

* 如果参与乘法/除法/求模计算的某个操作数不是数值，后台会先使用`Number()`转型函数将其转换为数值。

## 3.4.5 加性操作符
1. 加法
    * 如果有一个操作数是对象、数值或布尔值，则调用它们的`toString()`方法取得相应的字符串值，然后再应用前面关于字符串的规则。对于`undefined`和`null`，则调用`String()`函数并取得字符串"undefined"和"null"。
2. 减法
    * 如果有一个操作数是字符串、布尔值、null或undefined，则先在后台调用`Number()`将其转换为数值。如果转换的结果是NaN，则减法的结果就是NaN。
    * 如果有一个操作数是对象，则调用对象的`valueOf()`方法取得该对象的数值。如果对象没有`valueOf()`方法，则调用其`toString()`方法并将得到的字符串转为数值。

## 3.4.6 关系操作符
>任何操作数与`NaN`比较，结果都是`false`。所以，会出现如下结果：
```
    var result1 = NaN < 3;          //false
    var result2 = NaN >= 3;         //false
```

## 3.4.7 相等操作符
1. 相等和不相等
>先转换后比较

* `null`和`undefined`是相等的；
* 要比较相等性之前，不能将`null`和`undefined`转换成其他任何值
    ```
        undefined == 0;             //false
        null == 0;                  //false
    ```

## 3.4.8 逗号操作符
* 使用逗号操作符可以在一条语句中执行多个操作
    `var num1=1, num2=2, num3=3;`
* 使用逗号操作符赋值。在用于赋值时，逗号操作符总会返回表达式中的最后一项。<br>
    `var num = (5, 1, 4, 8, 0);             //num的值为0`

# 3.5 语句
## 3.5.1 label语句
>使用`label`语句可以在代码中添加标签，以便将来使用。<br>
`label: statement`

```
    start: for(var i=0;i<count;i++){
        alert(i);
    }
```

start标签可以在将来由`break`或`continue`语句引用。加标签的语句一般都要与for语句等循环语句配合使用。
```
    var num = 0;
    outermost:
    for(var i=0;i<10;i++){
        for(var j=0;j<10;j++){
            if(i==5 && j==5){
                break outermost;
            }
        }
    }
    alert(num);         //55
```

## 3.5.2 with语句
>with语句的作用是将代码的作用域设置到一个特定的对象中。目的主要是为了简化多次编写同一个对象的工作。

```
    var qs = location.search.substring(1);
    var hostName = location.hostname;
    var url = location.href;

    //上面几行代码都包含location对象。如果使用with语句，将上面代码改写成如下：
    with(location){
        var qs = search.substring(1);
        var hostName = hostname;
        var url = href;
    }
```
以上例子使用了`with`语句关联了location对象。这意味着`with`语句的代码块内部，每个变量首先被认为是一个局部属性，二如果在局部环境中找不到该变量的定义，就会查询location对象中是否有同名的属性。如果发现了同名属性，则以location对象属性的值作为变量的值。<br>
**严格模式下不允许使用`with`语句**，否则将视为语法错误。<br>
**！注意：由于大量使用`with`语句会导致性能下降，同时也会给调试代码造成困难，因此在开发大型应用程序时，不建议使用`with`语句。**

# 3.6 函数
**建议：** *要么让函数始终都返回一个值，要么永远都不要返回值。否则，如果函数有时候返回值，有时候不返回值，会给调试代码带来不便。*<br>
严格模式对函数有一些限制：<br>

* 不能把函数、参数命名为eval或arguments；
* 不能出现两个命名参数同名的情况。

# 3.7 小结
`ECMAScript`中没有函数签名的概念，因为其函数参数是以一个包含零或多个值的数组的形式传递的。