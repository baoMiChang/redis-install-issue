# macOS安装redis
官方网址:[Install Redis on macOS](https://redis.io/docs/getting-started/installation/install-redis-on-mac-os/)
# 问题
在`Starting and stopping Redis using launchd`这步骤时<br/>
`brew services start redis`命令执行后虽然显示成功<br/>
但执行下一步`brew services info redis`却显示没有启动成功<br/>
```
Running: ✘
Loaded: ✔
Schedulable: ✘
```
# 调查
1. 查看brew服务列表`brew services list`
    ```
    Name  Status     User     File
    redis error  256 liuchang ~/Library/LaunchAgents/homebrew.mxcl.redis.plist
    ```
2. 发现一个error，进到文件里去查看
3. 看到有log路径，再去查看log
4. 找到了失败原因`Failed listening on port 6379 (TCP), ab      orting.`,监听端口失败，说明端口在被使用中
# 解决
查看进程`ps -ef | grep -i redis`找到redis服务在使用6379端口
```
  501 35526     1   0  9:10上午 ??         0:07.67 /opt/homebrew/opt/redis/bin/redis-server 127.0.0.1:6379
```
结束进程`kill -9 35526`

重新查看redis服务`brew services info redis`
```
redis (homebrew.mxcl.redis)
Running: ✔
Loaded: ✔
User: baomichang
PID: 38159
```
