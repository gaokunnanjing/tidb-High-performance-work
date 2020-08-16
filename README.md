# tidb-High-performance-work
tidb高性能

## 一 work1源码编译部署tidb,并修改源码,在事务开启处打印 hello transaction
### 1先安装go环境和rust环境,参见网上资料
go版本是
https://www.rust-lang.org/en-US/other-installers.html<br>
注意选择nightly版本下载安装，
将url中的www.rust-lang.org替换成mirrors.ustc.edu.cn/rust-static会更快<br>
rust版本是<br>

环境变量配置

### 2下载tidb,pd和tikv源代码
git clone https://github.com/tikv/tikv.git
git clone https://github.com/pingcap/pd.git
git clone https://github.com/pingcap/tidb.git
放置在一个单独的文件夹下

### 3编译tidb,pd和tikv

中间遇到一个问题<br>

根据百度方法,使用如下设置解决<br>
cargo rustc -- -C link-arg=-nostartfiles<br>
编译成功之后,会生成下面的可执行程序

### 4单机部署与测试
首先进入相应文件夹,按顺序启动 pd,tikv和tidb<br>
启动pd-server<br>
./bin/pd-server --name=pd1 \
--data-dir=pd1 \
--client-urls="http://127.0.0.1:2379" \
--log-file=pd.log

启动三个KV进程<br>
./tikv-server --pd="127.0.0.1:2379" \
--addr="127.0.0.1:20160" \
--data-dir=tikv1 \
--log-file=tikv1.log

.tikv-server --pd="127.0.0.1:2379" \
--addr="127.0.0.1:20161" \
--data-dir=tikv2 \
--log-file=tikv2.log

./tikv-server --pd="127.0.0.1:2379" \
--addr="127.0.0.1:20162" \
--data-dir=tikv3 \
--log-file=tikv3.log

启动tidb<br>
./bin/tidb-server --store=tikv \
--path="127.0.0.1:2379" \
--log-file=tidb.log

然后使用 mysql client进行连接<br>
mysql -h 127.0.0.1 -P 4000 -u root -D test

### 5修改源代码,重新部署,使得开启事务时打印 hello transaction
修改txn.go 514行 startTS函数<br>

重新编译部署后运行结果<br>

