+++
thumbnail = ""
tags = ["webhook", "nodejs", "hugo"]
categories = ["DevOps"]
date = "2017-02-06T15:53:03+08:00"
description = "在 github 中配置 webhook，通过 push 事件自动更新 blog"
title = "利用webhook自动更新博客"
+++

这是我开的第N个博客，希望能活得最长久，文章写的最多。

本博客站使用 Hugo 静态网站生成器生成。这样可以避免使用 Wordpress 等博客程序中额外的管理、安全等负担。所有的页面都是已经生成好的 HTML 网页，直接使用 Nginx 做分发即可。

静态博客的更新是件麻烦事儿，如果每次更新都要上传新写的 Markdown 文本到服务器上进行更新，则太耗费时间了。那么，如何利用网络上的资源，直接在线更新静态博客站呢？

这里我采用了在 Github 的 repository 中创建 webhook 的方式来更新。具体步骤如下：

* 在 Github 中创建 repository，用来存放博客文章。
* 在该 repo 中创建 webhook，截获 push 事件。任何的 `git push`以及直接在 Github 网站上做的更新或新增文章都会有一个`POST`请求发送到一个指定的地址。这个地址需要放在自己的服务器上。
* 在自己的服务器上，创建接收该`POST`请求的服务。当收到请求时，执行重新部署任务。在重新部署的任务中，使用`git clone`的方式，即可获得最新的文章。

自此，就可以在 Github 的网站中直接新增文章，而不用登陆服务器了。

<!--more-->

附上简单的接收`POST`请求并重新部署的 Node 程序：

```
const express = require('express')
const app = express()

app.post('/webhook/blog/deploy', function (req, res) {
  const spawn = require('child_process').spawn;
  const redeploy = spawn('/home/user1/webhook/redeploy.sh', []);

  redeploy.stdout.on('data', (data) => {
    console.log(`stdout: ${data}`);
  });

  redeploy.stderr.on('data', (data) => {
    console.log(`stderr: ${data}`);
  });

  redeploy.on('close', (code) => {
    console.log(`child process exited with code ${code}`);
    res.send('OK');
  });
})

app.listen(3000, function () {
  console.log('Webhook handle app listening on port 3000!')
})

```

使用 Express 框架，只接受 webhook 定义的地址。

#### 后续运维

这样配置只有整个博客需要三个程序。

* Hugo 用来生成静态网站，只在部署脚本中调用。
* Nginx 用来分发网站，以及代理转发到 Node 的请求。可以用 Systemd 管理自启动。
* Node 程序。用来处理 webhook 的请求。需要自定义配置加入到 Systemd 的管理中。配置如下：

```
[user1@server1 blog]$ cat /etc/systemd/system/deploy.service
[Service]
ExecStart=/home/user1/.nvm/versions/node/v6.6.0/bin/node /home/user1/webhook/app.js
Restart=always
StandardOutput=syslog
StandardError=syslog
SyslogIdentifier=deploy
User=user1
Group=user1
Environment=NODE_ENV=production

[Install]
WantedBy=multi-user.target

```

然后使用 systemctl enable/start 这个服务即可。
