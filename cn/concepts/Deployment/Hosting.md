# 托管（Hosting）

以下是不完整的 Sails.js 托管服务供应商清单。

##### 部署到 Modulus？

+ http://blog.modulus.io/sails-js

##### 部署到 NodeJitsu？
要部署到 NodeJitsu，你需要稍微修改设置文档：
在应用程序文件夹开启 `config/local.js`。你需要加入以下几行到此设置文档。

```
// Port this Sails application will live on
port: 80,
host: 'subdomain.jit.su',
```

`host:` 不是默认建立的属性。你需要加入这个属性。当执行 `jitsu deploy` 时，Nodejitsu 会询问你的 `subdomain`

+ https://blog.nodejitsu.com/keep-a-nodejs-server-up-with-forever/
+ https://github.com/balderdashy/sails/issues/455

##### 部署到 OpenShift？
要部署到 OpenShift，你需要稍微修改设置文档：
在应用程序文件夹开启 `config/local.js`。你需要加入以下几行到此设置文档。

```
port: process.env.OPENSHIFT_NODEJS_PORT,
host: process.env.OPENSHIFT_NODEJS_IP,
```

##### 使用 DigitalOcean？

+ https://www.digitalocean.com/community/articles/how-to-create-an-node-js-app-using-sails-js-on-an-ubuntu-vps
+ https://www.digitalocean.com/community/articles/how-to-use-pm2-to-setup-a-node-js-production-environment-on-an-ubuntu-vps
+ https://www.digitalocean.com/community/articles/how-to-host-multiple-node-js-applications-on-a-single-vps-with-nginx-forever-and-crontab

##### 部署到 Heroku？

+ [SailsCasts：Deploying a Sails App to Heroku](http://irlnathan.github.io/sailscasts/blog/2013/11/05/building-a-sails-application-ep26-deploying-a-sails-app-to-heroku/)
+ https://groups.google.com/forum/#!topic/sailsjs/vgqJFr7maSY
+ https://github.com/chadn/heroku-sails
+ http://dennisrongo.com/deploying-sails-js-to-heroku/#.UxQKPfSwI9w
+ http://stackoverflow.com/a/20184907/486547

##### 部署到 AWS？

+ http://blog.grio.com/2014/01/your-own-mini-heroku-on-aws.html
+ http://serverfault.com/questions/531560/creating-an-sails-js-application-on-aws-ami-instance
+ http://bussing-dharaharsh.blogspot.com/2013/08/creating-sailsjs-application-on-aws-ami.html
+ http://cloud.dzone.com/articles/how-deploy-nodejs-apps-aws-mac

##### 使用 PM2？

+ http://devo.ps/blog/2013/06/26/goodbye-node-forever-hello-pm2.html


##### 部署到 CloudControl？

+ https://www.cloudcontrol.com/dev-center/Guides/NodeJS/Sailsjs



##### 取得专业协助

这些日子以来，拥有一定技术的情况下，部署强大的应用程序变得越来越简单。尽管如此，你不一定有时间来自己处理这些事情。
Sails.js 是由我的公司维护，[Balderdash](http://balderdash.co)，一间在美国德州奥斯丁的 Node.js 顾问公司。如果你的公司需要专业协助，我们很乐意提供帮助。部署不是很困难，而且在大多情况下，它不应该超过几个小时。



<docmeta name="uniqueID" value="Hosting276234">
<docmeta name="displayName" value="Hosting">

