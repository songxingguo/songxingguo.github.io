title: 《深入浅出Node.js》-读书笔记-理解Buffer
author: songxingguo
tags:
  - Node
categories:
  - 读书笔记
date: 2018-10-06 11:23:00
---
## 理解Buffer

JavaScript对于字符串（string）的操作十分友好，无论是宽字节字符串还是单字节字符串，都被认为是一个字符串。示例代码如下所示：

```js
console.log("0123456789".length); // 10
console.log("零一二三四五六七八九".length); //10
console.log("\u00bd".length); // 1
```
<!-- more -->

对比PHP中的字符串统计，我们需要动用额外的函数来获取字符串的长度。示例代码如下所示：

```php
<?phpecho strlen("0123456789"); // 10
echo "\n";echo strlen("零一二三四五六七八九"); // 30
echo "\n";echo mb_strlen("零一二三四五六七八九", "utf-8"); //10
echo "\n";?>
```

文件和网络I/O对于前端开发者而言都是不曾有的应用场景，因为前端只需做一些简单的字符串操作或DOM操作基本就能满足业务需求，在ECMAScript规范中，也没有对这些方面做任何的定义，只有CommonJS中有部分二进制的定义。由于应用场景不同，在Node中，应用需要处理网络协议、操作数据库、处理图片、接收上传文件等，在网络流和文件的操作中，还要处理大量二进制数据，JavaScript自有的字符串远远不能满足这些需求，于是Buffer对象应运而生。

### Buffer结构

Buffer是一个像Array的对象，但它主要用于操作字节。下面我们从模块结构和对象结构的层面上来认识它。

#### 模块结构

Buffer是一个典型的JavaScript与C++结合的模块，它将性能相关部分用C++实现，将非性能相关的部分用JavaScript实现，如下图所示。

