---
title: 油猴脚本编写教程
date: 2022-06-24 12:57:28
author: Will
img: /images/banner/tampermonkey.png
categories: 其他
tags:
  - 油猴脚本
  - JS
---


油猴脚本（Tampermonkey）是一个非常流行的浏览器扩展，它可以运行由广大社区编写的扩展脚本，来实现各式各样的功能，常见的去广告、修改样式文件、甚至是下载视频。今天我们就来看看如何编写自己的油猴脚本。当然为了运行油猴脚本，你应该在浏览器中安装油猴插件。

### 安装油猴插件

安装油猴插件非常简单，直接在浏览器的扩展商店中安装即可。国产浏览器的话一般可以通过下载扩展文件手动拖动的方式来安装。下图是微软新版Edge浏览器的扩展商店，直接搜索Tampermonkey即可。

![新版Edge扩展商店](/images/油猴脚本编写教程-SegmentFault思否/3843056900-8983f8db535e19d8_fix732.png)

### 新建脚本

首先在浏览器右上角找到并点击油猴插件，选择添加新脚本。
+ 
![添加新脚本](/images/油猴脚本编写教程-SegmentFault思否/3094194886-b721a19ba853294b_fix732.png)

然后就会打开如图所示的编辑器窗口，我们就可以在其中编辑自己的脚本文件了。如果你喜欢的话，还可以将脚本内容复制到合适的编辑器中编辑，完成之后再复制回来。

![编辑脚本文件](/images/油猴脚本编写教程-SegmentFault思否/4224078242-f7656f879d3066fa_fix732.png)

如果你点击开发者菜单的话，可以选择ES6模板，然后就可以在脚本中使用新版JavaScript的特性了，它会有Babel转译回ES5。不过这个模板貌似有点问题，用了它就没办法使用代码纠错功能了。所以这里我还是选择了默认的ES5模板。

### 脚本编写方法

#### 功能注释

首先来看看脚本的内容，上面是一大排注释，这些注释可以非常有用的，它表明了脚本的各个属性。下面来简单介绍一下。

