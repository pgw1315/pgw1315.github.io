---
title: Python图像处理库PIL中图像格式转换（一）
author: Will Holmes
categories: Python
tags:
  - 图像处理
  - PIL从入门到精通
  - PIL
  - Python
  - 格式转换
  - 模式转换
date: 2021-11-07 04:51:04
---


在数字图像处理中，针对不同的图像格式有其特定的处理算法。所以，在做图像处理之前，我们需要考虑清楚自己要基于哪种格式的图像进行算法设计及其实现。本文基于这个需求，使用python中的图像处理库PIL来实现不同图像格式的转换。

对于彩色图像，不管其图像格式是PNG，还是BMP，或者JPG，在PIL中，使用Image模块的open()函数打开后，返回的图像对象的模式都是“RGB”。而对于灰度图像，不管其图像格式是PNG，还是BMP，或者JPG，打开后，其模式为“L”。

通过之前的博客对Image模块的介绍，对于PNG、BMP和JPG彩色图像格式之间的互相转换都可以通过Image模块的open()和save()函数来完成。具体说就是，在打开这些图像时，PIL会将它们解码为三通道的“RGB”图像。用户可以基于这个“RGB”图像，对其进行处理。处理完毕，使用函数save()，可以将处理结果保存成PNG、BMP和JPG中任何格式。这样也就完成了几种格式之间的转换。同理，其他格式的彩色图像也可以通过这种方式完成转换。当然，对于不同格式的灰度图像，也可通过类似途径完成，只是PIL解码后是模式为“L”的图像。

这里，我想详细介绍一下Image模块的convert()函数，用于不同模式图像之间的转换。

Convert()函数有三种形式的定义，它们定义形式如下：

im.convert(mode) ⇒ image

im.convert(“P”, **options)  ⇒ image

im.convert(mode, matrix)  ⇒ image

使用不同的参数，将当前的图像转换为新的模式，并产生新的图像作为返回值。

