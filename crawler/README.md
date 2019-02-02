# 记一件小事

有这样一个网页：https://free0.gyteng.com/ 我想搞一个爬虫，抓取页面的二维码，没有太多实际用途只是一个试验或者说一次学习

查看页面源代码之后发现使用了验证码服务以及ajax动态加载技术：
![image](https://github.com/sstutanotam/blogLike/blob/master/crawler/imgs/js.PNG)

这是一个非常简单的页面如果使用selenium去爬取它的内容感觉杀鸡用牛刀

由于网页禁用了图片验证过程，实际上我们只要通过阅读它的JS代码找出token的生成逻辑就可以了

于是我通过浏览器F12查看了页面的详细细节，然后找到了then函数：
![image](https://github.com/sstutanotam/blogLike/blob/master/crawler/imgs/qr.PNG)

由于then函数是recaptcha_zh_cn.js处理的，点击recaptcha_zh_cn.js之后发现了then函数的两个参数 G 和 h：
![image](https://github.com/sstutanotam/blogLike/blob/master/crawler/imgs/then.PNG)

显然G是一个函数它的参数就是token，我继续寻找，在reload?k=...里面发现了 G 和 h：
![image](https://github.com/sstutanotam/blogLike/blob/master/crawler/imgs/reload.PNG)

看一下 G ：
![image](https://github.com/sstutanotam/blogLike/blob/master/crawler/imgs/G.PNG)

还有 h ：
![image](https://github.com/sstutanotam/blogLike/blob/master/crawler/imgs/h.PNG)

到这一步就发现这个JS脚本的逻辑太复杂分析不出token的生成逻辑

而且进一步思考即使我能通过JS脚本分析出token的生成逻辑也没太大意义。因为网站的JS脚本可以更新，他们甚至可以24小时更新一次

他们每更新一次我们就需要通过人工阅读JS代码的方式重新获取一次token的生成逻辑，这是不划算的

这么一个简单的页面到头来似乎也必须通过selenium去爬，这让我对爬虫技术有了一些更深刻的思考

我已经多次重复，这是一个简单的页面，而且使用了一个简单的技术就使得爬虫的爬取难度大大提高

于是我想未来可能所有的网站都会使用这种技术，也就是说未来所有的爬虫都必须使用selenium加webdriver的方式

过去我们使用代理IP池加cookie池的方式多线程快速爬取一个网站几乎是所向无敌的，单个的IP和cookie可以模拟普通用户慢慢获取数据，许许多多IP和cookie联合起来就会像个爬虫一样快速吞噬所有数据

然而selenium加webdriver的方式对于多线程快速爬取来说简直是灾难，会造成内存和cpu资源的过度浪费

网站要识别人类和机器人大体就两种方式。一是让机器人做一些人类很容易做但机器人很难做的事，比如图片验证码。但这种方式容易造成用户体验下降

二是通过机器人频繁快速访问的特点让机器人每次访问都多耗费一点内存和cpu资源，那么频繁访问必然造成系统资源枯竭

然而爬虫有另一个特点，就是对于同一个页面的相同内容多次爬取是没有意义的。假设未来爬取网页的成本很高，我们可以算一个账：

假如有100个人需要某个页面的同一个内容，如果他们每人搞个爬虫一人爬取一次。假设每次爬取成本0.01元，他们总共的成本就是1元

如果他们让专业的爬虫公司去做，只需要爬取一次然后把获得的数据复制100份，分给他们每人一份，总共的成本只有0.01元

而且专业的爬虫公司可以购买高配置的专业设备、研发效率更高的webdriver和爬虫程序

当然我们也需要看到专业的爬虫公司面临的法律风险也更大

而且动态加载技术对于好多网站也算是技术门槛，尤其是一些比较复杂的网站，要想短时间内全部更新换代还比较难
