# 前言
>&emsp;&emsp;引用类型的值（对象）是**引用类型**的一个实例。引用类型是一种数据结构，用于将数据和功能组织在一起。`ECMAScript`从技术上将是一门面向对象的语言，但它不具备传统的面向对象语言所支持的类和接口等基本结构。引用类型有时候也被称为**对象定义**，因为它们秒速的是一类对象所具有的属性和方法。

`var person = new Object();`<br>
这行代码创建了Object引用类型的一个新实例。使用的*构造函数*是Object，它只为新对象定义了默认的属性和方法。

# 5.1 Object类型
1. 创建Object实例的方式有两种。
    * 使用new操作符后跟`Object`构造函数
    <pre><code>
        var person = new Object();
        person.name = "Nicholas";
        person.age = 29;
    </code></pre>
    * 使用对象字面量表示法（实际上不会调用Object构造函数）
    <pre><code>
        var person = {
            name : "Nicholas",
            age : 29
            //属性名可用字符串表示，也可以不用
            //"name" : "Nicholas"
        }
        //也可以留空花括号
        var person = {};    //与 new Object()相同
        person.name = "Nicholas";
        person.age = 29;
    </code></pre>
2. 访问对象属性的方法有两种
    * 点表示法
    * 方括号表示法
    <pre><code>
        console.log(person.name);
        console.log(person["name"]);
    </code></pre>
方括号语法的优点是可以通过变量来访问属性；如果属性名中包含会导致语法错误的字符，或者属性，名使用的是关键字或保留字，也可以用方括号。
<pre><code>
    //变量属性
    var propertyName = "name";
    console.log(person[propertyName]);  //Nicholas
    //会导致错误语法的字符
    person["first name"] = "Nicholas";
</code></pre>
**建议：**除非必须使用变量来访问属性，否则建议使用点表示法。

# 5.2 Array类型
1. 创建数组的基本方式有两种
    * 使用Array构造函数
    <pre><code>
        var colors = new Array();
        var colors = new Array(20);     //创建length为20的数组
        var colors = new Array("red","blue","green");   //创建包含3个字符串的数组
        var colors = Array(2);  //可以省略new关键字
    </code></pre>
    * 使用数组字面量表示法（不会调用Array构造函数）
    <pre><code>
        var colors = ["red","blue","green"];
        var values = [1,2,];    //不要这样！这样会创建一个包含2或3项的数组
        var options = [,,,,,];  //不要这样！这样会创建一个包含5或6项的数组
    </code></pre>
    **！注意：**在IE中，values会成为一个包含3个项目（最后一个为undefined）的数组；在其他浏览器中，values会成为一个包含2个项目的数组。options类似。**强烈建议不要使用这种语法**
2. 数组的`length`属性它不是只读的。通过设置这个属性，可以从数组的末尾移除项或向数组中添加新项。
    <pre><code>
        var colors = ["red","blue","green"];
        colors.length = 2;
        console.log(colors(2));     //undefined
        colors[length] = "orange";  //方便给数组添加新项
    </code></pre>
## 5.2.1 检测数组
对于*一个网页*或者*一个全局作用域*而言，使用`instanceof`足矣。如果网页中包含多个框架，那实际上就存在两个以上不同的全局执行环境，从而存在两个以上不同版本的`Array`构造函数。为解决这个问题，`ECMAScript 5`新增了`Array.isArray()`方法。这个方法用来确定某个值到底是不是数组，而不管它是在哪个全局执行环境中创建的。
<pre><code>
    if(Array.isArray(value)){
        //...
    }
</code></pre>
## 5.2.2 转换方法
* `toLocaleString()`
* `toString()`
* `valueOf()`
* `join()`<br>
如果数组中的某一项的值是`null`或者`undefined`，那么该值在以上4个方法返回的结果中以空字符串表示。
## 5.2.3 栈方法
* `push()`  返回数组的新长度
* `pop()`  返回移除的项
## 5.2.4 队列方法
* `shift()`     //返回移除的项
* `unshift()`   //返回数组的新长度<br>
`shift()`和`push()`结合使用；`unshift()`和`pop()`结合使用。<br>
**兼容性问题：**IE7及更早版本，`unshift()`方法总是返回`undefined`而不是数组的新长度。
## 5.2.5 重排序方法
1. `reverse()`
2. `sort()`
    * 默认情况下，`sort()`按升序排列数组项。`sort()`方法会调用每个数组项的`toString()`转型方法，然后比较得到的字符串。
    <pre><code>
        var values = [0,1,5,10,15];
        values.sort();
        alert(values);      //0,1,10,15,5
    </code></pre>
    *  ·`sort()`方法可以接收一个*比较函数*作为参数，以便我们指定哪个值位于哪个值的前面。
## 5.2.6 操作方法
* `concat()`：不影响原数组
* `slice()`: 不影响原数组<br>
    如果`slice()`方法的参数中有一个负数，则用数组长度加上该数来确定相应的位置。比如一个长度为5的数组上调用slice(-2,-1)与调用slice(3,4)得到的结果是一样的，如果结束位置小于起始位置，则返回空数组。
* `splice()`

