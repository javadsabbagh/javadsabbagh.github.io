---
title: Clustering Redis
date: 2019-09-30 13:08:03
tags: [redis]
---

## Redis clustering model
For creating redis cluster you need at least three master nodes. Cluster topolgy for master nodes is ring and every worker(salve) node is connected to a master. 


## Different clustering options
1. redis-cli
2. ruby

## Running redis nodes
We run redis instances in Docker containers, then:
```bash
docker pull redis
```

Suppose, we have three machines: 117, 118, and 119. In every machine, run two docker instances of redis:

```bash
docker run -d --rm --name redis-117-1 --net host redis redis-server --port 6379 --cluster-enabled yes --cluster-config-file nodes.conf --cluster-node-timeout 5000 --appendonly yes
docker run -d --rm --name redis-117-2 --net host redis redis-server --port 6380 --cluster-enabled yes --cluster-config-file nodes.conf --cluster-node-timeout 5000 --appendonly yes
```

Options: 
- **-d** means daemon mode, opposite to interactive mode(**-it**).
- **- -net host** means use physical interface for networking, instead of default bridge driver.
- **redis-server** is an application in redis container we are aiming to run. The rest are arguments to redis-server programs.
- *Security consideration*:
There is a trojan for redis conatiners, so if the machine publicly available on internet make sure it is only bounded to localhost interface:
```
-p 127.0.0.1:6379:6379
```

Make sure docker containers are running:
```
# docker ps
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS               NAMES
ae9e87cad613        redis               "docker-entrypoint.s…"   4 seconds ago       Up 3 seconds                            redis-117-2
b46238ad09af        redis               "docker-entrypoint.s…"   14 seconds ago      Up 14 seconds                           redis-117-1
```

Which ports are being listened by redis instances:
```
# ss -ntlp  | grep redis
LISTEN     0      128          *:6379                     *:*                   users:(("redis-server",pid=2724,fd=7))
LISTEN     0      128          *:6380                     *:*                   users:(("redis-server",pid=2800,fd=7))
LISTEN     0      128          *:16379                    *:*                   users:(("redis-server",pid=2724,fd=11))
LISTEN     0      128          *:16380                    *:*                   users:(("redis-server",pid=2800,fd=11))
LISTEN     0      128         :::6379                    :::*                   users:(("redis-server",pid=2724,fd=6))
LISTEN     0      128         :::6380                    :::*                   users:(("redis-server",pid=2800,fd=6))
LISTEN     0      128         :::16379                   :::*                   users:(("redis-server",pid=2724,fd=10))
LISTEN     0      128         :::16380                   :::*                   users:(("redis-server",pid=2800,fd=10))
```

- *:6379 means port is bounded to IPV4 interface.
- :::6379 means port is bounded to IPV6 interface.
- port 6379 is used for clients connecting redis.
- port 16379 is used for cluster communications.

## Cluster setup
```
export cluster_hosts="192.168.100.117:6379 192.168.100.118:6379  192.168.100.119:6379 192.168.100.117:6380 192.168.100.118:6380  192.168.100.119:6380"
docker run -i --net host --rm redis redis-cli --cluster create $cluster_hosts --cluster-replicas 1
```

