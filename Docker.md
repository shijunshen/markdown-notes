# Docker

## 启动配置

```shell
systemctl start docker.service # 开启docker
systemctl enable docker.service # 开机自启动
systemtcl status docker.service # 查看docker的运行状态
```

## 镜像命令

**docker images** 查询当前主机上的镜像

```shell
[root@centos01 ~]# docker images
REPOSITORY    TAG       IMAGE ID       CREATED        SIZE
hello-world   latest    feb5d9fea6a5   9 months ago   13.3kB

#解释
REPOSITORY：镜像的仓库源
TAG：镜像的标签
IMAGE ID：镜像的ID
CREATED：镜像的创建时间
SIZE：镜像的大小

#可选项
[root@centos01 ~]# docker images --help

Usage:  docker images [OPTIONS] [REPOSITORY[:TAG]]

List images

Options:
  -a, --all             Show all images (default hides intermediate images)	#列出所有镜像
      --digests         Show digests
  -f, --filter filter   Filter output based on conditions provided			#过滤
      --format string   Pretty-print images using a Go template
      --no-trunc        Don't truncate output
  -q, --quiet           Only show image IDs									#只显示镜像id

```



**docker search** 搜索镜像

```shell
[root@centos01 ~]# docker search --help

Usage:  docker search [OPTIONS] TERM

Search the Docker Hub for images

Options:
  -f, --filter filter   Filter output based on conditions provided
      --format string   Pretty-print search using a Go template
      --limit int       Max number of search results (default 25)
      --no-trunc        Don't truncate output

#附加条件
docker search mysql --f=STARS=5000	#STARS 5000以上
```



**docker pull ** 下载

```shell
[root@centos01 ~]# docker pull mysql
Using default tag: latest				#不写tag就是latest
latest: Pulling from library/mysql		
e54b73e95ef3: Pull complete				#分层下载，docker images的核心 联合文件系统
bb429e544310: Pull complete
c148b3f9047c: Pull complete
a1dd213a3236: Pull complete
297095d1476d: Pull complete
87f3aa837301: Pull complete
535019436481: Pull complete
23722cff1cc3: Pull complete
eb19883dc4c6: Pull complete
6eaa2c236095: Pull complete
Digest: sha256:444f037733d01fc3dfc691a9ab05e346629e8e4d3a6c75da864f21421fb38ced	#签名
Status: Downloaded newer image for mysql:latest	#真实地址
docker.io/library/mysql:latest
```



**docker rmi** 删除镜像id

```shell
[root@centos01 ~]# docker rmi -f 容器id	#删除指定的容器
[root@centos01 ~]# docker rmi -f 容器id 容器id 容器id	#删除多个容器
[root@centos01 ~]# docker rmi -f $(docker images -aq)	#删除全部容器
```



## 容器命令

说明：有了镜像才能创建容器，linux下载一个centos镜像来测试学习

```shell
docker pull centos
```

新建容器并启动

```shell
docker run [可选参数] image

#参数说明
--name="Name"	#容器名字：tomcat01，tomcat02
-d				#后台方式运行
-it				#使用交互方式运行，进入容器查看内容
-p				#小写，指定容器的端口 -p	8080:8080
	-p ip：主机端口：容器端口
	-p 主机端口：容器端口 （常用）
	-p 容器端口
	容器端口
-P				#随机指定端口

#测试
[root@centos01 ~]# docker run -it centos /bin/bash	#启动并进入容器
[root@7a2bc7289358 /]	#是容器id 不是镜像id

exit 退出命令
docker ps：查看运行的容器
		-a 所有运行的（包括历史运行的）
		-n=？ 显示最近创建的容器，指定数目
		-q	只显示容器的编号
```

退出容器

```shell
exit	#直接容器停止并推出
Ctrl + P + Q #不停止退出
```

删除容器

```shell
docker rm 容器id					#删除指定的容器，不能删除正在运行的容器，如果要强制删除 rm -f #force的意思
docker rm -f ${docker ps -aq}		#删除所有的容器
```

启动停止

```shell
docker start id
docker restart id
docker stop	id
docker kill id		#强制停止
```





## 其他常用命令

后台启动容器

```shell
# 命令 docker run -d 镜像名！
[root@centos01 /]# docker run -d centos

#docker ps 发现centos停止了
#常见的坑：docker 容器使用后台运行，就必须要有一个前台进程，docker发现没有前台的应用，就会自动停止
```

