# DOM
> IE中的所有DOM对象都是以COM对象的形式实现的。这意味着IE中的DOM对象与原生JavaScript对象的行为或活动特点并不一致。

## 10.1 节点层次
> 在`HTML`页面中，文档元素始终都是`<html>`元素。在`XML`中，没有预定义的元素，因此任何元素都可能成为文档元素。

### 10.1.1 Node类型
DOM1级定义了一个Node接口，这个接口由DOM中的所有节点类型实现。JavaScript中做为Node类型实现了这个Node接口。除IE外，在其他所有浏览器中都可以访问到这个类型。Javascript中的所有节点类型都继承自Node类型，因此所有节点类型都共享着相同的基本属性和方法。
每个节点都有一个nodeType属性，用于表明节点的类型。节点类型由在Node类型中定义的下列12个数值常量来表示，任何节点类型必居其一：
|常量名|常量值|节点类型|描述|
|---------|----------|-----------|--------|
|Node.ELEMENT_NODE |	1 |	Element|代表元素|
|Node.ATTRIBUTE_NODE|	2|	Attr|	代表属性|
|Node.TEXT_NODE|	3	|Text	|代表元素或属性中的文本内容|
|Node.CDATA_SECTION_NODE|	4|	CDATASection|	代表文档中的 CDATA 部（不会由解析器解析的文本）|
|Node.ENTITY_REFERENCE_NODE|	5|	EntityReference|	代表实体引用|
|Node.ENTITY_NODE|	6|	Entity|	代表实体|
|Node.PROCESSING_INSTRUCTION_NODE|	7|	ProcessingInstruction|	代表处理指令|
|Node.COMMENT_NODE|	8|	Comment|	代表注释|
|Node.DOCUMENT_NODE|	9|	Document|	代表整个文档（DOM 树的根节点）|
|Node.DOCUMENT_TYPE_NODE|	10|	DocumentType|向为文档定义的实体提供接口|
|Node.DOCUMENT_FRAGMENT_NODE|	11|	DocumentFragment|	代表轻量级的 Document 对象（文档的某个部分）|
|Node.NOTATION_NODE|	12	|Notation	|代表 |DTD |中声明的符号|

```
//在IE中无效。因为ID没有公开Node类型的构造函数
if(someNode.nodeType == Node.ELEMENT_NODE){
    console.log("Node is an element");
}
//适用于所有浏览器
if(someNode.nodeType == 1){
    alert("Node is an element.");
}

```

1、nodeName和nodeValue属性
对于元素节点，nodeName中保存的始终都是元素的标签名，而nodeValue的值则始终为null。

2、节点关系
* 每个节点都有一个childNodes属性，其中保存着一个NodeList对象。NodeList是一种类数组对象。它是基于DOM结构动态执行查询的结果，因此DOM结构的变化能改自动反应在NodeList对象中。
* 有两种方法访问保存在NodeList中的节点。可以通过方括号，也可以使用item()方法。
    ```
    var firstChild = someNode.childNodes[0];
    var secondChild = someNode.childNodes.item(1);
    ```
* 把NodeList类数组转为数组
    ```
    //在IE8及之前版本中无效，因为在IE8及更早的版本将NodeList实现为一个COM对象。
    var arrayOfNodes = Array.prototype.slice.call(someNode.childNodes,0);
    ```
* 节点之间还有以下一些属性
    * parentNode
    * previousSibling
    * nextSibling
    * firstChild
    * lastChild
* hasChildNodes()方法在节点包含一或多个子节点的情况下返回true。这比查询childNodes列表的length属性更简单。
* ownerDocument属性，该属性指向表示整个文档的文档节点。

3、操作节点
* appendChild()：向childNodes列表的末尾添加一个节点。
    ```
    var returnedNode = someNode.appendChild(newNode);
    ```
如果传入到appendChild()中的节点已经是文档的一部分了，那结果就是将该节点从原来的位置转移到新的位置。即使可以将DOM树看成是由一系列指针连接起来的，但任何DOM节点也不能同时出现文档中的多个位置上。
* insertBefore()：这个方法接受两个参数：要插入的节点和作为参照的节点。把节点插入到参照节点的前面。如果参照节点是null，则与appendChild()执行相同的操作。
* replaceChild()：这个方法接受两个参数：要插入的节点和要替换的节点。被替换的节点仍然在文档中，但它在文档中已经没有了自己的位置。
* removeChild()：这个方法接受一个参数，即要移除的节点。被移除的节点将成为方法的返回值。

