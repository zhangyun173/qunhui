# tinycore-redpill
This is a testing version. Do not use unless you are certain you have no data to lose.

# Instructions 

A normal build process would start with :

1. Image burn

a. For physical gunzip and burn img file to usb stick

b. For virtual gunzip and use the provided vmdk file 

2. Boot tinycore

3. ssh to your booted loader or just open the desktop terminal 

4. Bring over your json files (global_config.json,custom_config.json, user_config.json )

5. Check the contents of user_config.json, if satisfied keep or else run :

a. Change you serial and mac address by running ./rploader.sh serialgen DS3615xs

b. Update user_config.json with your VID:PID of your usb stick by running ./rploader.sh identifyusb now

c. Update user_config.json with your SataPortMap and DiskIdxMap by running ./rploader.sh satamap now (needs testing)

d. Backup your changes to local loader disk by running  ./rploader.sh backup now


6. ./rploader.sh build bromolow-7.0.1-42218


[各台 Synology NAS 使用的 CPU 型号、套件架构及内存种类](https://kb.synology.com/zh-hk/DSM/tutorial/What_kind_of_CPU_does_my_NAS_have)

[编译工具](https://github.com/pocopico/tinycore-redpill)

支持编译的型号如下

|型號|CPU 型號|核心|執行緒|FPU|套件架構|記憶體|
|:--------:|:--------:|:--------:|:--------:|:--------:|:--------:|:--------:|
|DS3615xs|Intel Core i3-4130|雙核心|4|有|Bromolow|DDR3 ECC 4 GB|
|DS3617xs|Intel Xeon D-1527|四核心|8|有|Broadwell|DDR4 ECC SODIMM 16 GB|
|DS916+|Intel Pentium N3710|四核心|4|有|Braswell|DDR3 2 / 8 GB|
|DS918+|Intel Celeron J3455|四核心|4|有|Apollolake|DDR3L SODIMM 4GB|
|DS920+|Intel Celeron J4125|四核心|4|有|Geminilake|DDR4 4GB|
|DS3622xs+|Intel Xeon D-1531|六核心|12|有|Broadwellnk|DDR4 ECC SODIMM 16 GB|
|FS6400|Intel Xeon Silver 4110 x 2|八核心 x 2|16 x 2|有|Purley|DDR4 ECC RDIMM 32 GB|
|DVA3219|Intel Atom C3538|四核心|4|有|Denverton|DDR4 Non-ECC SODIMM 4GB|
|DVA3221|Intel Atom C3538|四核心|4|有|Denverton|DDR4 Non-ECC SODIMM 8GB|
|DS1621+|AMD Ryzen V1500B|四核心|8|有|V1000|DDR4 ECC SODIMM 4GB|


CPU对比表格里面尽量相似,方法:先是厂商(AMD,CPU)->CPU版本->核心数 

比如我的CPU是 AMD x64 2600 找到只有一个符合条件型号 DS1621+ 架构V1000

要先做个软路由翻墙 不然没发进行 建议全局 或者谁有心把URL替换到国内就方便了

1. 登录 
> 用户： tc 密码： P@ssw0rd

 设定 网关和dns 这个 自动获取的可以忽略
```
sudo route delete default gw 192.168.1.253 eth0;sudo route add default gw 192.168.1.254 eth0;sudo sed -i 's/.*/nameserver 8.8.8.8/g' /etc/resolv.conf;
```
1.更新脚本
```
sudo ./rploader.sh update now
```
全部更新
```
sudo ./rploader.sh fullupgrade now
```
1.查询 SataPortMap DiskIdxMap 

(注:这个不能用root权限 也就是不能加sudo 虚拟机是 "SataPortMap": "1", "DiskIdxMap": "00")
```
./rploader.sh satamap now  
```
4. 生成序列号和mac地址
```
./rploader.sh serialgen DS1621+
```
1. 获取USB的vid和pid  虚拟机用	"vid": "0x46f4",    "pid": "0x0001",
```
./rploader.sh identifyusb now
```

1. 然后编译对应的架构和版本 我的架构是V1000版本用最新的7.1.0-42661
```
sudo ./rploader.sh build ds1621p_42661
```
编译过程中如果遇到查找驱动失败的 手动添加的方法是
```
sudo ./rploader.sh ext ds1621p_42661 add https://raw.githubusercontent.com/pocopico/rp-ext/master/e1000/rpext-index.json
```
然后再重新编译

###### 注:如果遇到下载软件包失败DSM_DS918+_42661.pat 可以先在国内官网下载 在替换到相应的目录
###### [国内下载地址](https://www.synology.cn/zh-cn/support/download)
