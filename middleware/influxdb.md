# influxdb

```shell
docker run -d --name influxdb -p 8086:8086 influxdb
export INFLUXDB_TOKEN=3cZY5pYkkU3t3xXOwhXGNn6DvDs3wica8WQI5mTaHO6ME_rhuFSo7c773pkw_5n1slPSGANOYvyEJsz-U_RNbg==

export INFLUX_TOKEN=HcpkucDyqHDCMj905ZPc1P6rjQxPTAhQqlHq2C31V6y5GEaqTVqpbcFtoFqzrr3rFSeo7-BjxmRIRjPPWFc2xQ==
telegraf --config http://127.0.0.1:8086/api/v2/telegrafs/0b9eb14b216c1000
```
