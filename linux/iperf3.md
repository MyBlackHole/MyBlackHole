# iperf3


## Install
```
paru -S iperf3
```

## Usage

- tcp (默认)
```
# server
iperf3 -s -p 6666

# client
iperf3 -c 127.0.0.1 -p 6666 -P 4 -t 400
```

- udp
```
# server
iperf3 -s -p 6666
# client
iperf3 -c 127.0.0.1 -p 6666 -u -b 10m -t 400
```

- 多线程
```
# server
iperf3 -s -p 6666 -P 4
# client
iperf3 -c 127.0.0.1 -p 6666 -P 4 -t 400
```

## 参考
- [iperf3](https://iperf.fr/iperf-doc.php)