查看日志

```shell
docker logs
```

查看容器中进程信息

```shell
[root@centos01 /]# docker top 56885854f7f1
UID                 PID                 PPID                C                   STIME               TTY                 TIME                CMD
root                12241               12222               0                   14:49               pts/0               00:00:00            /bin/bash
```

查看镜像元数据

```shell
[root@centos01 /]# docker inspect 56885854f7f1
[
    {
        "Id": "56885854f7f1048a2c35f2163a68528d0cb928f4533f0940c5c542fd7f07969d",
        "Created": "2022-07-07T06:49:25.275216791Z",
        "Path": "/bin/bash",
        "Args": [],
        "State": {
            "Status": "running",
            "Running": true,
            "Paused": false,
            "Restarting": false,
            "OOMKilled": false,
            "Dead": false,
            "Pid": 12241,
            "ExitCode": 0,
            "Error": "",
            "StartedAt": "2022-07-07T06:49:25.551475412Z",
            "FinishedAt": "0001-01-01T00:00:00Z"
        },
        "Image": "sha256:5d0da3dc976460b72c77d94c8a1ad043720b0416bfc16c52c45d4847e53fadb6",
        "ResolvConfPath": "/var/lib/docker/containers/56885854f7f1048a2c35f2163a68528d0cb928f4533f0940c5c542fd7f07969d/resolv.conf",
        "HostnamePath": "/var/lib/docker/containers/56885854f7f1048a2c35f2163a68528d0cb928f4533f0940c5c542fd7f07969d/hostname",
        "HostsPath": "/var/lib/docker/containers/56885854f7f1048a2c35f2163a68528d0cb928f4533f0940c5c542fd7f07969d/hosts",
        "LogPath": "/var/lib/docker/containers/56885854f7f1048a2c35f2163a68528d0cb928f4533f0940c5c542fd7f07969d/56885854f7f1048a2c35f2163a68528d0cb928f4533f0940c5c542fd7f07969d-json.log",
        "Name": "/romantic_mccarthy",
        "RestartCount": 0,
        "Driver": "overlay2",
        "Platform": "linux",
        "MountLabel": "",
        "ProcessLabel": "",
        "AppArmorProfile": "",
        "ExecIDs": null,
        "HostConfig": {
            "Binds": null,
            "ContainerIDFile": "",
            "LogConfig": {
                "Type": "json-file",
                "Config": {}
            },
            "NetworkMode": "default",
            "PortBindings": {},
            "RestartPolicy": {
                "Name": "no",
                "MaximumRetryCount": 0
            },
            "AutoRemove": false,
            "VolumeDriver": "",
            "VolumesFrom": null,
            "CapAdd": null,
            "CapDrop": null,
            "CgroupnsMode": "host",
            "Dns": [],
            "DnsOptions": [],
            "DnsSearch": [],
            "ExtraHosts": null,
            "GroupAdd": null,
            "IpcMode": "private",
            "Cgroup": "",
            "Links": null,
            "OomScoreAdj": 0,
            "PidMode": "",
            "Privileged": false,
            "PublishAllPorts": false,
            "ReadonlyRootfs": false,
            "SecurityOpt": null,
            "UTSMode": "",
            "UsernsMode": "",
            "ShmSize": 67108864,
            "Runtime": "runc",
            "ConsoleSize": [
                0,
                0
            ],
            "Isolation": "",
            "CpuShares": 0,
            "Memory": 0,
            "NanoCpus": 0,
            "CgroupParent": "",
            "BlkioWeight": 0,
            "BlkioWeightDevice": [],
            "BlkioDeviceReadBps": null,
            "BlkioDeviceWriteBps": null,
            "BlkioDeviceReadIOps": null,
            "BlkioDeviceWriteIOps": null,
            "CpuPeriod": 0,
            "CpuQuota": 0,
            "CpuRealtimePeriod": 0,
            "CpuRealtimeRuntime": 0,
            "CpusetCpus": "",
            "CpusetMems": "",
            "Devices": [],
            "DeviceCgroupRules": null,
            "DeviceRequests": null,
            "KernelMemory": 0,
            "KernelMemoryTCP": 0,
            "MemoryReservation": 0,
            "MemorySwap": 0,
            "MemorySwappiness": null,
            "OomKillDisable": false,
            "PidsLimit": null,
            "Ulimits": null,
            "CpuCount": 0,
            "CpuPercent": 0,
            "IOMaximumIOps": 0,
            "IOMaximumBandwidth": 0,
            "MaskedPaths": [
                "/proc/asound",
                "/proc/acpi",
                "/proc/kcore",
                "/proc/keys",
                "/proc/latency_stats",
                "/proc/timer_list",
                "/proc/timer_stats",
                "/proc/sched_debug",
                "/proc/scsi",
                "/sys/firmware"
            ],
            "ReadonlyPaths": [
                "/proc/bus",
                "/proc/fs",
                "/proc/irq",
                "/proc/sys",
                "/proc/sysrq-trigger"
            ]
        },
        "GraphDriver": {
            "Data": {
                "LowerDir": "/var/lib/docker/overlay2/f02ae094502aabfde91a64aaf346809de7606b6f8fee456c10a93715c11e7283-init/diff:/var/lib/docker/overlay2/a482e5190fcd6642820eae3f5741b6de81ded64446ef292a2831e128fd46feda/diff",
                "MergedDir": "/var/lib/docker/overlay2/f02ae094502aabfde91a64aaf346809de7606b6f8fee456c10a93715c11e7283/merged",
                "UpperDir": "/var/lib/docker/overlay2/f02ae094502aabfde91a64aaf346809de7606b6f8fee456c10a93715c11e7283/diff",
                "WorkDir": "/var/lib/docker/overlay2/f02ae094502aabfde91a64aaf346809de7606b6f8fee456c10a93715c11e7283/work"
            },
            "Name": "overlay2"
        },
        "Mounts": [],
        "Config": {
            "Hostname": "56885854f7f1",
            "Domainname": "",
            "User": "",
            "AttachStdin": true,
            "AttachStdout": true,
            "AttachStderr": true,
            "Tty": true,
            "OpenStdin": true,
            "StdinOnce": true,
            "Env": [
                "PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin"
            ],
            "Cmd": [
                "/bin/bash"
            ],
            "Image": "centos",
            "Volumes": null,
            "WorkingDir": "",
            "Entrypoint": null,
            "OnBuild": null,
            "Labels": {
                "org.label-schema.build-date": "20210915",
                "org.label-schema.license": "GPLv2",
                "org.label-schema.name": "CentOS Base Image",
                "org.label-schema.schema-version": "1.0",
                "org.label-schema.vendor": "CentOS"
            }
        },
        "NetworkSettings": {
            "Bridge": "",
            "SandboxID": "8b76dc30fe9aeaeb8bc30c47ee860e1f8b58d30dc0e434cf798ee79abb22008a",
            "HairpinMode": false,
            "LinkLocalIPv6Address": "",
            "LinkLocalIPv6PrefixLen": 0,
            "Ports": {},
            "SandboxKey": "/var/run/docker/netns/8b76dc30fe9a",
            "SecondaryIPAddresses": null,
            "SecondaryIPv6Addresses": null,
            "EndpointID": "cdd2b8606426e15a829e1e80e111940a2e00e3b43779b70349d45d7c1b72e944",
            "Gateway": "172.17.0.1",
            "GlobalIPv6Address": "",
            "GlobalIPv6PrefixLen": 0,
            "IPAddress": "172.17.0.2",
            "IPPrefixLen": 16,
            "IPv6Gateway": "",
            "MacAddress": "02:42:ac:11:00:02",
            "Networks": {
                "bridge": {
                    "IPAMConfig": null,
                    "Links": null,
                    "Aliases": null,
                    "NetworkID": "25665ce78dd43a76185db37232982cbf04740b6a994f950058ef292fb21da2e3",
                    "EndpointID": "cdd2b8606426e15a829e1e80e111940a2e00e3b43779b70349d45d7c1b72e944",
                    "Gateway": "172.17.0.1",
                    "IPAddress": "172.17.0.2",
                    "IPPrefixLen": 16,
                    "IPv6Gateway": "",
                    "GlobalIPv6Address": "",
                    "GlobalIPv6PrefixLen": 0,
                    "MacAddress": "02:42:ac:11:00:02",
                    "DriverOpts": null
                }
            }
        }
    }
]

```



