---
title: MacOS提示“打不开xxx,因为Apple无法检查其是否包含恶意软件...”
author: Will Holmes
categories: MacOS
tags:
	-MacOS
date: 2021-11-15 13:16:49
---

>当你的mac提示“打不开xxx,因为Apple无法检查其是否包含恶意软件...”时，一般是由于你下载了非App Store且不受苹果信任的开发者的软件，怎么解决呢？

[![](https://i.loli.net/2020/05/09/dl7ghqj4ranTvwF.png "MacOS提示“打不开xxx,因为Apple无法检查其是否包含恶意软件...”")](https://i.loli.net/2020/05/09/dl7ghqj4ranTvwF.png)

>其实很简单，只需要在设置-安全性与隐私-通用-允许从以下位置下载的应用-勾选“任何来源”，这时候有朋友就会说了，只有前两项怎么办？

[![](https://i.loli.net/2020/05/09/KqWfNwoO7RlDvs9.png "MacOS提示“打不开xxx,因为Apple无法检查其是否包含恶意软件...”")](https://i.loli.net/2020/05/09/KqWfNwoO7RlDvs9.png)

>其实这个选项在最近几个macOS版本中被隐藏了（具体版本未考究），只需要通过命令就可以打开
### 操作步骤：
1、在启动台-其他文件夹-终端，打开
2、输入命令：
```bash
sudo spctl --master-disable #复制粘贴进去
```
执行回车，然后输入电脑密码再回车即可（输密码是看不到的，直接输入就可以了）
[![](https://i.loli.net/2020/05/09/QWvHcPUFT1IizGY.png "MacOS提示“打不开xxx,因为Apple无法检查其是否包含恶意软件...”")](https://i.loli.net/2020/05/09/QWvHcPUFT1IizGY.png)
之后就可以看到“任何来源”选项已经勾选，此时软件已经可以正常打开了。
**PS：关闭显示“任何来源”的命令是**
```bash
sudo spctl --master-enable
```
操作方法与上面同