|属性名|作用|
| --- |--- |
 | name | 油猴脚本的名字|
 | namespace | 命名空间，类似于Java的包名，用来区分相同名称的脚本，一般写成作者名字或者网址就可以了 |  |
 | version | 脚本版本，油猴脚本的更新会读取这个版本号|
 | description | 描述，用来告诉用户这个脚本是干什么用的|
 | author | 作者名字|
 | match | 只有匹配的网址才会执行对应的脚本，例如`*`、`http://*`、`http://www.baidu.com/*`等，参见[谷歌开发者文档](https://developer.chrome.com/docs/extensions/mv3/match_patterns/)
|
 | grant | 指定脚本运行所需权限，如果脚本拥有相应的权限，就可以调用油猴扩展提供的API与浏览器进行交互。如果设置为`none`的话，则不使用沙箱环境，脚本会直接运行在网页的环境中，这时候无法使用大部分油猴扩展的API。如果不指定的话，油猴会默认添加几个最常用的API|
 | require | 如果脚本依赖其他js库的话，可以使用require指令，在运行脚本之前先加载其他库，常见用法是加载jquery|
 | connect | 当用户使用[GM_xmlhttpRequest](https://www.tampermonkey.net/documentation.php?version=4.9&ext=dhdg&show=dhdg#GM_xmlhttpRequest)请求远程数据的时候，需要使用connect指定允许访问的域名，支持域名、子域名、IP地址以及`*`通配符|
 | updateURL | 脚本更新网址，当油猴扩展检查更新的时候，会尝试从这个网址下载脚本，然后比对版本号确认是否更新|

#### 脚本权限

下面简单介绍一下grant指令那里可以填写的一些权限，详情请查看[油猴脚本文档](https://www.tampermonkey.net/documentation.php)。这里就简单介绍几个常用的，可以调用的函数全部以GM_作为开头。

|权限名|功能|
| --- |--- |
| unsafeWindow | 允许脚本可以完整访问原始页面，包括原始页面的脚本和变量。|
| GM_getValue(name,defaultValue) | 从油猴扩展的存储中访问数据。可以设置默认值，在没成功获取到数据的时候当做初始值。如果保存的是日期等类型的话，取出来的数据会变成文本，需要自己转换一下。|
| GM_setValue(name,value) | 将数据保存到存储中|
| GM_xmlhttpRequest(details) | 异步访问网页数据的API，这个方法比较复杂，有大量参数和回调，详情请参考官方文档。|
| GM_setClipboard(data, info) | 将数据复制到剪贴板中，第一个参数是要复制的数据，第二个参数是MIME类型，用于指定复制的数据类型。|
| GM_log(message) | 将日志打印到控制台中，可以使用F12开发者工具查看。|
| GM_addStyle(css) | 像网页中添加自己的样式表。|
| GM_notification(details, ondone), GM_notification(text, title, image, onclick) | 设置网页通知，请参考文档获取用法。|
| GM_openInTab(url, loadInBackground) | 在浏览器中打开网页，可以设置是否在后台打开等几个选项|

还有一些API没有介绍，请大家直接查看官方文档吧。

### 编写脚本

编写脚本就很简单了，编写到`// Your code here ..`那里即可。可以编写函数，然后在最后调用这几个函数，这样的模块化编写方法写出来的脚本比较容易维护。

#### 等vagrant更新时候提醒我的脚本

前段时间了解了vagrant这个东西，感觉很有意思，准备研究一下，但是照着官网教程运行的时候，第一步就发生了错误。我上网一搜，原来我更新的virtualbox比较新，vagrant恰好不支持。但是如今几个月过去了，vagrant还是没有更新，所以我要写一个脚本，等到vagrant更新的时候，给我网页上弹出一个对话框。

首先访问[vagrant官网](https://www.vagrantup.com/)，然后就可以看到中间下载按钮上大大的版本号2.2.6了。因为版本肯定是不会倒退的，所以只要判断一下版本号不是2.2.6，就可以弹出提示了。通过F12开发者工具可以看到，这三个按钮其实都是链接，只不过显示成了按钮的样子，而且他们恰好都位于`header`标签之中。如果如果可以的话，直接用选择器就可以非常轻松的获取到版本号。

![vagrant官网](/images/油猴脚本编写教程-SegmentFault思否/3073214562-a785d8e2c4712cef_fix732.png)

为了能在更新的时候及时获取到提示，我需要脚本在所有网站上生效，来检测版本。但是这样做会导致另外一个问题，那就是每次打开一个网页都会运行一次检查vagrant的脚本，而这是完全不必要的。所以需要一个额外的判断，这就需要利用油猴提供的API来保存当前日期，只有每天第一次的时候才会执行检查代码。本来我想的很复杂，需要一个日期变量，然后还要额外一个变量保存是否是今天第一次更新。后来我发现我想的太多了，做法其实很简单。每天先获取一次日期，然后和事先保存的日期比较，如果不一样的话才执行脚本，并将日期设置为今天的日期；如果日期一样的话无事发生。

最后一个问题就是如何来判断版本号，有两种方法：第一种就是上面提到的，直接解析HTML代码并找到版本号；第二种是更直接的办法， 因为vagrant也是Github上开源的项目，所以可以直接调用Github的API来获取最新发布的版本号。可惜的是，第二种办法我试了一下居然不成功，不知为何，没办法获取到发布信息，但是换成其他项目就可以。所以最后没办法只好采用第一种办法。有兴趣的同学可以自己试一下第二种方法。

好了，所有相关的坑我都已经解释完毕了，相信大家应该很容易就可以看懂下面的代码，我就不介绍了。虽然看着简单，但是我其实还是踩了不少的坑，就这点代码花了我好几天的时间。而且确实这个代码写的也并不是很好，因为ajax取回来的代码是完整一个html页面，貌似用原版DOM API没办法解析，最后只好用jQuery的`parseHTML`方法解析的。而且我还因为原生方法和jQuery之间的方法名搞混了，浪费了很多时间。

```js
// ==UserScript==
// @name         remind_me_vagrant_update
// @namespace    https://github.com/techstay/myscripts
// @version      0.1
// @description  remind me if vagrant support virtualbox
// @author       techstay
// @match        *
// @require      https://cdn.staticfile.org/jquery/3.4.1/jquery.min.js
// @connect vagrantup.com
// @grant GM_setValue
// @grant GM_getValue
// @grant GM_setClipboard
// @grant GM_log
// @grant GM_xmlhttpRequest
// @grant unsafeWindow
// @grant window.close
// @grant window.focus
// ==/UserScript==

(function () {
    'use strict';

    const CHECKED_DATE = 'checkedDate';

    function checkDateEquals(a, b) {
        return a.getFullYear() === b.getFullYear() &&
            a.getMonth() === b.getMonth() &&
            a.getDate() === b.getDate();
    }

    function checkVagrantVersion() {
        GM_setValue(CHECKED_DATE, new Date());
        GM_xmlhttpRequest({
            "method": "GET",
            "url": "https://www.vagrantup.com/",
            "headers": {
                "user-agent": 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/79.0.3945.130 Safari/537.36'
            },
            "onload": function (result) {
                var list = jQuery.parseHTML(result.response);
                jQuery.each(list, function (i, el) {
                    if (el.nodeName == 'HEADER') {
                        var header = jQuery.parseXML(el.innerHTML);
                        var version = header.getElementsByTagName('a')[1].textContent.replace('Download ', '');
                        if (version != '2.2.6') {
                            alert('Vagrant update!');
                        }
                        return false;
                    }
                });
            }
        });
    }

    var today = new Date();
    var lastCheckedDay = new Date(GM_getValue(CHECKED_DATE, new Date('2006-1-1')));
    if (!checkDateEquals(lastCheckedDay, today)) {
        checkVagrantVersion();
    }

})();
```

#### 调试脚本

编写脚本很难一次成功，大部分时间都花在了调试上面。调试油猴脚本的话有几种调试方法。

第一种方法就是最原始的打印日志，可以利用`console.log`和`GM_log`来将关键信息打印出来，上面的脚本就是我靠打印日志一点点发现各种参数错误的。说实话这种办法有点笨。

第二种就是利用浏览器的调试功能，在脚本需要调试的地方插入`debugger;`语句，然后在打开F12开发者工具的情况下刷新页面，就会发现网页已经暂停在相应位置上。这样就可以利用F12开发者工具进行单步调试、监视变量等操作了。

![调试脚本](/images/油猴脚本编写教程-SegmentFault思否/2630308594-5ea670883f9dcd05_fix732.png)

#### 将文章同步复制到Csdn和思否编辑器的脚本

我的文章一般都是简书首发，然后复制粘贴到Csdn中，但是后来我发现每次手动操作太蠢了，为什么不用脚本来自动化呢？所以我又写了个脚本帮忙完成自动化工作。本来以为这个脚本应该比较简单，不过还是踩了很多坑才凑合把功能写出来。

首先是数据的保存，利用油猴提供的`GM_setValue`倒是可以很简单的将文章标题和内容保存起来。不过问题来了，如何在不同页面之间共享呢？有几种方案：第一种最简单粗暴，直接复制两份，对应页面首先判断是否存在数据，存在的话才执行复制操作，然后清空数据。这种方案最简单，而且如果自己直接新建文章的话也不会出问题。第二种就是数据只保存一份，通过几个变量来确定什么时候复制完成，清空数据，但是这样比较复杂，要理清逻辑顺序很麻烦。所以最后我就采用了第一种办法。

然后又遇到一个问题，那就是如果编辑器自带了保存和恢复功能，很可能会把我复制过去的文章给覆盖了，所以需要等页面加载完之后，延迟一段时间才进行复制操作。然后我又谷歌了一番，差不多解决了这个问题。

然后遇到了一个非常棘手的问题，就是SF的编辑器设计比较复杂，没办法通过直接填充`value`或者`text`属性的方式来写入文章，我想了很久也没有想出来怎么解决。没办法只好改用剪贴板的方式来糊弄了，也就是将文章内容复制到剪贴板里头，然后手动粘贴到编辑器中。

最后一个问题就是简书上这个复制按钮应该如何实现，其实简书编辑器的工具栏倒是空了一些部分，我本来想把按钮直接加到那个上面。但是我发现貌似一旦添加东西，那个工具栏会自动重载取消更改，所以水平所限没做到，只好利用jQueryUI加了一个很丑的浮动按钮，而且因为拖动的时候会触发单击，没办法把按钮改成了双击触发。

最后的脚本就是下面这样的。相比第一个脚本多了几个打开新页面、删除变量、访问剪贴板的API。

```js
// ==UserScript==
// @name         copy_jianshu_to_csdn_and_segmentfault
// @namespace    https://github.com/techstay/myscripts
// @version      0.1
// @description  将简书文章复制到csdn和思否编辑器中
// @author       techstay
// @match        https://editor.csdn.net/md/
// @match https://segmentfault.com/write
// @match https://www.jianshu.com/writer*
// @require      https://cdn.staticfile.org/jquery/3.4.1/jquery.min.js
// @require      https://cdn.bootcss.com/jqueryui/1.12.1/jquery-ui.min.js
// @grant GM_setValue
// @grant GM_getValue
// @grant GM_deleteValue
// @grant unsafeWindow
// @grant GM_setClipboard
// @grant window.close
// @grant window.focus
// @grant GM_openInTab
// ==/UserScript==
(function () {
    'use strict';

    const SF_URL = 'https://segmentfault.com/write'
    const CSDN_URL = 'https://editor.csdn.net/md/'

    const SF_TITLE = 'sf_title'
    const SF_CONTENT = 'sf_content'
    const CSDN_TITLE = 'csdn_title'
    const CSDN_CONTENT = 'csdn_content'

    function saveArticle() {
        GM_setValue(CSDN_TITLE, $('._24i7u').val())
        GM_setValue(CSDN_CONTENT, $('#arthur-editor').val())
        GM_setValue(SF_TITLE, $('._24i7u').val())
        GM_setValue(SF_CONTENT, $('#arthur-editor').val())
    }

    function copyToCsdn() {
        var title = GM_getValue(CSDN_TITLE, '')
        var content = GM_getValue(CSDN_CONTENT, '')
        if (title != '' && content != '') {
            $('.article-bar__title').delay(2000).queue(function () {
                $('.article-bar__title').val(title)
                $('.editor__inner').text(content)
                GM_deleteValue(CSDN_TITLE)
                GM_deleteValue(CSDN_CONTENT)
                $(this).dequeue()
            })
        }
    }

    function copyToSegmentFault() {
        $(document).ready(function () {
            var title = GM_getValue(SF_TITLE, '')
            var content = GM_getValue(SF_CONTENT, '')
            if (title != '' && content != '') {
                $('#title').delay(2000).queue(function () {
                    $('#title').val(title)
                    GM_setClipboard(content, 'text')
                    GM_deleteValue(SF_TITLE)
                    GM_deleteValue(SF_CONTENT)
                    $(this).dequeue()
                })

            }
        })

    }

    function addCopyButton() {
        $('body').append('<div id="copyToCS">双击复制到CSDN和思否</div>')
        $('#copyToCS').css('width', '200px')
        $('#copyToCS').css('position', 'absolute')
        $('#copyToCS').css('top', '70px')
        $('#copyToCS').css('left', '350px')
        $('#copyToCS').css('background-color', '#28a745')
        $('#copyToCS').css('color', 'white')
        $('#copyToCS').css('font-size', 'large')
        $('#copyToCS').css('z-index', 100)
        $('#copyToCS').css('border-radius', '25px')
        $('#copyToCS').css('text-align', 'center')
        $('#copyToCS').dblclick(function () {
            saveArticle()
            GM_openInTab(SF_URL, true)
            GM_openInTab(CSDN_URL, true)
        })
        $('#copyToCS').draggable()
    }

    $(document).ready(function () {
        if (window.location.href.startsWith('https://www.jianshu.com')) {
            addCopyButton()
        } else if (window.location.href.startsWith(SF_URL)) {
            copyToSegmentFault()
        } else if (window.location.href.startsWith(CSDN_URL)) {
            copyToCsdn()
        }
    })
})()
```

### 其他注意事项

#### 脚本编写流程

踩了几天坑，最后总结一下编写油猴脚本的一点步骤。首先要思考脚本的实现方式，需要用到什么API和权限，然后填写好脚本的注释信息。

然后将功能封装成函数的形式，最后在脚本末尾调用实现的函数。写的差不多的时候复制到浏览器中尝试运行。

遇到困难的时候，可能需要直接在F12开发者工具里进行调试。有些网页不用jQuery，为了方便，我们需要自己将jQuery导入到页面中，可以将下面的代码复制到浏览器控制台中。

```js
var jq = document.createElement('script');
jq.src = "https://cdn.staticfile.org/jquery/3.4.1/jquery.min.js";
document.getElementsByTagName('head')[0].appendChild(jq);
```

#### 发布脚本

##### 更新URL

脚本做完了，自然是要共享出来让大家一起使用的。当然既然要发布，自然要支持更新方便日后维护。方法也很简单，直接在上面的注释部分添加`updateURL`即可，然后设置脚本访问地址。例如我要将脚本发布到Github上，就添加下面的注释。

```js
// @updateURL https://raw.githubusercontent.com/techstay/myscripts/master/tampermonkey/remind_me_vagrant_update.js
```

##### 上传脚本

油猴脚本支持好几个网站，其中目前最主流的是[GreasyFork](https://greasyfork.org/zh-CN)，登录这个网站注册一个账号，然后进入用户页面选择提交脚本，然后填写脚本代码和各项信息。

![提交脚本](/images/油猴脚本编写教程-SegmentFault思否/3346250129-51af0aada1db5227_fix732.png)

这样脚本就提交上去了，其他人也可以搜索到并安装脚本了！

![提交脚本完成](/images/油猴脚本编写教程-SegmentFault思否/2044765339-3bf40f378ab7c24b_fix732.png)


### 参考文章
[油猴脚本编写教程-SegmentFault思否](https://segmentfault.com/a/1190000021654926)