进入当前正在运行的容器

```shell
# 容器通常都是使用后台方式运行的，需要进入容器，修改一些配置

# 命令
docker exec -it 容器id bashShell
[root@centos01 /]# docker exec -it 56885854f7f1 /bin/bash

# 方式二
docker attach 容器id
#docker exec:进入容器后开启一个新的终端，可以在里面操作（常用）
#docker attach:进入容器正在执行的终端，不会启动新的进程！
```

从容器内拷贝文件到主机上

```shell
docker cp 容器id：容器内路径 目的主机路径

#拷贝是一个手动过程，未来我们使用 -v 卷的计数可以实现自动同步
```



## 练习

Docker安装Nginx

```shell
#1、搜索镜像
#2、下载镜像
#3、启动镜像

#-d:后台运行
#--name：给容器命名
#-p：宿主机端口：容器内端口

[root@centos01 home]# docker run -d --name nginx01 -p:3344:80 nginx
e713f4e7492efe32225add13e613687387758923212dabf47d4142f9b28cf2e8
[root@centos01 home]# docker ps
CONTAINER ID   IMAGE     COMMAND                  CREATED         STATUS         PORTS                                   NAMES
e713f4e7492e   nginx     "/docker-entrypoint.…"   4 seconds ago   Up 3 seconds   0.0.0.0:3344->80/tcp, :::3344->80/tcp   nginx01
[root@centos01 home]# curl localhost:3344
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
<style>
html { color-scheme: light dark; }
body { width: 35em; margin: 0 auto;
font-family: Tahoma, Verdana, Arial, sans-serif; }
</style>
</head>
<body>
<h1>Welcome to nginx!</h1>
<p>If you see this page, the nginx web server is successfully installed and
working. Further configuration is required.</p>

<p>For online documentation and support please refer to
<a href="http://nginx.org/">nginx.org</a>.<br/>
Commercial support is available at
<a href="http://nginx.com/">nginx.com</a>.</p>

<p><em>Thank you for using nginx.</em></p>
</body>
</html>

```



