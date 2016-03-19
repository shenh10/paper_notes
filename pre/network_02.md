
## 计算机网络系统方法阅读笔记(Computer Networks A System Approach)

### Chap2
网络适配器 CSR(Control State Register)
内存系统有效吞吐量
``` latex
Throughput = TransferSize / TransferTime
TransferTime = RTT + 1/Bandwidth * TransferSize
```

##### 链路
电缆：5类双绞线、同轴电缆、光纤
租用线路：传统电话服务(POTS)、综合业务数字网(ISDN)、digital subscribe line(xDSL,ADSL, VDSL)、电缆调制解调器(有线电视CATV)
香农定理：
可达信道容量 
$$ C = log_2(1+ SNR) $$


##### 无线链路
免许可频段限制：1. 限制传输功率 2. 扩展频谱（调频frequency hopping和直接序列 direct access）；802.11(Wifi) 802.15.1(蓝牙) 802.16(wimax)
##### 编码
NRZ(non-return to  zero， 1-0，基线漂移,难以时钟恢复)
NRZI(Non-return to zero inverted, 信号跳变编码1。无法解决连续零) 
曼切斯特编码(Manchester Encoding: 由低到高编码0，高到低1)、查分曼切斯特编码
4B/5B 冗余编码
比特率、波特率

### 组帧
**面向字节的协议（PPP）**：起止标记法(正文开始符STX、结束符ETX、CRC，数据链路转义DLE) 字节计数符
**面向比特的协议（HDLC）**
**基于时钟的组帧（Synchronous Optical Network, SONET）**:光网络远距离传输标准 STS-N
**差错检测**：二维奇偶校验(two-dimensional parity， 7bit + 1，整体再取一次奇偶校验) 校验和(checksums。反码相加取反) 循环冗余校验(cyclic redundancy check 多项式整除， CRC32)
**可靠传输**： 确认(acknowledgement)和超时(timeout) 重传(retransmit)
使用确认和超时实现可靠传输的策略称为自动请求重发（ARQ: 停止和等待， 滑动窗口)

>滑动窗口算法：
> 发送端：SWS(send window size)/ LAR(lastest acknowledgement recieved) / LFS(last frame sent)
> $$ LAF-LAR \le SWS $$
> 接收端：RWS(recieve window size)/ LAF(last acceptable frame)/ LFR(last frame recieved)
> $$ LAF-LFR \le RWS $$
> 当接收到一个帧，接收端查看是否在接收方窗口内，若在则等待第一个连续frame序列，发送最大frame的ACK
> 循环使用帧编号，需要考虑接受发送窗口相对大小
> RWS= 1时
> $$SWS \le MaxSeqNum -1 $$
> RWS=SWS时
> $$ SWS < (MaxSeqNum + 1)/2$$
> 滑动窗口的实现：信号量锁定窗口资源
> 滑动窗口的３个任务：１．不可靠的链路上可靠地传输帧　２．保持帧的传输顺序　３．流量控制

##### 以太网(802.3)
带冲突检测的载波监听多路访问（CSMA/CD）

物理特性：收发器(transceiver，连接到分接头，检测线路空闲)、中继器(repeater，任意主机间最多４台)　10Base2细缆
帧格式
|-|

64| 48|48|16 |32
|-----------|
| 前同步码preamble｜目的地址|源地址| 类型　｜　帧体　| CRC





