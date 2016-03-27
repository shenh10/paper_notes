
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
MAC层(前同步码8字节+14字节首部+4字节CRC)

|64| 48|48|16 ||32
|-----------|
| 前同步码preamble| 目的地址|源地址| 类型　|　帧体　| CRC 

单播、多播、广播地址（全1）、混杂模式


发送器算法：p-坚持，collide情况下发送器最少发送96位（12字节）（64位前同步码+32位干扰序列），最多情况下发送512位至更多（发送到目的端时才冲突，以太网最小帧长度14字节首部+46字节数据+4字节CRC）
以太网最大长度2500m，因为构成它的同轴电缆最长500m，最多接4个中继器；最大延时限定为51.2us，在10Mbps以太网上对应512bit
指数避退(exponential backoff) k* 51.2us k: 0~2^n-1; n <=16

##### 环网(802.5、 FDDI、RPR)

- 环网与以太网共同点：包括一个分布式算法控制每个节点何时传输；所有节点可看到所有的帧

令牌环
- token ring：token（特定二进制序列）每个节点接受令牌然后转发，发送帧则取下token换上数据，转一轮回到发送方把数据取下插入token；中继器多站访问单元MSAU隔离坏节点；
- THT(Token Holding Time)限定一个节点持有令牌时间；优先级保证公平(reservation位)
- monitor保证令牌始终存在。任一节点可申请成为monitor
- 帧格式

|8|8|8| 48|48||32|8|8
|-----------|
| start delimiter| access control| frame control| 目的地址|源地址|　帧体　| CRC | end delimiter| frame status

FDDI( Fiber Distributed Data Interface )：双环反向、备用光纤线路

弹性分组环(Resilient Packet Ring, RPR)
- 双环反向、接收端取下帧
- buffer insertion
- QOS
- 故障恢复： wrapping, steering（临近节点通知其他节点）
- MAN（城域网）