4、其他方法
* cloneNode()：用于创建调用这个方法的节点的一个完全相同的副本。这个方法接受一个布尔值参数，为true时执行深复制，复制节点机器整个子节点数；为false时，执行潜复制，即只复制节点本身。
**注意：**cloneNode()方法不会复制添加到DOM节点中的JavaScript属性，例如事件处理程序等。这个方法只复制特性、（在明确指定的情况下也复制）子节点。其他一切都不会复制。IE在此存在一个bug，即它会复制事件处理程序，所以建议在复制之前最后先移除事件处理程序。
* normalize()：处理文档树中的文本节点。

### 10.1.2 Document类型
> 在浏览器中，`document`对象是`HTMLDocument`（继承自Document类型）的一个实例，表示整个HTML页面。而且，`document`对象是`window`对象的一个属性，因此可以将其作为全局对象来访问。Document节点具有下列特性：
* nodoeType的值为9
* nodeName的值为"#document"
* nodeValue的值为null
* parentNode的值为null
* ownerDocument的值为null
* 其子节点可能是一个DocumentType（最多一个）、Element（最多一个）、ProcessingInstruction或Comment。
Document类型可以表示HTML页面或者基于XML的文档。

在Firefox、Safari、Chrome和Opera中，可以通过脚本访问Document类型的构造函数和原型。但在所有浏览器中都可以访问HTMLDocument类型的构造函数和原型，包括IE8及其后续版本。

1、文档的子节点
* 有两个内置的访问Document节点的子节点的快捷方式。第一个是`documentElement`属性，该属性始终指向HTML页面中的`<html>`元素，所有浏览器都支持这个属性。另一个就是通过childNodes列表访问文档元素。
    ```
    <html>
        <body>
        </body>
    </html>

    var html = document.documentElement;    //取得对<html>的引用
    console.log(html === document.childNodes[0]);   //true
    console.log(html === document.firstChild);  //true
    ```
* document对象还有一个body属性，直接指向`<body>`元素。所有浏览器都支持这个属性。
    ```
    var body = document.body;   //取得对<body>的引用
    ```
* Document另一个可能的子节点是DocumentType。通常将`<!DOCTYPE>`标签看成一个与文档其他部分不同的实体，可以通过doctype属性（在浏览器中是document.doctype)来访问它的信息。浏览器对document.doctype的支持差别很大。
    ```
    var doctype = document.doctype;     //取得对<!DOCTYPE>的引用
    ```

2、文档信息
作为HTMLDocument的一个实例，document对象还有一些标准的Document对象所没有的属性。
* 其中第一个属性title，包含着`<title>`元素中的文本。通过获取/修改d当前页面的标题，修改title属性的值不会改变`<title>`元素。
    ```
    var originalTitle = document.title;
    document.title = 'New page title';
    ```
* URL属性中包含页面完整的URL（即地址栏中显示的URL），domain属性中只包含页面的域名，而referrer属性中则保存着链接到当前页面的那个页面的URL。在没有来源页面的情况下，referrer属性中可能会包含空字符串。所有这些信息都存在于请求的HTTP头部。
    ```
    //取得完整的URL
    var url = document.URL;     //http://www.wrox.com/WileyCDA/
    //取得域名
    var domain = document.domain;   //www.wrox.com
    //取得来源页面的URL
    var referrer = document.referrer;
    ```
