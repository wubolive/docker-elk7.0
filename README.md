官方文档：https://www.elastic.co/guide/en/elasticsearch/reference/current/docker.html

### 拉取Docker镜像

```
docker pull docker.elastic.co/elasticsearch/elasticsearch:7.0.0
docker pull docker.elastic.co/kibana/kibana:7.0.0
docker pull mobz/elasticsearch-head:5
```

### 拉取docker-compose文件

```
git clone git@github.com:wubolive/docker-elk7.0.git
```

### 优化内核配置

```
echo "vm.max_map_count=262144" >> /etc/sysctl.conf && sysctl -p
vim /etc/security/limits.conf 
# 表示任何一个用户可以打开的最大的文件描述符数量
*               soft    nofile         655350 
*               hard    nofile         655350
# 表示任何一个用户可以打开的最大的进程数
*               soft    nproc          655350 
*               hard    nproc          655350
* soft memlock unlimited
* hard memlock unlimited
```

### 给予elasticsearch目录权限

```
chmod g+rwx elasticsearch
chown 1000:1000 elasticsearch
```

### 启动ES集群

```
docker-compose up -d
```

### 查看启动状态

```
[root@elk-stask home]# docker-compose ps 
  Name                 Command               State                Ports               
-------------------------------------------------------------------------------------
Es-Head     /bin/sh -c grunt server          Up      0.0.0.0:9100->9100/tcp           
Es-Master   /usr/local/bin/docker-entr ...   Up      0.0.0.0:9200->9200/tcp, 9300/tcp 
Es-Node1    /usr/local/bin/docker-entr ...   Up      9200/tcp, 9300/tcp               
Es-Node2    /usr/local/bin/docker-entr ...   Up      9200/tcp, 9300/tcp               
kibana      /usr/local/bin/kibana-docker     Up      0.0.0.0:5601->5601/tcp   
```

集群启动将会暴露3个端口：

* 0.0.0.0:9100(elasticsearch)
* 0.0.0.0:9200(elasticsearch-Head)
* 0.0.0.0:5601(kibana)

