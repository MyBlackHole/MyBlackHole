# statsd
statsd是一个udp或tcp的守护进程。使用简单的行协议收集客户端的metic数据。statsd使用udp的好处
StatsD系统包括三部分：客户端（client）、服务器（server）和后端（backend）
用于采集数据

UDP:
- UDP不需要建立连接，速度很快，不会影响应用程序的性能。
- “fire-and-forget”机制，就算statsd server挂了，也不会造成应用程序crash。

## 协议
```shell
<bucket>:<value>|<type>[|@sample_rate]
bucket：是一个metric的标识，可以看成一个metric的变量。
value：metrics值，通常是数字。
type：metric的类型，通常有timer、counter、gauge和set四种。
sample_rate：采样率，降低客户端到statsd服务器的带宽。客户端减少数据上报的频率，然后在发送的数据中加入采样频率，如0.1。statsd server收到上报的数据之后，如cnt=10，得知此数据是采样的数据，然后flush的时候，按采样频率恢复数据来发送给backend，即flush的时候，数据为cnt=10/0.1=100，而不是容易误解的10*0.1=1。
```

### 指标 metric
statsd 有四种指标类型：counter、timer、gauge和set。

#### 计数器 counter
counter类型的指标，用来计数。在一个flush区间，把上报的值累加。值可以是正数或者负数
```shell
user.logins:10|c        // user.logins + 10
user.logins:-1|c        // user.logins - 1 
user.logins:10|c|@0.1   // user.logins + 100
                        // users.logins = 10-1+100=109
```

#### 计时器 timer
timers 用来记录一个操作的耗时，单位ms
statsd会记录平均值(mean)、最大值(upper)、最小值(lower)、累加值(sum)、平方和(sum_squares)、个数(count)以及部分百分值
```shell
rpt:100|g
如下是在一个flush期间，发送了一个rpt的timer值100。以下是记录的值。
count_80: 1,
mean_80: 100,
upper_80: 100,
sum_80: 100,
sum_squares_80: 10000, 
std: 0,
upper: 100,
lower: 100,
count: 1,
count_ps: 0.1,
sum: 100,
sum_squares: 10000,
mean: 100,
median: 100
```
[细节](https://www.cnblogs.com/ygunoil/p/12394143.html)


## 配置
```shell
默认的配置文件
exampleConfig.js
```

## docker 
```shell
docker pull statsd/statsd

docker run -p 8125:8125 -p 8126:8126 --name statsd -d statsd/statsd
```

## docker-compose
```shell
# 官方 源码 docker-compose
version: '2'
services:
  statsd:
    build: .
    links:
    - carbon:graphite
    ports:
    - 8125:8125/udp
    - 8126:8126

  graphite-web:
    image: dockerana/graphite
    links:
    - carbon
    ports:
    - 8000:8000
    volumes_from:
    - carbon

  carbon:
    image: dockerana/carbon
    ports:
    - 2003:2003
    - 2004:2004
    - 7002:7002
    volumes:
    - /opt/graphite

docker-compose up -d --build
```

## 概念
### buckets
当一个 Whisper 文件被创建，它会有一个不会改变的固定大小
在这个文件中可能有多个 buckets 对应于不同分辨率的数据点，每个 bucket 也有一个保留属性指明数据点应该在 bucket 中应该被保留的时间长度，Whisper 执行一些简单的数学计算来计算出多少数据点会被实际保存在每个 bucket 中

### Values
每个 stat 都有一个 value，该值的解释方式依赖于 modifier。通常，values 应该是整数

### Flush interval
在 flush interval (冲洗间隔，通常为10秒) 超时之后，stats 会聚集起来，传送到上游的后端服务



## 例子

- 测试
```shell
echo "foo:1|c" | nc -w 1 -u 127.0.0.1 8125
```
