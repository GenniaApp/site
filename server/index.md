# GenniaServer
这是从 Gennia 中移植出来的一个单独的服务器模块。

它不依赖图形界面，只需要一个命令行，和 Node.js 相关套件即可运行。

```sh
$ git clone git@github.com:GenniaApp/GenniaServer.git
$ cd GenniaServer
$ npm install
$ npm run start
```

服务器会占用您的 `9016` 端口，请确保该端口没有任务占用，并配置相关安全组策略保证该端口对外开放。

或者，您可以自定义 `main.js` 中的 `global.serverConfig` 中的参数:

* `name`: 您的服务器房间名（默认 `Gennia Server`）
* `port`：端口号（默认 `9016`）
