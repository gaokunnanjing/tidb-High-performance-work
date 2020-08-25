# lesson2
tidb高性能

## 一 测试环境搭建
### 1机器软硬件配置
一台台式机<br>

![](https://github.com/gaokunnanjing/tidb-High-performance-work/blob/master/work2_images/%E5%BE%AE%E4%BF%A1%E5%9B%BE%E7%89%87_20200825214153.png) <br>
### 2测试方案
在单机上使用docker Compose搭建测试集群环境<br>
拓扑结构<br>
![](https://github.com/gaokunnanjing/tidb-High-performance-work/blob/master/work2_images/%E6%8B%93%E6%89%91.png)<br>


## 二开始测试

### 1sysbench<br>
导入数据<br>
sysbench oltp_point_select --config-file=config  --tables=32 --table-size=100000 prepare<br>
运行测试<br>
sysbench oltp_point_select --config-file=config  --threads=32 --tables=32 --table-size=100000 run<br>

测试结果<br>
![](https://github.com/gaokunnanjing/tidb-High-performance-work/blob/master/work2_images/sys.png) <br>


### 2go-ycsb
导入数据<br>
./bin/go-ycsb load mysql -P workloads/workloada -p recordcount=500000 -p mysql.host=172.18.0.1 -p mysql.port=4000 --threads 64<br>
运行测试<br>
./bin/go-ycsb run mysql -P workloads/workloada -p recordcount=500000 -p mysql.host=172.18.0.1 -p mysql.port=4000 --threads 64<br>

测试结果<br>
![](https://github.com/gaokunnanjing/tidb-High-performance-work/blob/master/work2_images/go-ycsb.png) <br>


### 3go-tpc
导入数据<br>
./bin/go-tpc tpcc -H 172.18.0.1 -P 4000 -D tpcc --warehouses 10 prepare <br>
运行测试<br>
./bin/go-tpc tpcc -H 172.18.0.1 -P 4000 -D tpcc --warehouses 10 prepare <br>
测试结果<br>
![](https://github.com/gaokunnanjing/tidb-High-performance-work/blob/master/work2_images/go-ycsb.png) <br>
