## WiFi Based Localization/ Indoor Human Activity Recognition

### 文章列表
- Electronic Frog Eye: Counting Crowd Using Wifi 
- 802.11 with Multiple Antennas for Dummies (Daniel Halperin. Univ. of Washington & Intel Labes)
- Non-Invasive Detection of Moving and Stationary Human with Wifi (Liu Yunhao)
- Human Mobility Enhances Global Positioning Accuracy for Mobile Phone Localization (Liu Yunhao)
- E-eyes: Device-free Location-oriented Activity Identification Using Fine-grained WiFi Signal(Stevens Institude of Technology, USA)
- Wisee: Whole-Home Gesture Recognition Using Wireless Signals(Qifan Pu, Univ. of Washington)

### Electronic Frog Eye

### E-eye
综述
文章利用CSI信息识别原地和移动的动作，使用半监督学习获得行为profile作为识别的对照。
主要focus在location-oriented 活动识别（即，人类活动与场所的联系，比如厕所刷牙，厨房做饭）
- 用较少的发送设备从CSI信息中提特征，识别房屋中的in-place活动
- 利用一个wifi ap和几个设备的框架，采用相关Profile matching 算法匹配已知活动
- 动态构建profile，若无线设备移动了/替换了可以动态自适应
- 实验：2套公寓四个月数据、1个AP3个连接设备，精确区分8个跨房间的走路活动(20轮)，9个日常活动（59轮）和其他活动(100轮)，精确度96%，1个device则92%
- 宽信道可以增加准确率

限制：只能识别单人，无宠物和其他移动物体

方法
1. 区分walking和in-place activity: 前者幅度variance 大，后者幅度variance小但是有repeat Pattern；基于variance阈值区分两种活动
2. walking movement： Multiple-Dimensional Dynamic Time Warping(MD-DTW)，归一化不同速度的行为；in-place activity: 比较CSI分布直方图，用Earth Mover Distance(EMD)作为距离度量

### WiSee
综述
文章提出利用WiFi多普勒频移的方法来检测人类手势
- 提出了一个能在LOS,NLOS, TTW环境下使用的无线系统Wisee
- 展示了一个从无线信号种提取手势信息的算法：从宽带OFDM传输中提取多普勒频移，并对９种手势进行分类识别

方法
1. 从无线信号中提取多普勒频移
$$
\Delta f \propto \frac{2vcos(\theta)}{c} f
$$
难度在于在无线环境下人类运动导致的多普勒频移非常小；以WIFI为例，5GHZ对应17HZ(挥手0.5m/s)，被频谱覆盖
解决方案：接收端转化接收信号为窄带脉冲
特征：接收端对M个重复的OFDM symbol作MN点FFT时，每个信道的带宽减少了M倍，故而每个子信道都有窄带脉冲
方法：在接收端把所有的arbitrary　symbol转化为与第一个一样的symbol,获得窄带脉冲而不影响信号的多普勒信息。
疑难：频移、发送端需连续发送符号、循环前缀
2. 映射多普勒频移到手势
半秒FFT得到分辨率为２HZ的频谱，使用带通滤波器获得８～１３４HZ的频段信息
信号分段：手势特征由正负多普勒频移构成，　根据信噪比阈值，手势开始时信噪比变大，结束时信噪比变小，根据SNR和阈值关系判断信号起止位置。
手势分类：映射每个类为３位'1''-1''2'的数字组合，表示多普勒频移的方向
3. 多人干扰：目标用户执行特定动作，接收端与preamble匹配,同时根据接收到的preamble信息找到权值W对应的MIMO channel使得多普勒能量最大
$$
D_m = \sum_{i=1}^N W_n D_{nm}
$$
N为接收端天线数，$$$D_{nm}$$$ 表示第n个天线上第m段的多普勒能量，梯度下降寻找最小值
从而锁定target　user的方向
4. 多径效应
多径和镜面反射等干扰会导致多普勒频移正反颠倒。在分段中Wisee只需捕捉能量最大的路径，所以不需要太多天线来区分多径效应。而多径造成的正负多普勒频移颠倒只需利用preamble来纠正，因为默认对着接收端的正推是preamble信号


实现步骤