## 用完即删

通常用于测试：

```shell
docker run -it -rm tomcat:9.0
```



## 可视化

- portainer （先用这个）

```shell
 docker run -d -p 8080:9999 --restart=always -v /var/run/docker.sock:/var/run/docker.sock --privileged=true portainer/portainer
```





# Docker配置相关环境

## Prometheus

1、下载镜像包：

```shell
docker pull prom/prometheus
```

2、配置Prometheus

```shell
mkdir /opt/prometheus
cd /opt/prometheus/
vim prometheus.yml

#opt是linux中存放第三方软件的文件夹
```

内容如下：

```shell
global:
  scrape_interval:     60s
  evaluation_interval: 60s
  

scrape_configs:

  - job_name: prometheus
    static_configs:
      - targets: ['localhost:9090']
        labels:
          instance: prometheus

  - job_name: Data
    static_configs:
      - targets: ['192.168.37.184:5000']	#python中对应的端口号是5000，所以从主机5000端口接收数据
        labels:
          instance: Data
  
alerting:
  alertmanagers:
    - static_configs:
      - targets: ['172.17.0.1:9093']	#这里的ip是docker的ip地址，也可以改为宿主机的ip地址，但对应暴露的端口号也要改变

rule_files:
  - rules.yml


```

3、配置rules.yml，即alert的规则

```shell
vim /opt/prometheus/rules.yml
```

内容如下：

```yaml
groups:
- name: server-alarm
  rules:
  - alert: "内存告警"
    expr: (1 - (node_memory_MemAvailable_bytes / (node_memory_MemTotal_bytes))) * 100 > 80
    for: 1m
    labels:
      severity: warning
    annotations:
      summary: "{{$labels.instance}}: 检测到 高内存 使用率！"
      description: "{{$labels.instance}}: 内存使用率在 80% 以上 (当前使用值为:{{ $value }})"


  - alert: "CPU告警"
    expr: (1 - avg(irate(node_cpu_seconds_total{mode="idle"}[2m])) by(instance)) * 100 > 80
    for: 1m
    labels:
      severity: warning
    annotations:
      summary: "{{$labels.instance}}: 检测到 高CPU 使用率！"
      description: "{{$labels.instance}}: CPU使用率在 80% 以上 (当前使用值为:{{ $value }})"


  - alert: "磁盘告警"
    expr: 100 - (node_filesystem_free_bytes{fstype=~"tmpfs|ext4"} / node_filesystem_size_bytes{fstype=~"tmpfs|ext4"} * 100) > 5
    for: 1m
    labels:
      severity: warning
    annotations:
      summary: "{{$labels.instance}}: 检测到 高磁盘 使用率！"
      description: "{{$labels.instance}}: 磁盘使用率在 80% 以上 (当前使用值为:{{ $value }})"
      
  - alert: "InstanceUp"
    expr: up == 1
    for: 1m
    labels:
      severity: warning
    annotations:
      summary: "{{ $labels.instance }}"
      description: "{{ $labels.instance }} of job {{ $labels.job }} has been up for more than 1 minutes."
```

