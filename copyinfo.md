关于blog复制过来的一些信息：

copy from：https://github.com/Huxpro/huxpro.github.io



**主页面各模块：**

HOME：./index.html

ABOUT：./about.html   具体内容在./_includes/about/en.md(英文)|zh.md(中文)

PORTFOLIO：./portfolio/index.html

TAGS：./tags.html

404.html：提示：访问错误

offline.html:提示：阅读过的页面可以在离线时访问哦 ;)



**其他文件含义：**

./_config.yml:jekyll配置，博客名称，简介	，社交软件名称，	友情链接等等。

./_includes:about具体详情、底部footer

./_layouts:博客具体展示页面的模板，default、keynote、page、post。

./_posts:具体的每一篇博客.markdown文件，yyyy-mm-dd-title格式。

./less:???

./package.json: some about blog and devDependencies and scripts.

./pwa:    manifest.json: blog name 、icons and color

./feed.xml:



**改造成自己的信息需要修改的文件：**

1、./_config.yml：改写成关于自己的信息。disqus讨论的账号名称。 Google Analytics。

2、./index.html:HOME页面标语（简介）改成自己的。

3、./_includes/footer.html:首页底部的作者和GitHub项目链接修改。

4、./about.html:简介修改。

5、./_includes/en.md | zh.md:自我介绍修改。

6、./package.json:修改name，GitHub name。

7、./pwa/：blog name and icons。



**其他：**

1、Progressive Web Apps 是 Google 提出的用前沿的 Web 技术为网页提供 App 般使用体验的一系列方案。更多访问：https://zhuanlan.zhihu.com/p/25459319