在这3个属性中，只有domain是可以设置的。但出于安全方面的限制，也并非可以给domain设置任何值。如果URL中包含一个子域名，例如`p2p.wrox.com`，那么就只能将domain设置为"`wrox.com`"（"`www.wrox.com`"也是如此）。不能将这个属性设置为URL中不包含的域。
```
//假设页面来自p2p.wrox.com
document.domain = "wrox.com";   //成功
document.domain - "nczonline.net";  //出错
```
由于跨域安全限制，来自不同子域的页面无法通过JavaScript通信。而通过将每个页面的document.domain设置为相同的值，这些页面就可以互相访问对方包含的JavaScript对象了。
假如有一个页面加载子`www.wrox.com`，其中包含一个内嵌框架，框架内的页面加载自`p2p.wrox.com`。由于document.domain字符串不一样，内外两个页面直接无法互相访问对方的JavaScript对象。但如果将这两个页面的document.domain都设置为"`wrox.com`"，它们直接就可以通信了。
浏览器对domain属性还有一个限制，即如果域名一开始是“松散的”(loose)，那么不能将它再设置为“紧绷的”(tight)。在上面这个例子中，在设置为"`wrox.com`"之后，就不能再设置为"`p2p.wrox.com`"，否则将会导致错误。所有的浏览器都存在这个限制。

3、查找元素
* getElementById()：返回文档中第一次出现的匹配的元素。在IE7及更早版本中，不区分元素ID的大小写。而且早在IE7及更早版本中，如果文档中表单元素的name属性与给定ID匹配也会被该方法返回。
* getElementsByTagName()：返回一个HTMLCollection对象，这个对象与NodeList类型，可以使用方括号语法或item()方法来访问对象中的项。
HTMLCollection对象还有一个namedItem()方法，使用这个方法，可以通过元素的name属性取得集合中的项。
    ```
    <img src="myImage.gif" name="myImage">

    var images = document.getElementdByTagName("img");
    //从images中得到name=mtImage的<img>元素
    var myImage = images.namedItem("myImage");
    ```
对命名的项，也可以通过方括号语法来访问
```
var myImage = images["myImage"];
```
* getElementsByName()：也会返回一个HTMLCollection对象。

4、特殊集合
document对象还有一些特殊的集合，包括：
* document.anchors：包含文档中所有带有name特性的`<a>`元素；
* document.forms：包含文档中所有的`<form>`元素；
* document.images：包含文档中所有的`<img>`元素；
* document.links：包含文档中所有带href特性的`<a>`元素。
与HTMLCollention对象类似，集合中的项也会随之当前文档内容的更新而更新。

5、DOM一致性检测
DOM1级只为document.implementation规定了一个方法，即hasFeature()。这个方法接受两个参数：要检测的DOM功能的名称及版本号。如果浏览器支持给定名称和版本的功能，则该方法返回true。
```
var hasXmlDom = document.implementation.hasFeature("XML", "1.0");
```
hasFeature()方法也有缺点，返回true有时候也不意味着实现与规范一直的功能，即在没有完全实现某些DOM功能的情况下也可能返回true。为此，建议除了检测hasFeature()之外，还同时使用能力检测。

6、文档写入
write()、writeln()、open()和close()。其中write()和writeln()方法都接受一个字符串参数，即要写入到输出流中的文本。write()会原样写入，而writeln()则会在字符串的末尾添加一个换行符。
```
<body>
    <p>The current date and time is:
        <script>
            document.write("<strong>"+ (new Date()).toString() +"</strong>");
        </script>
    </p>
</body>
```
用write()、writeln()在HTML页面中动态创建的DOM元素，可以在将来访问该元素。
也可以用write()、writeln()动态包含外部资源，例如JavaScript文件等。但必须注意不能像下面这样直接包含`</script>`，因为这会导致该字符串被解释为脚本快的结束。
```
document.write("<script></script>");    //错误
document.write("<script><\/script>");    //正确
```
如果在文档加载结束后再调用document.write()，那么输出的内容将会重写整个页面。

### 10.1.3 Element类型
> Element类型用于表现XML或HTML元素，提供了对元素标签名、子节点及特性的访问。Element节点具有以下特征：
* nodeType的值为1；
* nodeName的值为元素的标签名；
* nodeValue的值为null；
* parentNode可能是Document或Element；
* 其子节点可能是Element、Text、Comment、ProcessingInstruction、CDATASection或EntityReference。

