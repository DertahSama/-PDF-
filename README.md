# Download_SS_PDF
这是一个从超星图书馆（http://www.sslibrary.com ）下载PDF并且自动添加目录的python脚本。鉴于大概没有外国友人用，所以no English。

本脚本由本刚刚学会python的菜鸟一边google一边编写出来，当然不能突破超星图书馆的版权限制，原理只是网页爬虫，只能省去阁下按几百次右键保存图片的时间。

所以请使用者自重，若他人将该项目用于非法用途，本人概不负责。

## 更新历史
- 2022年4月21日 19:30:35 ver1.1 加入了不良页面的诊断&重下载功能
- 2022年4月22日 16:10:35 ver1.2 加入了压缩为纯黑白PDF的功能，可大大减小体积
- 2022年4月22日 18:30:23 ver1.3 优化了下不良页面的诊断&重下载功能，但实在下不了我也没办法了
- 2022年4月24日 15:28:26 ver1.4 优化了一点细节
- 2022年4月28日 21:07:23 ver1.5 调整架构，提高运行效率
- 2022年4月29日 18:35:56 ver1.6 进一步提高纯黑白压缩效率，现在体积大概可以÷15
- 2022年5月31日 20:45:54 ver1.7 输出的pdf页面宽度统一为18 cm，整齐点。另外为exe用户提供了更改参数的功能。
- 2022年10月06日 17:31:46 ver1.8 新增了选页下载的功能；给requests加了个伪装头，但好像超星的反爬机制升级了，成功率并不高
- 2024年11月23日 21:37:36 ver1.9 转眼都两年没有更新了！(￣ε(#￣)这次是发现反爬机制再度升级，request不管用了，于是换用了httpx来做，另外pypdf大升级把语法搞得面目全非，让我几乎把pdf相关的部分重写了一遍……总之现在虽然颤颤巍巍但还能下载得了的样子，大家使用愉快~~

## 功能
1. 完整下载封面、版权页、前言页、目录页等，合成为完整的书籍PDF；
2. 与官方pdz下载同等的最高画质（zoom=3）；
3. 顺带下载了目录，并妥当地嵌入到了PDF书签中；
![snipaste_20220420_212157](https://user-images.githubusercontent.com/74524914/164239989-9b3190d7-0233-45c5-9287-38d1c6be6b0f.jpg)
4. （ver1.2 new!）可以将下载的PDF压缩为纯黑白，可在保持清晰度情况下可大大减小体积，ver1.6进一步提高了压缩的效率。\
![snipaste_20220429_185940](https://user-images.githubusercontent.com/74524914/165932245-2b523ffb-6c5f-4287-a8c9-64739661bf99.jpg)



## 环境与用法
**新推出了点开即用的exe文件** ，降低使用门槛。exe点开之后可能会卡几秒，别急。报「no publisher」是因为我没给微软交认bao证hu费，别慌。

（原来python打包exe这么简单，我以为会很复杂。但是代价就是打出来的包十分不精练……）

>环境为python 3.x，需要的模块如下：
>```python
>import requests,time,os,shutil,img2pdf,sys,re,numpy,cv2,glob
>from PyPDF2 import PdfFileReader,PdfFileWriter
>from PIL import Image
>from io import BytesIO
>```
>
>如果阁下是完全不会python的新人，要使用，只需下载一个Visual Studio Code，安装python扩展，然后打开python所在的目录（大概在\Program Files (x86)\Microsoft Visual >Studio\Shared\Python39_64\之类的地方），在Script文件夹上按住Shift地右键→在此处打开Powershell窗口，然后运行以下命令：
>```
>pip3 install requests PyPDF2 Pillow img2pdf numpy opencv-python glob
>```
>然后用VS code打开本脚本运行即可。

用法非常简单：只需在超星网页打开一本书，复制阅读界面的网址进命令行，回车，然后等它下载就可以了。
![snipaste_20220420_205402](https://user-images.githubusercontent.com/74524914/164235308-4b62c5e9-475e-4400-b53b-69bb32fad3c6.png)



## 设置
主要能进行清晰度和下载间隔的设置：
1. 清晰度`zoom`：超星的最高分辨率图即为`zoom=3`，但是代价是总是去色的；如果想下载彩色书籍而保留颜色，可更改到`zoom=2`。
2. 下载间隔`interval`:下太快会被ban的！所以默认`interval=1`，即每下一页停1s，因此下载速度略慢。若阁下对自己的ip有信心可以改短一点。

【1.8更新】还可以设置重试下载的次数。

## 关于新式阅读器的注意事项！
本脚本只能处理 img.sslibrary.com 开头的旧式阅读器页面，而近几年新出版的书有些提供了原生电子版pdf，用的是 ssj.sslibrary.com 开头的新式阅读器：由于这种新式阅读器每页不再是图片了，所以 **ssj.sslibrary.com 开头的新式阅读器是本脚本处理不了的** ！

但是就算是这些新书，超星也依旧做了旧式扫描版，阁下可以访问读秀（ https://book.duxiu.com ），这里同时提供了新式阅读器（「书世界」）和旧式阅读器（「汇雅电子书」）的进入链接，**阁下进这个「汇雅电子书」的页面，就是本脚本能处理的页面了** 。
![snipaste_20220529_132917](https://user-images.githubusercontent.com/74524914/170854051-955d4fcb-0d98-447b-9159-c5bcc2c5d65f.jpg)

另一种方法是，在超星图书馆自己的搜索页面里，「PDF阅读」的网址复制出来会是这样的：\
`https://www.sslibrary.com/reader/pdf/pdfreader?ssid=...`

在只要把这里面的两个`pdf`改成`jpath`，就能进入旧式阅读器了：\
`https://www.sslibrary.com/reader/jpath/jpathreader?ssid=...`

页面清晰度嘛自然是比不上原生电子版的……但是……又不是不能用对吧……(；´д｀)ゞ

## Credit
本脚本受到https://github.com/0NG/sslibrary-pdf-downloader 的启发而编写，补完了前辈计划做而没有做完的工作。
