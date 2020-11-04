# PM2 Node 进程管理工具

pm2 是一个进程管理工具，可以用它来管理你的 node 进程，并查看 node 进程的状态，当然也支持性能监控，进程守护，负载均衡等功能。

## 一、安装

pm2 需要全局安装，使用以下命令：

```bash
npm install -g pm2
```

## 二、基本命令

pm2 工具安装完成后，可直接在项目目录思想执行以下命令启动进程管理：

| **功能**                         | **命令**                                |
| -------------------------------- | --------------------------------------- |
| 启动进程/应用                    | pm2  start bin/www 或 pm2 start  app.js |
| 启动并重命名进程/应用            | pm2 start  app.js --name wb123          |
| 监听进程/应用目录变化，自动重启  | watch pm2  start bin/www --watch        |
| 结束进程/应用                    | pm2 stop  www                           |
| 结束所有进程/应用                | pm2 stop  all                           |
| 删除进程/应用                    | pm2 delete  www                         |
| 删除所有进程/应用                | pm2 delete  all                         |
| 列出所有进程/应用                | pm2 list                                |
| 查看某个进程/应用具体情况        | pm2  describe www                       |
| 查看进程/应用的资源消耗情况      | pm2 monit                               |
| 查看pm2的日志                    | pm2 logs                                |
| 若要查看某个进程/应用的日志,使用 | pm2 logs  www                           |
| 重新启动进程/应用                | pm2  restart www                        |
| 重新启动所有进程/应用            | pm2  restart all                        |