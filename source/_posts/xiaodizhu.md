---
title: 小地主果蔬批发网站开发总结
date: 2017-12-26 01:53:45
tags: [Angular,Koa,Webpack]
---
这是大学接触web开发以后做的第一个大型项目。我刚开始是负责前端开发部分，后来因为客户的需求变更，需要将单商户改为多商户，故我对后端代码也进行了大量编写与修改。

### 技术选择
先简单说一下技术选择。
- JS框架选用Angular1.0，项目开始时AngularJs1.0刚出炉，就灰常想尝试一下。
- css框架选用amazeui，这样做出来的网页比较好看。
- 打包工具选用webpack，毕竟是自动化打包发布加热修复，用着就是好。
- 后端使用Koa1.0，使用大明鼎鼎的tj的co用于最基本的编程风格。
- 数据库选用mysql。
- 后端渲染模板使用ect。

### 脚手架
我大致地用webpack和koa搭了一个脚手架，文件结构如下所示：
````
FruitMaker/
├── instances/  用于存放全局、单例的文件
├── lib/ 能够复用的文件，不仅限于该项目
├── log 日志目录 (不被包含在git中）
├── models/
│   ├── index.js 入口文件，自动加载其他文件，除 migrate.js
│   │   > 其他文件需要遵从以下规则
│   │
    module.exports = (sequelize, DataType) => { return yourModel;};

│   └── user.js 微信用户
├── public/ 资源文件， 压缩好的文件都放这里
│   ├── fonts/  字体目录
│   ├── js/
│   └── css/
├── routes/
│   ├── index.js 入口文件，自动加载其他文件
│   │   > 其他文件需要遵从以下规则
│   │
    module.exports = (sequelize, DataType) => { return yourModel;};

│   └── user.js 微信用户
├── scripts/   脚本目录（一些要运行的小脚本）
├── src/   源码目录
│   ├── bower_components/  bower目录
│   ├── template_content/  后台模版源码
│   ├── js/
│   └── css/
├── test/   单元测试目录
├── views/   视图目录
├── helpers/   辅助模块目录
│   ├── auth.js  登录验证相关
│   ├── verifyCode  短信验证码
````

本工程在src内存储前端源码，在routes和models里面存储后端源码，其中models是数据库访问层，routes是路由层。

### 一些教训
- 在项目刚开始的时候，工程复杂度并不是很高，把前端和后端代码放到一个工程里面，影响不大，merge时git的conflicts也比较少。但是随着功能点的增多，项目变得越来越大，经常在排查一个bug时，需要挨个确定错误是出在前端还是出在后端，在查找对应相关文件的过程中，往往需要花大量精力用在前后端文件的判别上，很容易由于分工不明导致效率低下。

- 特别是客户要求由单商户改为多商户，我一个人去更改对应的功能，新代码很容易写，除bug却非常耗时间，往往是看了前端代码再看后端代码，看了后端再看前端，大脑在不同的域内来回切换，精神稍微不集中就忘记了排查到哪儿了，就得推倒重来。

### 一些收获
- 在这个项目里，前端技术我熟悉了AngularJs的开发，Angular1用于增删改查，着实是方便。
- 后端技术学到最多的就是promise了，由于nodejs的单线程异步性质，很多代码就需要利用co和promise进行同步控制，比如原先正常的循环就需要改成递归才能保证执行的结果。
- 架构方面，吸取到的最大的教训就是千万不要把前端代码和后端代码放到一个工程里，一定要做前后端分离，于是就有了用node做中间层的想法。同时也验证了做模块分离的正确性。

### 项目部分页面截图
这是我开发的页面的截图

<img src="/images/xiaodizhu/1.png" alt="首页商店页面" width="300px" height="543px">

![商品类别管理](/images/xiaodizhu/2.png)
![订单管理](/images/xiaodizhu/3.png)
![商品信息添加](/images/xiaodizhu/4.png)
![超级管理员人员管理](/images/xiaodizhu/5.png)
![区域管理](/images/xiaodizhu/6.png)