要访问元素的标签名，可以使用nodeName属性，也可以使用tagName属性（为了清晰起见）。
```
var div = document.getElementById("div");
console.log(div.tagName == div.nodeName);   //true
console.log(div.tagName);   //"DIV"
```
在HTML中，标签名始终都会以全部大写表示；而在XML（有时候也包括XHTML）中，标签名则始终会与源代码中的保持一致。
```
//使用前先进行判断
if(element.tagName.toLowerCase() === "div"){
    //在此执行操作
}
```
1、HTML元素
> HTMLElement类型直接继承自Element并添加了一些属性，这些属性对应于每个HTML元素都存在的特性，包括id、title、lang（元素内容的语言代码，很少使用）、dir（语言的方向，值为“ltr”——left-to-right，从左至右；或rtl，从右至左。）、className。所有HTML元素都由HTMLElement类型，或它的子类型来表示。

2、取得特性
操作元素特性（针对任何特性，包括标准特性，以HTMLElement类型属性形式定义的特性）的DOM方法主要有三个：`getAttribute()`、`setAttribute()`、`removeAttribute()`。

* 元素属性方法，即.（点）方法，只能获取元素自身的属性。对于自定义属性，会返回undefined。只有公认（非自定义）的特性才会以属性的形式添加到DOM对象中。
```
div.id;     //"myDiv"
div.myAttr;     //undefined(IE除外)
```

* `getAttribute()`方法可以获取HTMLElement类型定义的属性特性，也可以获取自定义特性。
    ```
    div.getAttribute('class');     //box
    div.getAttribute('myAttr');
    ```

* 特性的名称是不区分大小写的，即“ID”和“id”代表的是同一个特性。

* style属性：用属性方法访问返回一个对象，用DOM方法访问返回一个样式的字符串。

* onclick等事件属性：属性访问返回一个function，DOM方法访问返回字符串形式的函数名。

* style和onclick这样的事件处理程序，访问元素属性和通过getAttribute()访问返回的结果不同。由于存在这些差别，所以只有在获取自定义特性值的情况下，才会使用getAttribute()方法。

3、设置特性
* 在js中，用属性（.）方式自定义的属性，就用.方式去获取；用DOM方法设置的自定义属性，就用DOM方法去获取。

* 元素自身的属性，可以用.方式获取和设置，也可以用DOM方法获取和设置。

* 在IE7及以前版本中，setAttribute()方法设置class和style特性没有任何效果。使用这个方法设置事件处理程序特性时也一样。所以还是推荐通过属性来设置特性。

* removeAttribute()方法用于彻底删除元素的特性，包括特性的值和元素的特性。

4、attributes属性
> Element类型是唯一一个使用attributes属性的DOM节点类型。attributes属性中包含一个NamedNodeMap，与NodeList类似。元素的每一个特性都由一个Attr节点表示，每个节点都保存在NamedNodeMap对象中。NamedNodeMap对象拥有下列方法。
* getNamedItem(name)：返回name节点
* removeNamedItem(name)：移除name节点，与removeAttribute()方法的效果相同
* setNamedItem(node)：向列表中添加节点
* item(pos)：返回位于pos位置的节点
attributes属性中包含一系列节点，每个节点的nodeName就是特性的名称，而节点的nodeValue就是特性的值。

```
//获取元素的id特性值的两种方式：
var id = element.attributes.getNamedItem("id").nodeValue;
var id = element.attributes["id"].nodeValue;

//设置
element.attributes["id"].nodeValue = "otherId";
```

5、创建元素
```
document.createElement("div");
//在IE中还可以这样使用:这种方式有助于避开在IE7及更早版本中动态创建元素的某些问题。
document.createElement("<div id=\"myNewDiv\" class=\"box\"></div>");
```

6、元素的子节点
```
<ul>
    <li></li>
    <li></li>
    <li></li>
</ul>
//在IE中会返回3个子节点，分别是3个<li>元素；在其他浏览器中，会返回7个子节点，包括3个<li>元素和4个文本节点。
```

### 10.1.4 Text类型
> 文本节点由Text类型表示，包含的是可以照字面解释的纯文本内容。纯文本中可以包含**转义**后的HTML字符，但不能包含HTML代码。Text节点具有以下特性：
* nodeType的值为3
* nodeName的值为"#text"
* nodeValue 的值为节点所包含的文本
* patentNode是一个Element
* 没有子节点

