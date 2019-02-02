# 记一件小事

有这样一个网页：https://free0.gyteng.com/

查看页面源代码之后发现使用了验证码服务以及ajax动态加载技术

这是一个非常简单的页面如果使用selenium去爬取它的内容感觉杀鸡用牛刀

由于网页禁用了图片验证过程，实际上我们只要通过阅读它的JS代码找出token的生成逻辑就可以了

于是我通过浏览器F12查看了页面的详细细节，然后找到了then函数：
![image](https://github.com/sstutanotam/blogLike/raw/master/crawler/imgs/qr.png)