4、网络准备

因为需要在本地运行两个容器，且两个容器需要互相能否访问，所以这里创建一个 network。

```shell
docker network create prom
```

查看：

```shell
[root@centos01 ~]# docker network ls
NETWORK ID     NAME      DRIVER    SCOPE
086bf47d28da   bridge    bridge    local
19357bf4da6f   host      host      local
14df82a9ab10   none      null      local
e09f984b92bc   prom      bridge    local

[root@centos01 ~]# docker inspect e09f984b92bc
[
    {
        "Name": "prom",
        "Id": "e09f984b92bc77fcaa9b5c6b1090938dde918f739b5eb67a18e56eb1f8c4eead",
        "Created": "2022-07-13T15:02:27.09181599+08:00",
        "Scope": "local",
        "Driver": "bridge",
        "EnableIPv6": false,
        "IPAM": {
            "Driver": "default",
            "Options": {},
            "Config": [
                {
                    "Subnet": "172.18.0.0/16",
                    "Gateway": "172.18.0.1"
                }
            ]
        },
        "Internal": false,
        "Attachable": false,
        "Ingress": false,
        "ConfigFrom": {
            "Network": ""
        },
        "ConfigOnly": false,
        "Containers": {},
        "Options": {},
        "Labels": {}
    }
]
```

5、启动Prometheus

```shell
docker run  -d --name prometheus --restart=always -p 9090:9090 -v /opt/prometheus/prometheus.yml:/etc/prometheus/prometheus.yml -v  /opt/prometheus/rules.yml:/etc/prometheus/rules.yml  prom/prometheus
```

## AlertManager

### 环境配置及运行

1、准备镜像

```shell
docker pull prom/alertmanager
```

2、Alertmanager配置：

- 先用docker将其运行起来

```shell
docker run -d --name alertmanager --network prom -p 9093:9093 prom/alertmanager
```

- 再将容器中的的alertmanager文件夹复制到宿主机内的/opt/alertmanager中

```shell
docker cp alertmanager:/etc/alertmanager /opt/alertmanager
```

- 修改alertmanager.yml配置文件

```shell
vim /opt/alertmanager/alertmanager.yml
```

如下：

```yaml
global:
  resolve_timeout: 5m
  smtp_smarthost: 'eng-smtp.calix.local:25'
  smtp_from: 'Prometheus@calix.com'
  smtp_auth_username: 'calix'
  smtp_auth_password: 'Calix123'
  smtp_require_tls: false
  

route:
  group_by: ['IP','alertname']
  group_wait: 20s
  group_interval: 5m
  repeat_interval: 30m
  receiver: Test
  routes:
    - match:
        IP: 10.245.48.240|10.245.48.28
      receiver: West_lake
    - match:
        IP: 10.245.48.21|10.245.48.22|10.245.48.23
      receiver: LHotse
    - match:
        IP: 10.245.48.25|10.245.48.30
      receiver: Potala
    - match:
        IP: 10.245.48.26|10.245.48.29
      receiver: Purple_mountain
    - match:
        IP: 10.247.19.4|10.247.19.5|10.247.19.6|10.247.19.7|10.247.19.8|10.247.19.9|10.247.19.10|10.247.19.13|10.247.19.14|10.247.19.15
      receiver: City_fibra
      
receivers:
- name: 'Test'
  email_configs:
  - to: 'corey.shi@calix.com,916933425@qq.com'
    send_resolved: true

- name: 'West_lake'
  email_configs:
  - to: 'sean.wang@calix.com,sam.chen@calix.com'
    send_resolved: true

- name: 'City_fibra'
  email_configs:
  - to: 'paul.zhang@calix.com,sam.chen@calix.com'
    send_resolved: true
    
- name: 'LHotse'
  email_configs:
  - to: 'philip.chen@calix.com,sam.chen@calix.com'
    send_resolved: true

- name: 'Potala'
  email_configs:
  - to: 'lincoln.yu@calix.com,sam.chen@calix.com'
    send_resolved: true

- name: 'Purple_mountain'
  email_configs:
  - to: 'jerry.wu@calix.com,sam.chen@calix.com'
    send_resolved: true


inhibit_rules:
  - source_match:
      severity: 'critical'
    target_match:
      severity: 'warning'
    equal: ['alertname', 'dev', 'instance']

```