![Buffer分工](http://p9myzkds7.bkt.clouddn.com/Node-Buffer/Buffer%E5%88%86%E5%B7%A5.jpg)

第5章揭示了Buffer所占用的内存不是通过V8分配的，属于堆外内存。由于V8垃圾回收性能的影响，将常用的操作对象用更高效和专有的内存分配回收策略来管理是个不错的思路。

由于Buffer太过常见，Node在进程启动时就已经加载了它，并将其放在全局对象（global）上。所以 **在使用Buffer时，无须通过require()即可直接使用。**

#### Buffer对象

Buffer对象类似于数组，它的元素为16进制的两位数，即0到255的数值。示例代码如下所示：

```js
var str = "深入浅出node.js";
var buf = new Buffer(str, 'utf-8');
console.log(buf);
// => <Buffer e6 b7 b1 e5 85 a5 e6 b5 85 e5 87 ba 6e 6f 64 65 2e 6a 73>
```
由上面的示例可见，不同编码的字符串占用的元素个数各不相同，上面代码中的中文字在UTF-8编码下占用3个元素，字母和半角标点符号占用1个元素。Buffer受Array类型的影响很大，可以访问length属性得到长度，也可以通过下标访问元素，在构造对象时也十分相似，代码如下：

```js
var buf = new Buffer(100);
console.log(buf.length); // => 100
```
上述代码分配了一个长100字节的Buffer对象。可以通过下标访问刚初始化的Buffer的元素，代码如下：

```js
console.log(buf[10]);
```
这里会得到一个比较奇怪的结果，它的元素值是一个0到255的随机值。同样，我们也可以通过下标对它进行赋值：

```js
buf[10] = 100;
console.log(buf[10]); // => 100
```
值得注意的是，如果给元素赋值不是0到255的整数而是小数时会怎样呢？示例代码如下所示：

```js
buf[20] = -100;
console.log(buf[20]); // 156
buf[21] = 300;
console.log(buf[21]); // 44
buf[22] = 3.1415;
console.log(buf[22]); // 3
```
给元素的赋值如果小于0，就将该值逐次加256，直到得到一个0到255之间的整数。如果得到的数值大于255，就逐次减256，直到得到0~255区间内的数值。如果是小数，舍弃小数部分，只保留整数部分。

### Buffer内存分配

Buffer对象的内存分配不是在V8的堆内存中，而是在Node的C++层面实现内存的申请的。因为处理大量的字节数据不能采用需要一点内存就向操作系统申请一点内存的方式，这可能造成大量的内存申请的系统调用，对操作系统有一定压力。为此Node在内存的使用上应用的是在C++层面申请内存、在JavaScript中分配内存的策略。

为了高效地使用申请来的内存，Node采用了slab分配机制。slab是一种动态内存管理机制，最早诞生于SunOS操作系统（Solaris）中，目前在一些*nix操作系统中有广泛的应用，如FreeBSD和Linux。

简单而言，slab就是一块申请好的固定大小的内存区域。slab具有如下3种状态。

- full：完全分配状态。
- partial：部分分配状态。
- empty：没有被分配状态。

当我们需要一个Buffer对象，可以通过以下方式分配指定大小的Buffer对象：

```js
new Buffer(size);
```
Node以8 KB为界限来区分Buffer是大对象还是小对象：

```js
Buffer.poolSize = 8 * 1024;
```
这个8 KB的值也就是每个slab的大小值，在JavaScript层面，以它作为单位单元进行内存的分配。

1. 分配小Buffer对象

如果指定Buffer的大小少于8 KB，Node会按照小对象的方式进行分配。Buffer的分配过程中主要使用一个局部变量pool作为中间处理对象，处于分配状态的slab单元都指向它。以下是分配一个全新的slab单元的操作，它会将新申请的SlowBuffer对象指向它：

```js
var pool;
function allocPool() {
  pool = new SlowBuffer(Buffer.poolSize); 
  pool.used = 0;
}
```
下图为一个新构造的slab单元示例。

![新构造的slab单元示例](http://p9myzkds7.bkt.clouddn.com/Node-Buffer/%E6%96%B0%E6%9E%84%E9%80%A0%E7%9A%84slab%E5%8D%95%E5%85%83%E7%A4%BA%E4%BE%8B.jpg)

在上图中，slab处于empty状态。

构造小Buffer对象时的代码如下：

```js
new Buffer(1024);
```
这次构造将会去检查pool对象，如果pool没有被创建，将会创建一个新的slab单元指向它：

```js
if (!pool || pool.length - pool.used < this.length) allocPool();
```
同时当前Buffer对象的parent属性指向该slab，并记录下是从这个slab的哪个位置（offset）开始使用的，slab对象自身也记录被使用了多少字节，代码如下：

```
this.parent = pool;
this.offset = pool.used;
pool.used += this.length;
if (pool.used & 7) pool.used = (pool.used + 8) & ~7;
```
下图为从一个新的slab单元中初次分配一个Buffer对象的示意图。

![从一个新的slab单元中初次分配一个Buffer对象的示意图](http://p9myzkds7.bkt.clouddn.com/Node-Buffer/%E4%BB%8E%E4%B8%80%E4%B8%AA%E6%96%B0%E7%9A%84slab%E5%8D%95%E5%85%83%E4%B8%AD%E5%88%9D%E6%AC%A1%E5%88%86%E9%85%8D%E4%B8%80%E4%B8%AABuffer%E5%AF%B9%E8%B1%A1%E7%9A%84%E7%A4%BA%E6%84%8F%E5%9B%BE.jpg)

这时候的slab状态为partial。

当再次创建一个Buffer对象时，构造过程中将会判断这个slab的剩余空间是否足够。如果足够，使用剩余空间，并更新slab的分配状态。下面的代码创建了一个新的Buffer对象，它会引起一次slab分配：

```js
new Buffer(3000);
```
下图为再次分配的示意图。

![从slab单元中再次分配一个Buffer对象的示意图](http://p9myzkds7.bkt.clouddn.com/Node-Buffer/%E4%BB%8Eslab%E5%8D%95%E5%85%83%E4%B8%AD%E5%86%8D%E6%AC%A1%E5%88%86%E9%85%8D%E4%B8%80%E4%B8%AABuffer%E5%AF%B9%E8%B1%A1%E7%9A%84%E7%A4%BA%E6%84%8F%E5%9B%BE.jpg)

如果slab剩余的空间不够，将会构造新的slab，原slab中剩余的空间会造成浪费。例如，第一次构造1字节的Buffer对象，第二次构造8192字节的Buffer对象，由于第二次分配时slab中的空间不够，所以创建并使用新的slab，第一个slab的8 KB将会被第一个1字节的Buffer对象独占。下面的代码一共使用了两个slab单元：

```js
new Buffer(1);new Buffer(8192);
```
这里要注意的事项是，由于同一个slab可能分配给多个Buffer对象使用，只有这些小Buffer对象在作用域释放并都可以回收时，slab的8 KB空间才会被回收。尽管创建了1个字节的Buffer对象，但是如果不释放它，实际可能是8 KB的内存没有释放。

2. 分配大Buffer对象

如果需要超过8 KB的Buffer对象，将会直接分配一个SlowBuffer对象作为slab单元，这个slab单元将会被这个大Buffer对象独占。

```js
// Big buffer, just alloc one
this.parent = new SlowBuffer(this.length);
this.offset = 0;
```
这里的SlowBuffer类是在C++中定义的，虽然引用buffer模块可以访问到它，但是不推荐直接操作它，而是用Buffer替代。上面提到的Buffer对象都是JavaScript层面的，能够被V8的垃圾回收标记回收。但是其内部的parent属性指向的SlowBuffer对象却来自于Node自身C++中的定义，是C++层面上的Buffer对象，所用内存不在V8的堆中。

3. 小结

简单而言，真正的内存是在Node的C++层面提供的，JavaScript层面只是使用它。当进行小而频繁的Buffer操作时，采用slab的机制进行预先申请和事后分配，使得JavaScript到操作系统之间不必有过多的内存申请方面的系统调用。对于大块的Buffer而言，则直接使用C++层面提供的内存，而无需细腻的分配操作。

### Buffer的转换

Buffer对象可以与字符串之间相互转换。目前支持的字符串编码类型有如下这几种。

- ASCII
- UTF-8
- UTF-16LE/UCS-2
- Base64
- Binary
- Hex

#### 字符串转Buffer

字符串转Buffer对象主要是通过构造函数完成的：

```js
new Buffer(str, [encoding]);
```
通过构造函数转换的Buffer对象，存储的只能是一种编码类型。encoding参数不传递时，默认按UTF-8编码进行转码和存储。

一个Buffer对象可以存储不同编码类型的字符串转码的值，调用write()方法可以实现该目的，代码如下：

```
buf.write(string, [offset], [length], [encoding])
```
由于可以不断写入内容到Buffer对象中，并且每次写入可以指定编码，所以Buffer对象中可以存在多种编码转化后的内容。需要小心的是，每种编码所用的字节长度不同，将Buffer反转回字符串时需要谨慎处理。

#### Buffer转字符串

实现Buffer向字符串的转换也十分简单，Buffer对象的toString()可以将Buffer对象转换为字符串，代码如下：

```js
buf.toString([encoding], [start], [end])
```
比较精巧的是，可以设置encoding（默认为UTF-8）、start、end这3个参数实现整体或局部的转换。如果Buffer对象由多种编码写入，就需要在局部指定不同的编码，才能转换回正常的编码。

#### Buffer不支持的编码类型

目前比较遗憾的是，Node的Buffer对象支持的编码类型有限，只有少数的几种编码类型可以在字符串和Buffer之间转换。为此，Buffer提供了一个isEncoding()函数来判断编码是否支持转换：

```js
Buffer.isEncoding(encoding)
```
将编码类型作为参数传入上面的函数，如果支持转换返回值为true，否则为false。很遗憾的是，在中国常用的GBK、GB2312和BIG-5编码都不在支持的行列中。

对于不支持的编码类型，可以借助Node生态圈中的模块完成转换。iconv和iconv-lite两个模块可以支持更多的编码类型转换，包括Windows 125系列、ISO-8859系列、IBM/DOS代码页系列、Macintosh系列、KOI8系列，以及Latin1、US-ASCII，也支持宽字节编码GBK和GB2312。iconv-lite采用纯JavaScript实现，iconv则通过C++调用libiconv库完成。前者比后者更轻量，无须编译和处理环境依赖直接使用。在性能方面，由于转码都是耗用CPU，在V8的高性能下，少了C++到JavaScript的层次转换，纯JavaScript的性能比C++实现得更好。

以下为iconv-lite的示例代码：

```js
var iconv = require('iconv-lite');
// Buffer转字符串
var str = iconv.decode(buf, 'win1251');
// 字符串转Buffervar buf = iconv.encode("Sample input string", 'win1251');
```
另外，iconv和iconv-lite对无法转换的内容进行降级处理时的方案不尽相同。iconv-lite无法转换的内容如果是多字节，会输出<图>；如果是单字节，则输出?。iconv则有三级降级策略，会尝试翻译无法转换的内容，或者忽略这些内容。如果不设置忽略，iconv对于无法转换的内容将会得到EILSEQ异常。如下是iconv的示例代码兼选项设置方式：

```js
var iconv = new Iconv('UTF-8', 'ASCII');
iconv.convert('ça va'); // throws EILSEQ

var iconv = new Iconv('UTF-8', 'ASCII//IGNORE');iconv.convert('ça va'); // returns "a va"

var iconv = new Iconv('UTF-8', 'ASCII//TRANSLIT');
iconv.convert('ça va'); // "ca va"

var iconv = new Iconv('UTF-8', 'ASCII//TRANSLIT//IGNORE');iconv.convert('ça va が'); // "ca va "
```

### Buffer的拼接

Buffer在使用场景中，通常是以一段一段的方式传输。以下是常见的从输入流中读取内容的示例代码：

```js
var fs = require('fs');
var rs = fs.createReadStream('test.md');
var data = '';
rs.on("data",
function(trunk) {
  data += trunk;
});
rs.on("end",
function() {
  console.log(data);
});
```
上面这段代码常见于国外，用于流读取的示范，data事件中获取的chunk对象即是Buffer对象。对于初学者而言，容易将Buffer当做字符串来理解，所以在接受上面的示例时不会觉得有任何异常。一旦输入流中有宽字节编码时，问题就会暴露出来。如果你在通过Node开发的网站上看到<图>乱码符号，那么该问题的起源多半来自于这里。
这里潜藏的问题在于如下这句代码：

```js
data += trunk;
```
这句代码里隐藏了toString()操作，它等价于如下的代码：

```js
data = data.toString() + trunk.toString();
```
值得注意的是，外国人的语境通常是指英文环境，在他们的场景下，这个toString()不会造成任何问题。但对于宽字节的中文，却会形成问题。为了重现这个问题，下面我们模拟近似的场景，将文件可读流的每次读取的Buffer长度限制为11，代码如下：

```js
var rs = fs.createReadStream('test.md', {highWaterMark: 11});
```
搭配该代码的测试数据为李白的《静夜思》。执行该程序，将会得到以下输出：

```
床前明<图><图><图>光，疑<图><图><图>地上霜；举头<图><图><图>明月，<图><图><图>头思故乡。
```
#### 乱码是如何产生的

上面的诗歌中，“月”、“是”、“望”、“低”4个字没有被正常输出，取而代之的是3个<图>。产生这个输出结果的原因在于文件可读流在读取时会逐个读取Buffer。这首诗的原始Buffer应存储为：

```
<Buffer e5 ba 8a e5 89 8d e6 98 8e e6 9c 88 e5 85 89 ef bc 8c e7 96 91 e6 98 af e5 9c b0 e4 b8 8a e9 9c 9c ef bc 9b e4 b8 be e5 a4 b4 e6 9c 9b e6 98 8e e6 9c 88 ...>
```
由于我们限定了Buffer对象的长度为11，因此只读流需要读取7次才能完成完整的读取，结果是以下几个Buffer对象依次输出：

```
<Buffer e5 ba 8a e5 89 8d e6 98 8e e6 9c><Buffer 88 e5 85 89 ef bc 8c e7 96 91 e6>
...
```
上文提到的buf.toString()方法默认以UTF-8为编码，中文字在UTF-8下占3个字节。所以第一个Buffer对象在输出时，只能显示3个字符，Buffer中剩下的2个字节（e6 9c）将会以乱码的形式显示。第二个Buffer对象的第一个字节也不能形成文字，只能显示乱码。于是形成一些文字无法正常显示的问题。

在这个示例中我们构造了11这个限制，但是对于任意长度的Buffer而言，宽字节字符串都有可能存在被截断的情况，只不过Buffer的长度越大出现的概率越低而已，但该问题依然不可忽视。

#### setEncoding()与string_decoder()

在看过上述的示例后，也许我们忘记了可读流还有一个设置编码的方法setEncoding()，示例如下：

```js
readable.setEncoding(encoding)
```
该方法的作用是让data事件中传递的不再是一个Buffer对象，而是编码后的字符串。为此，我们继续改进前面诗歌的程序，添加setEncoding()的步骤如下：

```js
var rs = fs.createReadStream('test.md', { highWaterMark: 11});rs.setEncoding('utf8');
```
重新执行程序，得到输出：


床前明月光，疑是地上霜；举头望明月，低头思故乡。


这是令人开心的输出结果，说明输出不再受Buffer大小的影响了。那Node是如何实现这个输出结果的呢？

要知道，无论如何设置编码，触发data事件的次数依旧相同，这意味着设置编码并未改变按段读取的基本方式。

事实上，在调用setEncoding()时，可读流对象在内部设置了一个decoder对象。每次data事件都通过该decoder对象进行Buffer到字符串的解码，然后传递给调用者。是故设置编码后，data不再收到原始的Buffer对象。但是这依旧无法解释为何设置编码后乱码问题被解决掉了，因为在前述分析中，无论如何转码，总是存在宽字节字符串被截断的问题。

最终乱码问题得以解决，还是在于decoder的神奇之处。decoder对象来自于string_decoder模块StringDecoder的实例对象。它神奇的原理是什么，下面我们以代码来说明：

```js
var StringDecoder = require('string_decoder').StringDecoder;var decoder = new StringDecoder('utf8');
var buf1 = new Buffer([0xE5, 0xBA, 0x8A, 0xE5, 0x89, 0x8D, 0xE6, 0x98, 0x8E, 0xE6, 0x9C]);
console.log(decoder.write(buf1));// => 床前明
var buf2 = new Buffer([0x88, 0xE5, 0x85, 0x89, 0xEF, 0xBC, 0x8C, 0xE7, 0x96, 0x91, 0xE6]);
console.log(decoder.write(buf2));// => 月光，疑
```
我将前文提到的前两个Buffer对象写入decoder中。奇怪的地方在于“月”的转码并没有如平常一样在两个部分分开输出。StringDecoder在得到编码后，知道宽字节字符串在UTF-8编码下是以3个字节的方式存储的，所以第一次write()时，只输出前9个字节转码形成的字符，“月”字的前两个字节被保留在StringDecoder实例内部。第二次write()时，会将这2个剩余字节和后续11个字节组合在一起，再次用3的整数倍字节进行转码。于是乱码问题通过这种中间形式被解决了。

虽然string_decoder模块很奇妙，但是它也并非万能药，它目前只能处理UTF-8、Base64和UCS-2/UTF-16LE这3种编码。所以，通过setEncoding()的方式不可否认能解决大部分的乱码问题，但并不能从根本上解决该问题。

#### 正确拼接Buffer

淘汰掉setEncoding()方法后，剩下的解决方案只有将多个小Buffer对象拼接为一个Buffer对象，然后通过iconv-lite一类的模块来转码这种方式。+=的方式显然不行，那么正确的Buffer拼接方法应该如下面展示的形式：

```js
var chunks = [];
var size = 0;
res.on('data',
function(chunk) {
  chunks.push(chunk);
  size += chunk.length;
});
res.on('end',
function() {
  var buf = Buffer.concat(chunks, size);
  var str = iconv.decode(buf, 'utf8');
  console.log(str);
});
```
正确的拼接方式是用一个数组来存储接收到的所有Buffer片段并记录下所有片段的总长度，然后调用Buffer.concat()方法生成一个合并的Buffer对象。Buffer.concat()方法封装了从小Buffer对象向大Buffer对象的复制过程，实现十分细腻，值得围观学习：

```js
Buffer.concat = function(list, length) {
  if (!Array.isArray(list)) {
    throw new Error('Usage: Buffer.concat(list, [length])');
  }
  if (list.length === 0) {
    return new Buffer(0);
  } else if (list.length === 1) {
    return list[0];
  }
  if (typeof length !== 'number') {
    length = 0;
    for (var i = 0; i < list.length; i++) {
      var buf = list[i];
      length += buf.length;
    }
  }
  var buffer = new Buffer(length);
  var pos = 0;
  for (var i = 0; i < list.length; i++) {
    var buf = list[i];
    buf.copy(buffer, pos);
    pos += buf.length;
  }
  return buffer;
};
```
### Buffer与性能

Buffer在文件I/O和网络I/O中运用广泛，尤其在网络传输中，它的性能举足轻重。在应用中，我们通常会操作字符串，但一旦在网络中传输，都需要转换为Buffer，以进行二进制数据传输。在Web应用中，字符串转换到Buffer是时时刻刻发生的，提高字符串到Buffer的转换效率，可以很大程度地提高网络吞吐率。

在展开Buffer与网络传输的关系之前，我们可以先来进行一次性能测试。下面的例子中构造了一个10 KB大小的字符串。我们首先通过纯字符串的方式向客户端发送，代码如下：

```js
var http = require('http');
var helloworld = "";
for (var i = 0; i < 1024 * 10; i++) {
  helloworld += "a";
}
// helloworld = new Buffer(helloworld);
http.createServer(function(req, res) {
  res.writeHead(200);
  res.end(helloworld);
}).listen(8001);
```
我们通过ab进行一次性能测试，发起200个并发客户端：

```
ab -c 200 -t 100 
http://127.0.0.1:8001/
```
得到的测试结果如下所示：

```
HTML transferred: 512000000 bytesRequests per second: 2527.64 [#/sec] (mean)
Time per request: 79.125 [ms] (mean)
Time per request: 0.396 [ms] (mean, across all concurrent requests)
Transfer rate: 25370.16 [Kbytes/sec] received
```
测试的QPS（每秒查询次数）是2527.64，传输率为每秒25 370.16 KB。

接下来我们注释掉helloworld = new Buffer(helloworld);使向客户端输出的是一个Buffer对象，无须在每次响应时进行转换。再次进行性能测试的结果如下所示：

```js
Total transferred: 513900000 bytes
HTML transferred: 512000000 bytesRequests per second: 4843.28 [#/sec] (mean)
Time per request: 41.294 [ms] (mean)
Time per request: 0.206 [ms] (mean, across all concurrent requests)
Transfer rate: 48612.56 [Kbytes/sec] received
```
QPS的提升到4843.28，传输率为每秒48 612.56 KB，性能提高近一倍。

通过预先转换静态内容为Buffer对象，可以有效地减少CPU的重复使用，节省服务器资源。在Node构建的Web应用中，可以选择将页面中的动态内容和静态内容分离，静态内容部分可以通过预先转换为Buffer的方式，使性能得到提升。由于文件自身是二进制数据，所以在不需要改变内容的场景下，尽量只读取Buffer，然后直接传输，不做额外的转换，避免损耗。

文件读取

Buffer的使用除了与字符串的转换有性能损耗外，在文件的读取时，有一个highWaterMark设置对性能的影响至关重要。在fs.createReadStream(path, opts)时，我们可以传入一些参数，代码如下：

```js
{
  flags: 'r',
  encoding: null,
  fd: null,
  mode: 0666,
  highWaterMark: 64 * 1024
}
```
我们还可以传递start和end来指定读取文件的位置范围：
```
{start: 90, end: 99}
```
fs.createReadStream()的工作方式是在内存中准备一段Buffer，然后在fs.read()读取时逐步从磁盘中将字节复制到Buffer中。完成一次读取时，则从这个Buffer中通过slice()方法取出部分数据作为一个小Buffer对象，再通过data事件传递给调用方。如果Buffer用完，则重新分配一个；如果还有剩余，则继续使用。下面为分配一个新的Buffer对象的操作：

```
var pool;
function allocNewPool(poolSize) {
  pool = new Buffer(poolSize);
  pool.used = 0;
}
```
在理想的状况下，每次读取的长度就是用户指定的highWaterMark。但是有可能读到了文件结尾，或者文件本身就没有指定的highWaterMark那么大，这个预先指定的Buffer对象将会有部分剩余，不过好在这里的内存可以分配给下次读取时使用。pool是常驻内存的，只有当pool单元剩余数量小于128（kMinPoolSpace）字节时，才会重新分配一个新的Buffer对象。Node源代码中分配新的Buffer对象的判断条件如下所示：

```js
if (!pool || pool.length - pool.used < kMinPoolSpace) { // discard the old pool
  pool = null;
  allocNewPool(this._readableState.highWaterMark);
}
```
这里与Buffer的内存分配比较类似，highWaterMark的大小对性能有两个影响的点。

- highWaterMark设置对Buffer内存的分配和使用有一定影响。
- highWaterMark设置过小，可能导致系统调用次数过多。

文件流读取基于Buffer分配，Buffer则基于SlowBuffer分配，这可以理解为两个维度的分配策略。如果文件较小（小于8 KB），有可能造成slab未能完全使用。

由于fs.createReadStream()内部采用fs.read()实现，将会引起对磁盘的系统调用，对于大文件而言，highWaterMark的大小决定会触发系统调用和data事件的次数。

以下为Node自带的基准测试，在benchmark/fs/read-stream-throughput.js中可以找到：

```js
function runTest() {
  assert(fs.statSync(filename).size === filesize);
  var rs = fs.createReadStream(filename, {
    highWaterMark: size,
    encoding: encoding
  });
  rs.on('open',
  function() {
    bench.start();
  });
  var bytes = 0;
  rs.on('data',
  function(chunk) {
    bytes += chunk.length;
  });
  rs.on('end',
  function() {
    try {
      fs.unlinkSync(filename);
    } catch(e) {}
    // MB/sec bench.end(bytes / (1024 * 1024));
  });
}
```
下面为某次执行的结果：

```
fs/read-stream-throughput.js type=buf size=1024: 46.284
fs/read-stream-throughput.js type=buf size=4096: 139.62
fs/read-stream-throughput.js type=buf size=65535: 681.88
fs/read-stream-throughput.js type=buf size=1048576: 857.98
```
从上面的执行结果我们可以看到，读取一个相同的大文件时，highWaterMark值的大小与读取速度的关系：该值越大，读取速度越快。

### 总结

体验过JavaScript友好的字符串操作后，有些开发者可能会形成思维定势，将Buffer当做字符串来理解。但字符串与Buffer之间有实质上的差异，即Buffer是二进制数据，字符串与Buffer之间存在编码关系。因此，理解Buffer的诸多细节十分必要，对于如何高效处理二进制数据十分有用。