通过博客“[Python图像处理库PIL的基本概念介绍](http://blog.csdn.net/icamera0/article/details/50647465)”，我们知道PIL中有九种不同模式。分别为1，L，P，RGB，RGBA，CMYK，YCbCr，I，F。

本文我采用的示例图像是图像处理中经典的lena照片。分辨率为512x512的lena图片如下：

![](https://img-blog.csdn.net/20160310080807668?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQv/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)

一、模式“RGB”转换为其他不同模式

## 模式“1”

模式“1”为二值图像，非黑即白。但是它每个像素用8个bit表示，0表示黑，255表示白。下面我们将lena图像转换为“1”图像。

例子：

```python

    >>>from PIL import Image
    
    >>> lena =Image.open("D:\\Code\\Python\\test\\img\\lena.jpg")
    
    >>> lena.mode
    
    'RGB'
    
    >>> lena.getpixel((0,0))
    
    (197, 111, 78)
    
    >>> lena_1 = lena.convert("1")
    
    >>> lena_1.mode
    
    '1'
    
    >>> lena_1.size
    
    (512, 512)
    
    >>>lena_1.getpixel((0,0))
    
    255
    
    >>> lena_1.getpixel((10,10))
    
    255
    
    >>>lena_1.getpixel((10,120))
    
    0
    
    >>>lena_1.getpixel((130,120))
    
    255

```
图像lena_1的模式为“1”，分辨率为512x512，如下：

![](https://img-blog.csdn.net/20160310080929507?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQv/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)

## 模式“L”

模式“L”为灰色图像，它的每个像素用8个bit表示，0表示黑，255表示白，其他数字表示不同的灰度。在PIL中，从模式“RGB”转换为“L”模式是按照下面的公式转换的：

L = R * 299/1000 + G * 587/1000+ B * 114/1000

下面我们将lena图像转换为“L”图像。

例子：

```python

    >>> from PIL importImage
    
    >>> lena = Image.open("D:\\Code\\Python\\test\\img\\lena.jpg")
    
    >>> lena.mode
    
    'RGB'
    
    >>> lena.getpixel((0,0))
    
    (197, 111, 78)
    
    >>> lena_L =lena.convert("L")
    
    >>> lena_L.mode
    
    'L'
    
    >>> lena_L.size
    
    (512, 512)
    
    >>>lena.getpixel((0,0))
    
    (197, 111, 78)
    
    >>>lena_L.getpixel((0,0))
    
    132

```
对于第一个像素点，原始图像lena为(197, 111, 78)，其转换为灰色值为：

197 *299/1000 + 111 * 587/1000 + 78 * 114/1000 = 132.952，PIL中只取了整数部分，即为132。

转换后的图像lena_L如下：

![](https://img-blog.csdn.net/20160310081028211?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQv/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)

## 模式“P”

模式“P”为8位彩色图像，它的每个像素用8个bit表示，其对应的彩色值是按照调色板查询出来的。

下面我们使用默认的调色板将lena图像转换为“P”图像。

例子：

```python

    >>> from PIL importImage
    
    >>> lena = Image.open("D:\\Code\\Python\\test\\img\\lena.jpg")
    
    >>> lena.mode
    
    'RGB'
    
    >>> lena.getpixel((0,0))
    
    (197, 111, 78)
    
    >>> lena_P =lena.convert("P")
    
    >>> lena_P.mode
    
    'P'
    
    >>>lena_P.getpixel((0,0))
    
    62

```
转换后的图像lena_P如下：

![](https://img-blog.csdn.net/20160310081127075?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQv/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)

## 模式“RGBA”

模式“RGBA”为32位彩色图像，它的每个像素用32个bit表示，其中24bit表示红色、绿色和蓝色三个通道，另外8bit表示alpha通道，即透明通道。

下面我们将模式为“RGB”的lena图像转换为“RGBA”图像。

例子：

```python

    >>> from PIL import Image
    
    >>>lena = Image.open("D:\\Code\\Python\\test\\img\\lena.jpg")
    
    >>>lena.mode
    
    'RGB'
    
    >>>lena.getpixel((0,0))
    
    (197,111, 78)
    
    >>>lena_rgba = lena.convert("RGBA")
    
    >>>lena_rgba.mode
    
    'RGBA'
    
    >>>lena_rgba.getpixel((0,0))
    
    (197,111, 78, 255)
    
    >>>lena_rgba.getpixel((0,1))
    
    (196,110, 77, 255)
    
    >>>lena.getpixel((0,0))
    
    (197,111, 78)
    
    >>>lena.getpixel((0,1))
    
    (196,110, 77)

```
从实例中可以看到，使用当前这个方式将“RGB”图像转为“RGBA”图像时，alpha通道全部设置为255，即完全不透明。

转换后的图像lena_rgba如下：

![](https://img-blog.csdn.net/20160310081249935?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQv/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)

## 模式“CMYK”

模式“CMYK”为32位彩色图像，它的每个像素用32个bit表示。模式“CMYK”就是印刷四分色模式，它是彩色印刷时采用的一种套色模式，利用色料的三原色混色原理，加上黑色油墨，共计四种颜色混合叠加，形成所谓“全彩印刷”。

四种标准颜色是：C：Cyan = 青色，又称为‘天蓝色’或是‘湛蓝’M：Magenta = 品红色，又称为‘洋红色’；Y：Yellow = 黄色；K：Key
Plate(blacK) = 定位套版色（黑色）。

下面我们将模式为“RGB”的lena图像转换为“CMYK”图像。

例子：

```python

    >>>from PIL import Image
    
    >>> lena =Image.open("D:\\Code\\Python\\test\\img\\lena.jpg")
    
    >>> lena_cmyk =lena.convert("CMYK")
    
    >>> lena_cmyk.mode
    
    'CMYK'
    
    >>>lena_cmyk.getpixel((0,0))
    
    (58, 144, 177, 0)
    
    >>> lena_cmyk.getpixel((0,1))
    
    (59, 145, 178, 0)
    
    >>>lena.getpixel((0,0))
    
    (197, 111, 78)
    
    >>>lena.getpixel((0,1))
    
    (196, 110, 77)

```
从实例中可以得知PIL中“RGB”转换为“CMYK”的公式如下：

C = 255 - R  
M = 255 - G  
Y = 255 - B  
K = 0

由于该转换公式比较简单，转换后的图像颜色有些失真。

转换后的图像lena_cmyk如下：

![](https://img-blog.csdn.net/20160310081334545?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQv/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)

## 模式“YCbCr”

模式“YCbCr”为24位彩色图像，它的每个像素用24个bit表示。YCbCr其中Y是指亮度分量，Cb指蓝色色度分量，而Cr指红色色度分量。人的肉眼对视频的Y分量更敏感，因此在通过对色度分量进行子采样来减少色度分量后，肉眼将察觉不到的图像质量的变化。

模式“RGB”转换为“YCbCr”的公式如下：

Y= 0.257*R+0.504*G+0.098*B+16  
Cb = -0.148*R-0.291*G+0.439*B+128  
Cr = 0.439*R-0.368*G-0.071*B+128

下面我们将模式为“RGB”的lena图像转换为“YCbCr”图像。

例子：

```python

    >>>from PIL import Image
    
    >>> lena =Image.open("D:\\Code\\Python\\test\\img\\lena.jpg")
    
    >>> lena_ycbcr =lena.convert("YCbCr")
    
    >>>lena_ycbcr.mode
    
    'YCbCr'
    
    >>>lena_ycbcr.getpixel((0,0))
    
    (132, 97, 173)
    
    >>>lena.getpixel((0,0))
    
    (197, 111, 78)

```
按照公式，Y = 0.257*197+0.564*111+0.098*78+16= 136.877

Cb= -0.148*197-0.291*111+0.439*78+128= 100.785  
Cr = 0.439*197-0.368*111-0.071*78+128 = 168.097

由此可见，PIL中并非按照这个公式进行“RGB”到“YCbCr”的转换。

转换后的图像lena_ycbcr如下：

![](https://img-blog.csdn.net/20160310081415264?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQv/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)

## 模式“I”

模式“I”为32位整型灰色图像，它的每个像素用32个bit表示，0表示黑，255表示白，(0,255)之间的数字表示不同的灰度。在PIL中，从模式“RGB”转换为“I”模式是按照下面的公式转换的：

I = R * 299/1000 + G * 587/1000 + B * 114/1000

下面我们将模式为“RGB”的lena图像转换为“I”图像。

例子：

```python

    >>> from PIL import Image
    
    >>>lena = Image.open("D:\\Code\\Python\\test\\img\\lena.jpg")
    
    >>>lena.getpixel((0,0))
    
    (197,111, 78)
    
    >>>lena.getpixel((0,1))
    
    (196,110, 77)
    
    >>> lena_I =lena.convert("I")
    
    >>> lena_I.mode
    
    'I'
    
    >>>lena_I.getpixel((0,0))
    
    132
    
    >>>lena_I.getpixel((0,1))
    
    131
    
    >>> lena_L =lena.convert("L")
    
    >>>lena_L.getpixel((0,0))
    
    132
    
    >>>lena_L.getpixel((0,1))
    
    131

```
从实验的结果看，模式“I”与模式“L”的结果是完全一样，只是模式“L”的像素是8bit，而模式“I”的像素是32bit。

## 模式“F”

模式“F”为32位浮点灰色图像，它的每个像素用32个bit表示，0表示黑，255表示白，(0,255)之间的数字表示不同的灰度。在PIL中，从模式“RGB”转换为“F”模式是按照下面的公式转换的：

F = R * 299/1000+ G * 587/1000 + B * 114/1000

下面我们将模式为“RGB”的lena图像转换为“F”图像。

例子：

```python

    >>>from PIL import Image
    
    >>> lena =Image.open("D:\\Code\\Python\\test\\img\\lena.jpg")
    
    >>>lena.getpixel((0,0))
    
    (197, 111, 78)
    
    >>>lena.getpixel((0,1))
    
    (196, 110, 77)
    
    >>> lena_F =lena.convert("F")
    
    >>> lena_F.mode
    
    'F'
    
    >>>lena_F.getpixel((0,0))
    
    132.95199584960938
    
    >>>lena_F.getpixel((0,1))
    
    131.95199584960938

```
模式“F”与模式“L”的转换公式是一样的，都是RGB转换为灰色值的公式，但模式“F”会保留小数部分，如实验中的数据。



