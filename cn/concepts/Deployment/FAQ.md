# 常见问题（FAQ）


##### 我可以使用环境变数吗？

你可以在 Sails 使用环境变数设置 `port` 和 `environment`。
`NODE_ENV=production sails lift`
`PORT=443 sails lift`

##### 在哪边放置我的生产环境资料库凭证（credentials）或其它设置？

对于其它部署／特定机器的设置，也就是任何形式的凭证，你应该使用 `config/local.js`。
它默认包含在 `.gitignore` 文档，这样你就不会无意中提交凭证到程序码储存库。

**config/local.js**
```javascript
// Local configuration
// 
// Included in the .gitignore by default,
// this is where you include configuration overrides for your local system
// or for a production deployment.
//
// For example, to use port 80 on the local machine, override the `port` config
module.exports = {
    port: 80,
    environment: 'production',
    adapters: {
        mysql: {
            user: 'root',
            password: '12345'
        }
    }
}
```

##### 如何让应用程序运作在服务器上？
你的 Node.js 实例已正常运作吗？在第一次的时候，当你有一个 IP 位址，便可以 ssh 连线到它，执行 `sudo npm install -g forever` 来安装 Sails 和 forever。

然后，`git clone` 你的工程（或 `scp` 到服务器，如果它不在 git 储存库中）到服务器并 `cd` 进入，接著 `forever start app.js`。


### 效能基准

Sails 的效能可与你所期望的标准 Node.js/Express 应用程序相比。换句话说，就是「快」！我们在 Sails 和 Waterline 做了一些优化，但本质上，我们的重点是不要把已经非常快的东西搞糟了。最重要的，我们要感谢 @ry、@visionmedia、@isaacs、#v8、@joyent 和在 Node.js 核心团队的其他成员。

+ http://serdardogruyol.com/?p=111


<docmeta name="uniqueID" value="FAQ475097">
<docmeta name="displayName" value="FAQ">