The output should be something like this:
```
>>> Performing hash slots allocation on 6 nodes...
Master[0] -> Slots 0 - 5460
Master[1] -> Slots 5461 - 10922
Master[2] -> Slots 10923 - 16383
Adding replica 192.168.100.118:6380 to 192.168.100.117:6379
Adding replica 192.168.100.119:6380 to 192.168.100.118:6379
Adding replica 192.168.100.117:6380 to 192.168.100.119:6379
M: 7b4c73012383e2056958d054265410825c68e43f 192.168.100.117:6379
   slots:[0-5460] (5461 slots) master
M: 10989ff2e637deb61327304822a2eb4e24fa9184 192.168.100.118:6379
   slots:[5461-10922] (5462 slots) master
M: 802243bcbb4acfdc66c6e6937d2c1bbade8dba72 192.168.100.119:6379
   slots:[10923-16383] (5461 slots) master
S: ceb0e5392edb85b8f2182f1b522e4d0781cea5bd 192.168.100.117:6380
   replicates 802243bcbb4acfdc66c6e6937d2c1bbade8dba72
S: ff871ea93201476b6de9c73f6bc8a6b62e922cb1 192.168.100.118:6380
   replicates 7b4c73012383e2056958d054265410825c68e43f
S: 5b655178414c62b0a2395c1c8e20ef27f0a03349 192.168.100.119:6380
   replicates 10989ff2e637deb61327304822a2eb4e24fa9184
Can I set the above configuration? (type 'yes' to accept): yes
>>> Nodes configuration updated
>>> Assign a different config epoch to each node
>>> Sending CLUSTER MEET messages to join the cluster
Waiting for the cluster to join
...
>>> Performing Cluster Check (using node 192.168.100.117:6379)
M: 7b4c73012383e2056958d054265410825c68e43f 192.168.100.117:6379
   slots:[0-5460] (5461 slots) master
   1 additional replica(s)
M: 10989ff2e637deb61327304822a2eb4e24fa9184 192.168.100.118:6379
   slots:[5461-10922] (5462 slots) master
   1 additional replica(s)
S: ceb0e5392edb85b8f2182f1b522e4d0781cea5bd 192.168.100.117:6380
   slots: (0 slots) slave
   replicates 802243bcbb4acfdc66c6e6937d2c1bbade8dba72
M: 802243bcbb4acfdc66c6e6937d2c1bbade8dba72 192.168.100.119:6379
   slots:[10923-16383] (5461 slots) master
   1 additional replica(s)
S: ff871ea93201476b6de9c73f6bc8a6b62e922cb1 192.168.100.118:6380
   slots: (0 slots) slave
   replicates 7b4c73012383e2056958d054265410825c68e43f
S: 5b655178414c62b0a2395c1c8e20ef27f0a03349 192.168.100.119:6380
   slots: (0 slots) slave
   replicates 10989ff2e637deb61327304822a2eb4e24fa9184
[OK] All nodes agree about slots configuration.
>>> Check for open slots...
>>> Check slots coverage...
[OK] All 16384 slots covered.
```


## Getting cluster nodes information
In one of the redis running machines, run

```
docker run -i --net host --rm redis redis-cli cluster nodes
```
Sample output:
```
ff871ea93201476b6de9c73f6bc8a6b62e922cb1 192.168.100.118:6380@16380 slave 7b4c73012383e2056958d054265410825c68e43f 0 1571211437523 5 connected
802243bcbb4acfdc66c6e6937d2c1bbade8dba72 192.168.100.119:6379@16379 master - 0 1571211436000 3 connected 10923-16383
5b655178414c62b0a2395c1c8e20ef27f0a03349 192.168.100.119:6380@16380 slave 10989ff2e637deb61327304822a2eb4e24fa9184 0 1571211437526 6 connected
10989ff2e637deb61327304822a2eb4e24fa9184 192.168.100.118:6379@16379 myself,master - 0 1571211436000 2 connected 5461-10922
7b4c73012383e2056958d054265410825c68e43f 192.168.100.117:6379@16379 master - 0 1571211436513 1 connected 0-5460
ceb0e5392edb85b8f2182f1b522e4d0781cea5bd 192.168.100.117:6380@16380 slave 802243bcbb4acfdc66c6e6937d2c1bbade8dba72 0 1571211437000 4 connected
```

## Connecting to cluster
**redis-cli** is a command-line tool used to connect redis. You should consider *-c* or *cluster* option when connecting to a redis node. You can connect to any cluster node (master or slave) and run a redis command:
```
docker run -it --rm --net=host redis redis-cli -c -h 192.168.100.117 -p 6380
```

- Note: If you don't specify -c option, you get an error whenever a key is located in another node. For example:
```bash
192.168.100.117:6379> get user_name
(error) MOVED 5798 192.168.100.118:6379
```
 It says that requested key is available on another server, i.e 118

- However, if you had connected in cluster mode, you would have get result with a message like this:
```
192.168.100.117:6380> get user_name
-> Redirected to slot [5798] located at 192.168.100.118:6379
"javad"
```
 As you see, redis-cli does the burdon of connecting to proper remote server, fetching, and returning the result.