
---
title: 鲜为人知的HTTP协议头字段详解大全「原创」
date: 2022-06-09 10:44:31
author: Will
img: /images/banner/http.awebp
categories: 网络
tags:
  - 网络
  - 协议
---
        
继上篇讲了[HTTP协议的基础](/2022/06/09/HTTP协议的基础/)之后，本篇重点介绍一下HTTP常用的Header。

HTTP Header非常之多，很少有人能完全分清这些Header到底是干什么的。鉴于RFC文件规范艰深晦涩难懂，本文对协议规范中列出的HTTP Header进行了梳理，用通俗的语言进行表达，便于读者吃透HTTP协议。

作者在阅读RFC文档的时候发现了很多以前没注意到的知识，估计做web开发的小伙伴们也大多忽视了这些知识，阅读文本会给你们带来很多意外的惊喜。

免责声明：如果下面有那句话有不对的地方，还请喷少点口水。




### Accept


表示客户端期望服务器返回的媒体格式。客户端期望的资源类型服务器可能没有，所以客户端会期望多种类型，并且设置优先级，服务器根据优先级寻找相应的资源返回给客户端。

```bash
### 注意：先逗号分割类型，再分号分割属性
Accept: audio/*; q=0.2, audio/basic
```

表示audio/basic类型的资源优先，如果没有，就随便其它什么格式的audio资源都可以。q的取值范围是(0-1]，其具体值并没有意义，它仅用来排序优先级，如果没有q，默认q=1，也就是最高优先级。

### Accept-Charset


表示客户端期望服务器返回的内容的编码格式。它同Accept头一样，也可以指定多个编码，以q值代表优先级。

```bash
### 注意：先逗号分割类型，再分号分割属性
Accept-Charset: utf8, gbk; q=0.6
```

表示utf8编码优先，如果不行，就拿gbk编码返回.

### Content-Type


Content-Type是服务器向客户端发送的头，代表内容的媒体类型和编码格式，是对Accept头和Accept-Charset头的统一应答。

```bash
Content-Type: text/html; charset=utf8
```

表示返回的Body是个html文本，编码为utf8