nodeValue属性和data属性包含的值相同，可以通过这两个属性访问Text节点中包含的文本。使用下列方法可以操作节点中的文本：
* appendData(text)：将text添加到节点的末尾
* deleteData(offset, count)：从offset指定的位置开始删除count个字符
* insertData(offset, text)：在offset指定的位置插入text
* replaceData(offset, count, text)：用text替换从offset指定的位置开始到offset+count为止处的文本
* splitText(offset)：从offset指定的位置将当前文本节点分成两个文本节点
* substringData(offset, count)：提取从offset指定的位置开始到offset+count为止处的字符串

1、创建文本节点
document.createTextNode()。作为参数的文本也将按照HTML或XML的格式进行编码。使用appengChild()把文本节点添加到元素中。

2、规范化文本节点
使用createTextNode()方法创建多个文本，并添加到同一个元素上，这样就存在多个文本节点是相邻的同胞节点。DOM文档中存在相邻的同胞文本节点很容易导致混乱，因为分不清哪个文本节点表示哪个字符串。noramlize()方法是Node类型定义的（因而在所有节点类型中都存在），如果在包含多个文本节点的元素上调用，这回将所有文本节点合并成一个节点。浏览器在解析文档时用于不会创建相邻的文本节点。这种情况只会作为执行DOM操作的结果出现。

### 10.1.5 Comment类型
Comment类型与Text类型继承自相同的基类，因此它拥有除splitText()之外的所有字符串操作方法。
另外，使用document.createComment()并为其传递注释也可以创建注释节点。此外，浏览器不会识别位于`</html>`标签后面的注释。如果需要访问注释节点，一点要保证他们位于`<html></html>`之间。

### 10.1.6 DocumentFragment类型
在所有节点类型中，只有DocumentFragment在文档中没有对应的标记。DOM规定文档偏多是一种“轻量级”的文档，可以包含和控制节点，但不会像完整的文档那样占用额外的资源。
* 使用document.createDocumentGragment()创建文档片段。文档片段继承了Node的所有方法。
* 如果将文档中的节点添加到文档片段中，就会从文档树种移除该节点，也不会从浏览器中再看到该节点。添加到文档片段中的新节点同样也不属于文档树。
* 使用appendChild()或insertBefore()将文档片段中的内容添加到文档中时，实际上只会将文档片段的所有子节点剪切添加到相应位置上；文档片段本身永远不会成为文档树的一部分。

## 10.2 DOM操作技术
### 10.2.1 动态脚本
使用`<script>`元素可以向页面中插入JavaScript代码，一种方式是通过其src特性包含外部文件，另一种方式就是用这个元素本身来包含代码。
* 使用src：在把script标签添加到页面中之前，是不会下载外部文件的。暂时没有办法探知脚本什么时候加载完成。
* 在`<script>`标签内包含：在IE中会导致错误。IE将`<script>`视为一个特殊的元素，不允许DOM访问其子节点。不过可以使用text属性来指定JavaScript代码。
    ```
    function loadScriptString(code){
        var script = document.createElement("script");
        script.type = "text/javascript";
        try {
            script.appendChild(document.createTextNode(code));
        } catch (ex){
            script.text = code;
        }
        document.body.appendChild(script);
    }

    loadScriptString("function sayHi(){alert('hi');}");
    ```
以这种方式执行代码，与在全局作用域中把相同的字符串传给eval()是一样的。

### 10.2.2 动态样式
两种方式创建动态样式：
* 使用`<link>`元素：这种方式动态创建的样式，在所有主流浏览器中都可以正常运行。
* 使用`<style>`元素：这种方式在IE中会报错，与`<script>`类似。解决办法是使用styleSheet属性下的cssText属性来指定样式。
在重用同一个`<style>`元素并再次设置这个属性时，有可能会导致浏览器崩溃。同样，将cssText属性设置为空字符串也可能导致浏览器崩溃。

### 10.2.3 操作表格
HTML DOM为`<table>`、`<tbody>`、`<tr>`元素添加了一些属性和方法。具体可查看第十章DOM->10.2.3 操作表格

### 10.2.4 使用NodeList
理解NodeList、NamedNodeMap和HTMLCollection，是从整体上透彻理解DOM的关键所在。这三个集合都是“动态的”；换句话说，每当文档结构发生变化时，它们都会得到更新。因此，它们始终都会保存着最新、最准确的信息。从本质上说，所有NodeList对象都是访问DOM文档时实时运行的查询。