- 把宿主机的文件夹挂载到容器上，再运行，这样修改宿主机的yml文件就能容器内文件也对应改变

```shell
docker run -itd --name alertmanager -p 9093:9093 -v /opt/alertmanager:/etc/alertmanager prom/alertmanager
```

### yml配置文件详解

```yaml
## Alertmanager 配置文件
global:
  resolve_timeout: 5m
  # smtp配置
  smtp_from: "123456789@qq.com"	 # 邮件发送地址
  smtp_smarthost: 'smtp.qq.com:465'	# 邮箱SMTP 服务地址
  smtp_auth_username: "123456789@qq.com" # 邮件发送地址用户名
  smtp_auth_password: "auth_pass"	# 邮件发送地址授权码
  smtp_require_tls: true
# email、企业微信的模板配置存放位置，钉钉的模板会单独讲如果配置。
templates:
  - '/data/alertmanager/templates/*.tmpl'
# 路由分组
route:
  receiver: ops
  group_wait: 30s # 在组内等待所配置的时间，如果同组内，30秒内出现相同报警，在一个组内出现。
  group_interval: 5m # 如果组内内容不变化，合并为一条警报信息，5m后发送。
  repeat_interval: 24h # 发送报警间隔，如果指定时间内没有修复，则重新发送报警。
  group_by: [alertname]  # 报警分组
  routes:
      - match:
          team: operations
        group_by: [env,dc]
        receiver: 'ops'
      - match_re:
          service: nginx|apache
        receiver: 'web'
      - match_re:
          service: hbase|spark
        receiver: 'hadoop'
      - match_re:
          service: mysql|mongodb
        receiver: 'db'
# 接收器
# 抑制测试配置
      - receiver: ops
        group_wait: 10s
        match:
          status: 'High'
# ops
      - receiver: ops # 路由和标签，根据match来指定发送目标，如果 rule的lable 包含 alertname， 使用 ops 来发送
        group_wait: 10s
        match:
          team: operations
# web
      - receiver: db # 路由和标签，根据match来指定发送目标，如果 rule的lable 包含 alertname， 使用 db 来发送
        group_wait: 10s
        match:
          team: db
# 接收器指定发送人以及发送渠道
receivers:
# ops分组的定义
- name: ops
  email_configs:
  - to: '9935226@qq.com,10000@qq.com'
    send_resolved: true
    headers:
      subject: "[operations] 报警邮件"
      from: "警报中心"
      to: "小煜狼皇"
  # 钉钉配置
  webhook_configs:
  - url: http://localhost:8070/dingtalk/ops/send
    # 企业微信配置
  wechat_configs:
  - corp_id: 'ww5421dksajhdasjkhj'
    api_url: 'https://qyapi.weixin.qq.com/cgi-bin/'
    send_resolved: true
    to_party: '2'
    agent_id: '1000002'
    api_secret: 'Tm1kkEE3RGqVhv5hO-khdakjsdkjsahjkdksahjkdsahkj'

# web
- name: web
  email_configs:
  - to: '9935226@qq.com'
    send_resolved: true
    headers: { Subject: "[web] 报警邮件"} # 接收邮件的标题
  webhook_configs:
  - url: http://localhost:8070/dingtalk/web/send
  - url: http://localhost:8070/dingtalk/ops/send
# db
- name: db
  email_configs:
  - to: '9935226@qq.com'
    send_resolved: true
    headers: { Subject: "[db] 报警邮件"} # 接收邮件的标题
  webhook_configs:
  - url: http://localhost:8070/dingtalk/db/send
  - url: http://localhost:8070/dingtalk/ops/send
# hadoop
- name: hadoop
  email_configs:
  - to: '9935226@qq.com'
    send_resolved: true
    headers: { Subject: "[hadoop] 报警邮件"} # 接收邮件的标题
  webhook_configs:
  - url: http://localhost:8070/dingtalk/hadoop/send
  - url: http://localhost:8070/dingtalk/ops/send

# 抑制器配置
inhibit_rules: # 抑制规则
  - source_match: # 源标签警报触发时抑制含有目标标签的警报，在当前警报匹配 status: 'High'
      status: 'High'  # 此处的抑制匹配一定在最上面的route中配置不然，会提示找不key。
    target_match:
      status: 'Warning' # 目标标签值正则匹配，可以是正则表达式如: ".*MySQL.*"
    equal: ['alertname','operations', 'instance'] # 确保这个配置下的标签内容相同才会抑制，也就是说警报中必须有这三个标签值才会被抑制。

```

