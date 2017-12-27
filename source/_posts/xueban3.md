---
title: 学伴后端开发总结
date: 2017-12-26 06:14:15
tags: [Koa,爬虫,Nginx]
---

学伴是实验室每年都要进行维护优化的一项产品。
从真真姐，冉学长，狐神的三天开发出的1.0版本；
到真真姐，冉学长，小马哥和男神的全新2.0，安卓，IOS平台全覆盖；
再到3.0的技术迁移与升级，学伴也是越做越好，特别是3.0版本，日活和注册用户数都过万，大连理工大学总共也只有三万多在校生，这说明我们的产品在学生群体里是非常受欢迎的。

### 从python到NodeJS
由于历史遗留问题，学伴2.0的后台没有使用JS进行开发，而是选择了python的flask这样一个轻量型的框架，2.0的后台存在着模块与模块之间逻辑划分不明确的缺点，因此我在改变后台编程语言为JS后，特别思考了这个问题，在重组整个架构时，确保了模块之间分工明确，组织合理。

### 技术选择
由于我负责的是学伴3.0的后台开发工作，而前端大部由安卓和IOS客户端来完成，只有一小部分如新闻与授权等用到了web页面，其余都是和数据与网络打交道的后端，因此正好可以用来小试前后端分离。
- 占比很少的前端使用webpack+scss。
- 后端使用Koa2和ES6进行开发，部署时使用babel进行转义。
- 组织架构采用功能分离的方法。
- 采用nginx进行负载均衡，保证服务器的CPU利用率不过低。
- 利用linux系统的expect软件编写脚本实现了全自动化部署。


### 工程架构
````
xueban3/
├── instances/  用于存放全局、单例的文件
├── lib/ 能够复用的文件，不仅限于该项目
├── log 日志目录 (不被包含在git中）
├── models/
│   ├── index.js 入口文件，自动加载其他文件，除 migrate.js
│   │   > 其他文件需要遵从以下规则
│   │
    module.exports = (sequelize, DataType) => { return yourModel;};
│
├── public/ 资源文件， 事先需要的公共资源和用户上传的经过压缩处理后的头像
│   ├── dists/  资源文件
│   ├── uploads/    用户头像等上传目录
│
├── routes/
│   ├── index.js 入口文件，自动加载其他文件
│   │   > 其他文件需要遵从以下规则
│   │
    module.exports = (sequelize, DataType) => { return yourModel;};
│   │
│   ├── restraints.js  访问限制路由
│   ├── upload.js 文件上传路由
│   └── api/ 所有客户端请求相关
│
├── modelAdaptor/   数据接口层，提供接口供路由层调用
├── Spider/   爬虫层
├── Task/      任务层，调用爬虫层，结果供路    由层调用
````

上面最重要的关系就是路由层—任务层—爬虫层的关系，路由层只能调用任务层提供的接口，不能越权调用爬虫层，通过这样的按功能分层，保证了代码耦合度的下降，同时通过数据库接口层，保证了各层特别是路由层的语义话，让代码的维护变得轻松简单。


### 一些收获
- 利用这次工程，锻炼了设置nginx负载均衡的能力，从而保证cpu的利用率不低于50%。
- 使用expect软件编写脚本，实现部署的全自动化，节省了大量时间人力，在后面的工程中都做了自动化部署脚本。
- 同安卓，IOS客户端开发的接口设定，要有错误码等信息，交流时应该注意综合二人的困难选一个折中的方面。
- 即使是JS，单步调试必不可少。

### 项目部分页面截图
这是应用部分页面的截图
<img src="/images/xueban/0.png" alt="登录页面" width="300px" height="533px">
<img src="/images/xueban/5.png" alt="课表页面" width="300px" height="533px">
<img src="/images/xueban/4.png" alt="动态页面" width="300px" height="533px">

<img src="/images/xueban/6.png" alt="发现页面" width="300px" height="533px">
<img src="/images/xueban/1.png" alt="个人页面" width="300px" height="533px">
<img src="/images/xueban/2.png" alt="卧谈会页面" width="300px" height="533px">