![](https://p1-jj.byteimg.com/tos-cn-i-t2oaga2asx/gold-user-assets/2018/3/24/162586bc251399fb~tplv-t2oaga2asx-zoom-in-crop-mark:1304:0:0:0.awebp)


### Accept-Language


表示客户端期望服务器返回的内容的语言。很多大型互联网公司是全球化的，它的技术文档一般有有多种语言，通过这个字段可以实现文档的本地化，对国内用户呈现简体中文文档，对英语系用户呈现英文文档。

```bash
Accept-Language:zh-CN,en-US;q=0.8,zh-TW;q=0.6
```

表示大陆简体中文优先，其次英语，再其次台湾繁体中文

### Content-Language


这个头字段内容是对Accept-Language的应答。服务器通过此字段告知客户端返回的Body信息的语言是什么。


![](https://p1-jj.byteimg.com/tos-cn-i-t2oaga2asx/gold-user-assets/2018/3/24/162586d170635751~tplv-t2oaga2asx-zoom-in-crop-mark:1304:0:0:0.awebp)


### Content-Length


表示传输的请求／响应的Body的长度。GET请求因为没有Body，所以不需要这个头。携带Body的并且可以提前知道Body长度的请求／响应必须带上这个字段，以便对方可以方便的分辨出报文的边界，也就是Body数据何时结束。如果Body太大，需要边计算边传输，不到最后计算结束是无法知道整个Body大小的，这个时候可以使用http分块传输，这个时候也是不需要Content-Length字段的。

### Content-Location


当客户端请求的资源在服务器有多个地址时，服务器可以通过Content-Location字段告知客户端其它的可选地址。这个字段比较少见。

### Content-MD5


在Header中提供这个信息是用来做Body内容校验。它表示Body信息被md5算法处理后的base64字符串。这个字段也比较少见。因为校验机制在TCP层已经有实现了，再来一层校验并没有多大意义。另外资源的md5值往往用来放在后面的ETag头信息中作为资源的唯一标识来使用。



### Date


如果服务器没有缓存，那么Date就是响应的即时生成时间。如果服务器设有缓存，那么Date就是响应内容被缓存的时间。它必须符合规范里定义的特定格式，这种格式叫着HTTP-Date，不支持随意定义自己的时间格式。

```bash
Date: Tue, 15 Nov 1994 08:12:31 GMT
```

### Age


表示资源缓存的年龄，也就是资源自缓存以来到现在已经过去了多少时间，单位是秒。

```bash
Age: 86400
```

### Expires


服务器使用Expect头来告知对方资源何时失效。如果它的值等于Date头的值，就表示资源已经实效。

```bash
Expires: Thu, 01 Dec 1994 16:00:00 GMT
```

### ETag


资源标签，每个资源可以提供多个标签信息。它一般用来和下面的If-Match和If-None-Match配合使用，用来判断缓存资源的有效性。比较常见的标签是资源的版本号，比如可以拿资源数据的md5校验码作为版本号。

### If-Match


If-Match的值一般是上面提到的ETag的值，它常用于HTTP的乐观锁。所谓HTTP乐观锁，是指客户端先GET这个资源得到ETag中的版本号，然后发起一个资源修改请求PUT|PATCH时通过If-Match头来指定资源的版本号，如果服务器资源满足If-Match中指定的版本号，请求就会被执行。如果不满足，说明资源被并发修改了，就需要返回状态码为412 Precondition failed 的错误。客户端可以选择放弃或者重试整个过程。

### If-None-Match


类似于If-Match，只是条件相反。


![](https://p1-jj.byteimg.com/tos-cn-i-t2oaga2asx/gold-user-assets/2018/3/24/162586e758846bfa~tplv-t2oaga2asx-zoom-in-crop-mark:1304:0:0:0.awebp)


### Allow


表示资源支持访问的HTTP Method类型。它是服务器对客户端的建议，告知对方请使用Allow中提到的Method来访问资源。

```bash
Allow: GET, HEAD, PUT
```

### Connection


当客户端和服务器需要协商连接的属性时，可以使用Connection头部。比较常用的一个值是close，用来通知对方在当前请求结束后关闭连接。

```bash
Connection: close
```

### Expect


用于请求发送之前向服务器询问许可。譬如要向服务器发送一个很大的文件而不确定是否超出限制，就可以在请求头里携带一个Expect头部

```bash
Expect: 100-continue
```

如果服务器说不行，就会返回417 Expectation Failed错误告知客户端放弃。如果可以那就返回100 continue状态码告知客户端放马过来吧，于是客户端就会继续上传Body内容。如果服务器提前收到Body内容就会放弃返回100 continue响应。

### From


该字段一般用来标记请求发起者的邮件地址，相当于给请求赋予一个责任人。如果服务器发现请求存在问题，就会通过此字段联系到发起人进行处理。因为邮件地址涉及到隐私信息，所以请求携带From头需要征得用户的同意。RFC协议建议所有的机器人代理发起的请求应该携带此头部，以免遇到问题时可以找到责任人。不过如果是恶意的机器人，估计这样的建议也只是耳边风而已。

### Host


RFC协议规定所有的HTTP请求必须携带Host头，即使Host没有值，也必须带上这个Host头附加一个空串，如果不满足，应用服务器应该抛出400 Bad Request。协议虽然这样规定，不过大部分网关或者服务器都比较仁慈，既然没有指定Host字段，那就给你默认加上一个。
网关代理可以根据不同的Host值转发到不同的upstream服务节点，它常用于虚拟主机服务业务。


![](https://p1-jj.byteimg.com/tos-cn-i-t2oaga2asx/gold-user-assets/2018/3/24/162586f22829c30a~tplv-t2oaga2asx-zoom-in-crop-mark:1304:0:0:0.awebp)


### Last-Modified


标记资源的最近修改时间，它和Date比较类似，区别是Last-Modified代表修改时间，而Date是创建时间。

### If-Modified-Since


浏览器向服务器请求静态资源时，如果浏览器本地已经有了缓存，就会携带If-Modified-Since头，值为资源的Last-Modified时间，询问服务器该资源自从这个Last-Modified时间之后有没有被修改。如果没有修改过，就会向浏览器返回304 Not Modified通知浏览器可以放心使用缓存内的资源。如果资源修改过，那就像正常的GET请求一样，携带资源的内容返回200 OK。

### If-Unmodified-Since


类似于If-Modified-Since，意义相反。区别是当服务器资源条件不满足时，不是返回304 Not Modified，而是返回412 Precondition Failed。


![](https://p1-jj.byteimg.com/tos-cn-i-t2oaga2asx/gold-user-assets/2018/3/24/162586fbf1ce6825~tplv-t2oaga2asx-zoom-in-crop-mark:1304:0:0:0.awebp)


### Range


支持断点续传的服务器必须处理Range头，它表示客户端请求资源的一部分时指定的请求字节范围。它是客户端向服务器发送的请求头。

```bash
Range: bytes=500-999
```

### Content-Range


针对上面的Range头，服务器响应客户端时也需提供相应的Content-Range头，表示传输的Body数据在整体资源块中的字节范围。比如下面的例子表示该资源总共有47022字节，当前响应的内容是21010-47021字节之间的内容。

```bash
Content-Range: bytes 21010-47021/47022
```

之所以是47021而不是47022是因为offset是以0开始的，47021就是最后一个字节。

### If-Range


在断点续传时，为确保连续2个请求之间服务器资源本身没有发生变化，需要If-Range头带上ETag的资源版本号。服务器资源根据这个版本号来判定资源是否改变了。如果没变，就返回206 Partial Content将部分资源返回。如果资源变了，那就相当于一个普通的GET请求，返回200 OK和整个资源内容。


![](https://p1-jj.byteimg.com/tos-cn-i-t2oaga2asx/gold-user-assets/2018/3/24/16258703f0d9dbfe~tplv-t2oaga2asx-zoom-in-crop-mark:1304:0:0:0.awebp)


### Location


服务器向客户端发送302跳转的时候，总会携带Location头信息，它的值为目标URL。

```bash
HTTP/1.1 302 Temporary Redirect
Location: https://www-temp.example.org/
```

### Max-Forwards


用来限定网关或者代理的层数，也就是最大转发次数。HTTP每经过一个网关或者代理层，Max-Forwards值就要减1。如果nginx接收到前端请求的时候Max-Forwards已经等于0，那么它就不应该再将请求转发到upstream指定的服务节点上。

### Pragma


这个头是比较常见的，在前端开发模式下经常会加上这个头部。

```bash
Pragma: no-cache
```

当网关收到一个带有这样请求的头部时，即使内部存在该请求资源的缓存并且有效也不可以直接发送给客户端，而必须转发给后面的upstream进行处理。
不过如果真的所有的网关都遵循这个协议的话，攻击是很容易构造的，所以它一般仅用于开发模式，防止静态资源修改后前端得不到即时更新。其它值的pragma值没有遇到过。

### Referer


Referer是非常常用的头，它表示请求的发起来源URI，也就是当前页面资源的父页面。如果你从A页面跳转到B页面，那么请求B页面的请求头里面就会有Referer信息，它的值就是A页面的访问地址。通过追踪Referer，可得出资源页面之间复杂的跳转链，它非常适合用于网页的数据分析和路径优化。


![](https://p1-jj.byteimg.com/tos-cn-i-t2oaga2asx/gold-user-assets/2018/3/24/16257fe9b60eb4c0~tplv-t2oaga2asx-zoom-in-crop-mark:1304:0:0:0.awebp)


### Retry-After


服务器升级时，来自客户端的请求会直接给予503(Service Unavailable)错误，通过在返回头里面加入Retry-After字段告知客户端何时服务可以恢复正常访问。Retry-After的头可以是HTTP-Date，也可以是整数，表示多少秒后服务可以恢复正常访问。浏览器在拿到这个值之后可以考虑增加一个定时器在未来的某个时间进行重试。

### Server


用于返回服务器相关的软件信息，来告知客户端当前的HTTP服务是由某某软件提供的，可以看成是一种软件广告。
RFC协议里对这个头信息做了警告：暴露出服务器信息可能会导致黑客更易于攻击你的服务，建议谨慎使用。

### User-Agent


携带当前的用户代理信息，一般包含浏览器、浏览器内核和操作系统的版本型号信息。它和Server头是对应的，一个是表达服务器信息，一个是表达客户端信息。服务器可以根据用户代理信息统计出网页服务的浏览器、操作系统的使用占比情况，服务器也可以根据UA的信息来定制不一样的内容。


![](https://p1-jj.byteimg.com/tos-cn-i-t2oaga2asx/gold-user-assets/2018/3/24/16257d74bf4f123a~tplv-t2oaga2asx-zoom-in-crop-mark:1304:0:0:0.awebp)


### Transfer-Encoding


传送Body信息时需要对Body数据采取何种变换。当HTTP对Body进行分块传送时，需要增加下面的头部信息才可以进行分块传送。其它类型目前没有遇到过。

```bash
Transfer-Encoding: chunked
```

### Upgrade


服务器建议客户端升级传输协议。比如当客户端使用HTTP/1.0发送请求时，服务器就可以建议客户端升级到HTTP/1.1。
这个时候就可以使用Upgrade头。客户端收到这个Upgrade后就会将后续请求转成HTTP/1.1格式继续进行交流。可以支持多个参数，使用逗号分割即可。

```bash
Upgrade: HTTP/1.1
```

当客户端要和服务器进行Websocket进行通讯时，在握手阶段服务器也会向客户端发送Upgrade头部信息，提示客户端将协议切换到Websocket。

```bash
Upgrade: WebSocket
```

### Vary


该头部用于缓存控制。对于一些缓存服务器，我们在请求里加入Vary参数可以告知缓存服务器对不同的Vary参数的响应使用不同的缓存单元。比如Vary参数里放入编码参数，那么不同编码的网页就会有不同的缓存。Vary的值可以有多个，只要任意一个值不一样就会有不同的缓存。
比如下面的这个例子告知缓存服务器对不同语言和不同编码的网页响应使用不同的缓存单元。

```bash
Vary: Accept-Encoding,Accept-Language
```

### Via


该字段用来标识一个请求经过的网关路由节点。如果这个请求经过了多个代理层，Via头部就会有多个网关信息。

### Warning


用于在响应中添加一些附加的警告信息，警告信息包含一个错误码和错误说明。通用的一些错误码在RFC协议中有具体规定。比如111号错误码表示缓存服务器的缓存项目已经过期，并且尝试reload资源，但是reload失败了，所以只好返回了旧的已经过期的内容，这个时候就需要通过warning头反馈客户端。

```bash
Warning: 111 Revalidation failed
```

### WWW-Authenticate


WWW-Authenticate是401 Unauthorized错误码返回时必须携带的头，该头会携带一个问题Challenge给客户端，告知客户端需要携带这个问题的答案来请求服务器才可以继续访问目标资源。这种问题Challenge可以自定义，比较常见的是Basic认证。

```bash
WWW-Authenticate: Basic realm=xxx
```

Basic指代base64加密算法(不安全)，realm指代认证范围/场合/情景名称。

### Authorization


对于某些需要特殊权限才能访问的资源需要客户端在请求里提供用户名密码的认证信息。它是对WWW-Authenticate的应答。

```bash
### value = base64(user_name:password)
Authorization: Basic YWRtaW46YWRtaW4xMjM=
```


![](https://p1-jj.byteimg.com/tos-cn-i-t2oaga2asx/gold-user-assets/2018/3/24/1625870c7cc549fb~tplv-t2oaga2asx-zoom-in-crop-mark:1304:0:0:0.awebp)


### Proxy-Authenticate


同WWW-Authorization头部，用于代理服务器认证。

### Proxy-Authorization


同Authorization头部，用于代理服务器认证。

### ETag vs Last-Modified vs Expires


ETag一般携带的是资源的版本号，协议没有具体规定版本号是什么。它可以是资源的md5校验码，也可以是uuid，甚至可以是自增的数字，也可以是资源的修改时间。它的匹配方式是相等/不相等。因为服务器需要维护版本号，取决的版本号是什么，这可能是一个存储和计算的负担。

Last-Modified携带的资源的修改时间。它的匹配方式是大于/小于。如果是静态资源文件，一般就是操作系统记录的文件修改时间。

Expires是服务器告知客户端资源的过期时间。客户端缓存的资源在这个时间之后自动过期，而不需要非得向服务器确认一下是不是304 Not Modified才认为没过期。

### Cache-Control


这可能是HTTP头里面最复杂的一个头了。这个头既可以用于请求，也可以用于响应。在请求和响应的取值不一样，分别代表了不同的意思。



+ no-cache 如果no-cache没有指定值，那就表示不允许缓存。对于请求来说，服务器不得使用缓存内容直接返回。对于响应来说，客户端不得缓存响应的资源内容。如果no-cache指定了值，那就表示值对应的头信息不得使用缓存，其它的信息还是可以缓存的。告知对方我只要新鲜刚出浴的数据。

+ no-store 告知对方不要持久化请求/响应数据到其它地方，这种信息是敏感的，要保持它的易失性。告知对方记在心里(memory)就行，别写在纸上(disk)。

+ no-transform 告知对方不要转换数据。比如客户端上传了raw图像数据，服务器一般都会选择性压缩图像数据进行存储。no-transform告知对方保留原始数据信息，不要进行任何转换。告知对方不要乱动我发过来的东西。

+ only-if-cached 用于请求头，告知服务器只要那些已经缓存的内容，不要去reload。如果没有缓存内容就返回504 Gateway Timeout错误。表示客户端不想太麻烦服务器，有就给，没就算了。

+ max-age 用于请求头。限制缓存内容的年龄，如果超过max-age年龄的，需要服务器去reload内容资源。这叫客户端的年龄歧视。

+ max-stale 用于请求头。客户端允许服务器返回缓存已过期的资源内容，但是限定了最大过期时间。表示客户端虽然很宽容，那是也是有限度的。

+ min-fresh 用于请求头。客户端限制服务器不要那些即将过期的资源内容。就好比我们去超市买牛奶，如果牛奶快过期了虽然还在保质期内咱们也就不会考虑。

+ public 用于响应头。表示允许客户端缓存响应信息，并可以给别人使用。比如代理服务器缓存静态资源供所有代理用户使用。

+ private 用于响应头。表示仅允许客户端缓存响应信息给自己使用，不得分享给别人。这样是为了禁止代理服务器进行缓存，而允许客户端自己缓存资源内容。意思是你个人留着用就行，别借给别人用。

