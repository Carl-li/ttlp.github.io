



华为云鲲鹏服务器配置如下：

### 背景

```
Huawei Kunpeng 920 2.6GHz
4vCPUs | 8GB
openEuler 20.03 64bit with ARM
```

连接机器后，先查看系统相关信息，注意这里是 `aarch64` 的，后续软件包也需要是 `aarch64` 版本的。

```
# 查看系统内核信息
[root@ecs-kunpeng-0005 ~]# uname -a
Linux ecs-kunpeng-0005 4.19.90-2003.4.0.0036.oe1.aarch64 #1 SMP Mon Mar 23 19:06:43 UTC 2020 aarch64 aarch64 aarch64 GNU/Linux
# 查看系统版本信息
[root@ecs-kunpeng-0005 ~]# cat /etc/os-release
NAME="openEuler"
VERSION="20.03 (LTS)"
ID="openEuler"
VERSION_ID="20.03"
PRETTY_NAME="openEuler 20.03 (LTS)"
ANSI_COLOR="0;31"
```

### 安装Redis

- 下载安装
  - 下载：`https://redis.io/download`
  - 上传：`redis-6.2.3.tar.gz`到服务器

```
# 解压
[root@ecs-kunpeng-0006 local]# tar -xvf redis-6.2.3.tar.gz
[root@ecs-kunpeng-0006 local]# mv redis-6.2.3 redis
[root@ecs-kunpeng-0006 local]# cd redis
# 编译
[root@ecs-kunpeng-0006 redis]# make
# 安装
[root@ecs-kunpeng-0006 redis]# make PREFIX=/usr/local/redis install
# 指定配置文件自动，这里配置了端口号为6380
[root@ecs-kunpeng-0006 redis]# bin/redis-server ./redis.conf
612260:C 17 May 2021 14:36:19.264 # oO0OoO0OoO0Oo Redis is starting oO0OoO0OoO0Oo
612260:C 17 May 2021 14:36:19.264 # Redis version=6.2.3, bits=64, commit=00000000, modified=0, pid=612260, just started
612260:C 17 May 2021 14:36:19.264 # Configuration loaded
612260:M 17 May 2021 14:36:19.264 * monotonic clock: POSIX clock_gettime
                _._                                                  
           _.-``__ ''-._                                             
      _.-``    `.  `_.  ''-._           Redis 6.2.3 (00000000/0) 64 bit
  .-`` .-```.  ```\/    _.,_ ''-._                                  
 (    '      ,       .-`  | `,    )     Running in standalone mode
 |`-._`-...-` __...-.``-._|'` _.-'|     Port: 6380
 |    `-._   `._    /     _.-'    |     PID: 612260
  `-._    `-._  `-./  _.-'    _.-'                                   
 |`-._`-._    `-.__.-'    _.-'_.-'|                                  
 |    `-._`-._        _.-'_.-'    |           https://redis.io       
  `-._    `-._`-.__.-'_.-'    _.-'                                   
 |`-._`-._    `-.__.-'    _.-'_.-'|                                  
 |    `-._`-._        _.-'_.-'    |                                  
  `-._    `-._`-.__.-'_.-'    _.-'                                   
      `-._    `-.__.-'    _.-'                                       
          `-._        _.-'                                           
              `-.__.-'                                               
612260:M 17 May 2021 14:36:19.265 # Server initialized
612260:M 17 May 2021 14:36:19.265 # WARNING Your kernel has a bug that could lead to data corruption during background save. Please upgrade to the latest stable kernel.
612260:M 17 May 2021 14:36:19.265 # Redis will now exit to prevent data corruption. Note that it is possible to suppress this warning by setting the following config: ignore-warnings ARM64-COW-BUG
```

上面 `Redis Server` 启动后又停止了，并且报了一个警告： `Your kernel has a bug that could lead to data corruption during background save. Please upgrade to the latest stable kernel.` 并且，给了解决的建议，即在 `redis.conf` 中取消这最后一条注释： `ignore-warnings ARM64-COW-BUG` 。

```
# 编辑vi redis.conf ，取消最后一行相关注释
[root@ecs-kunpeng-0006 redis]# vi redis.conf 
ignore-warnings ARM64-COW-BUG
# 尝试启动
[root@ecs-kunpeng-0006 redis]# bin/redis-server ./redis.conf
615117:C 17 May 2021 14:36:47.889 # oO0OoO0OoO0Oo Redis is starting oO0OoO0OoO0Oo
615117:C 17 May 2021 14:36:47.889 # Redis version=6.2.3, bits=64, commit=00000000, modified=0, pid=615117, just started
615117:C 17 May 2021 14:36:47.889 # Configuration loaded
615117:M 17 May 2021 14:36:47.890 * monotonic clock: POSIX clock_gettime
                _._                                                  
           _.-``__ ''-._                                             
      _.-``    `.  `_.  ''-._           Redis 6.2.3 (00000000/0) 64 bit
  .-`` .-```.  ```\/    _.,_ ''-._                                  
 (    '      ,       .-`  | `,    )     Running in standalone mode
 |`-._`-...-` __...-.``-._|'` _.-'|     Port: 6380
 |    `-._   `._    /     _.-'    |     PID: 615117
  `-._    `-._  `-./  _.-'    _.-'                                   
 |`-._`-._    `-.__.-'    _.-'_.-'|                                  
 |    `-._`-._        _.-'_.-'    |           https://redis.io       
  `-._    `-._`-.__.-'_.-'    _.-'                                   
 |`-._`-._    `-.__.-'    _.-'_.-'|                                  
 |    `-._`-._        _.-'_.-'    |                                  
  `-._    `-._`-.__.-'_.-'    _.-'                                   
      `-._    `-.__.-'    _.-'                                       
          `-._        _.-'                                           
              `-.__.-'                                               
615117:M 17 May 2021 14:36:47.890 # Server initialized
615117:M 17 May 2021 14:36:47.891 # WARNING Your kernel has a bug that could lead to data corruption during background save. Please upgrade to the latest stable kernel.
615117:M 17 May 2021 14:36:47.891 * Ready to accept connections
```