## Grafana

1、拉取镜像

```shell
docker pull grafana/grafana
```

新建空文件夹grafana-storage，用来存储数据

```shell
mkdir /opt/grafana-storage
```

2、添加权限

```shell
chmod 777 -R /opt/grafana-storage
```

3、启动grafana

```shell
docker run -d --name grafana --restart=always -p 521:3000 --name=grafana -v /opt/grafana-storage:/var/lib/grafana grafana/grafana
```

4、访问url：

http://ip:521/

5、登陆

默认的用户密码都是：admin

## Mysql

### 安装和配置

1、安装mysql

```shell
docker pull mysql
```

2、创建必要的文件夹

```shell
cd /opt # 我的软件都安装在/opt目录下。
mkdir mysql_docker
cd mysql_docker/
echo $PWD # 当前位置的绝对路径
```

3、启动mysql容器

```shell
docker run --name mysqlserver --restart=always --privileged=true -v $PWD/conf:/etc/mysql/conf.d -v $PWD/logs:/logs -v $PWD/data:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=123456 -d -i -p 3306:3306 mysql:latest

# $PWD是当前位置的绝对路径，所以必须在2中创建的文件夹下执行该命令
# --privileged=true：在docker run中加入 --privileged=true 给容器加上特定权限
# --name mysqlserver :指定容器的名字为mysqlserver 
# -v $PWD/conf:/etc/mysql/conf.d：将主机当前目录下的 conf/my.cnf 挂载到容器的 /etc/mysql/my.cnf。
# -v $PWD/data:/var/lib/mysql ：将主机当前目录下的data目录挂载到容器的 /var/lib/mysql 。
# -e MYSQL_ROOT_PASSWORD=123456：初始化 root 用户的密码。
```

4、测试mysql登录

```shell
docker exec -it mysqlserver /bin/bash # 也可以把容器名"mysqlserver"改为容器id
mysql -uroot -p
#设置了初始化密码是123456
```

5、测试远程连接

```shell
firewall-cmd --zone=public --add-port=3306/tcp --permanent # 开放3306端口
firewall-cmd --reload # 配置立即生效
firewall-cmd --zone=public --list-ports
```



### Mysql语法

增加列：alter table up_time add CARD1_REBOOT_TIMES INT(10);

删除表数据：delete from up_time;



### 复制mysql（本地到虚拟机）

1、在虚拟机创建一个数据库用来导入（最好数据库名字与本机中的相同）

2、从虚拟机登录本地mysql

```shell
mysql -uroot -p -h192.168.37.184 -P3306	#这一步是进入mysql的，可以不做，直接到第三步
```

3、备份本地数据库到虚拟机

**以下都是在虚拟机mysql命令行执行，不在登录后的"mysql>"中执行**

- 导出：连接本地mysql，导出数据库

```shell
mysqldump -h192.168.37.184 -P3306 -uroot -proot monitor > bak.sql;
#格式：mysqldump -h链接ip -P(大写)端口 -u用户名 -p密码 数据库名>d:XX.sql(路径)
```

- 导入：导入到虚拟机mysql

```shell
mysql -uroot -ppassword monitor <bak.sql
```

**注意：-p可以放在最后，因为密码显式输入会报warning**



### 遇到的问题

- Host * is not allowed to connect to this MySQL server（多次登陆失败）

```shell
#在要访问的那个远程mysql中执行以下命令
mysql -u root -p

mysql>use mysql;

mysql>update user set host ='%'where user ='root';

mysql>flush privileges;
```



# Docker遇到的问题

- Cannot connect to the Docker daemon at unix:///var/run/docker.sock. Is the docker daemon running?

```shell
service docker restart
```

- Prometheus时间不同步

```shell
ntpdate ntp.aliyun.com
```

