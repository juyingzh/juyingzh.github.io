---
layout: post
title:  应对新一代威胁的电子战
categories: [读书笔记]
tags: [电子战, 雷达, 通信]
---

* Content
{:toc}

书名：应对新一代威胁的电子战（EW 104: EW Against a New Generation of Threats）

作者： [美] David L. Adamy 著，朱松、王燕、常晋聃、王晓东 译

出版：电子工业出版社，2017年9月第1版



此书是EW100系列的第4本书。前两本书（《EW101：电子战基础》和《EW102：电子战进阶》）讲述了电子战的基础知识。第3本书（《EW103：通信电子战》）重点讲述通信电子战，主要是针对中东形势而写的。本书重点讲述当前电子战领域面临的新型威胁及其新的对抗技术。

## 第1章 引言

雷达威胁是指与雷达控制武器相关的雷达信号，包括：

  * 搜索和截获雷达
  * 跟踪雷达
  * 雷达处理器与导弹之间用于制导和数据传输的无线电链路

通信威胁包括：

  * 指挥控制通信
  * 综合防空系统各组成部分之间的数据链
  * 连接无人机与控制站的指挥和数据链
  * 引爆简易爆炸装置的链路
  * 用于军事目的的蜂窝电话链路

## 第2章 频谱战

### 2.1 战争的变化

以前只有经度、纬度、高度、时间四维战斗空间增加了频率维度，成为五维空间。

### 2.2 与传播相关的特定问题

距离对无线电传播影响很大。

一旦我们依赖来自多部接收机的输入，网络就会成为作战能力的关键。我们已经进入网络中心战。

### 2.3 连通

连通的需求：

  * 带宽
  * 时延
  * 吞吐率，以所需的速度传输信息
  * 信息保真度，从接收到的信息中恢复所需的信息
  * 信息安全
  * 传输安全
  * 干扰抑制，提供作战环境中所需的信息保真度
  * 抗干扰，防止敌方有意干扰下的信息保真度

人耳能处理大约15kHz的声音，但大多数信息有4kHz的频率携带。

### 2.4 干扰抑制

商用调频广播是首个广泛应用的扩频技术。扩频比被称为调制系数，是载波最大频偏与最高调制频率之比。商用调频频率分配间隔是100kHz。对于大的调制系数，传输带宽为：

$$
BW=2f_m\beta
$$
式中，BW为传输带宽，$f_m$ 为信号最大频率，$\beta$ 为调制系数。

输出信噪比改善（dB）公式为：

$$
SNR=RFSNR+5+2log\beta
$$

式中：SNR为输出信噪比（dB），RFSNR为检波前信噪比（dB）。

商用调频的最大调制频率为15kHz，调制系数为5，因此带宽为150kHz。使用通用解调器，RFSNR门限为12dB，则输出SNR为31dB，提高了19dB。根据传输信息的特点，发射机的预加重（提高较高调制频率的功率）和接收机的去加重（较低较高调制频率的功率）还能将SNR提高数dB。

军用扩频系统采用三种调制类型：跳频、线性调频、直接序列扩频。

### 2.8 赛博与电子战

赛博攻击使用的恶意代码包括：病毒（Virus）、计算机蠕虫（Computer Worm）、特洛伊木马（Trojan Horse）、间谍软件（Spyware）。

电子战具有三个主要组成部分和一个密切相关的领域：电子战支援（ES）、电子攻击（EA）、电子防护（EP）、诱饵。

### 2.9 带宽折中

带宽是所有通信网络中都需要折中考虑的重要参数。带宽越宽，信息传输速度越快，但提供足够信号保真度所需的接收信号功率就越大。接收误码率（BER）是检波前信噪比（RFSNR）的函数，RFSNR每降低1dB，BER将提高一个数量级。

### 2.10 纠错方法

常用的纠错方法：
  * 回传纠错，回传到发射端比较
  * 重复编码，重复发送多次，接收端比较
  * 前向纠错，使用检错纠错码（EDC）

有两类EDC码：
  * 卷积码，逐位纠错，描述为（n,k），表示为了保护k个信息比特，必须发送n个比特。适用于差错均匀分布。
  * 分组码，对整个字节进行纠错，描述为（n/k），表示为了保护k个信息符号，必须发送n个码符号（字节）。适用于差错成组出现。

Reed-Solomon(RS)码是广泛使用的分组码，能纠正的出错字节数等于附加字节数的一半（在所包括的数据字节数量之上）。如（31/15）RS码，每个分组发送31个字节，其中15个信息字节，16个纠错字节，它能对31个字节中最多8个出错字节进行纠错。

### 2.11 电磁频谱战实践

传统电子战是解决与动能威胁有关的电磁频谱问题。其中，雷达电子战的目的是使导弹无法截获或击中目标；通信电子战要阻止敌方进行有效的指挥控制。

现代战争中，电磁频谱的依赖与日俱增，电磁频谱本身已经变成敌方行动的目标。电磁频谱战的目的是拒止敌方安全、可靠地接入电磁频谱。

### 2.12 密写

密写（Steganography）也称隐写术，其与加密的区别，如同通信中的传输安全和信息安全，通过附加看似不相关的数据来掩盖秘密的信息，使敌方觉察不到我们正在进行的交流。

### 2.13 链路干扰

干扰的决定因素是干信比（J/S）。对数字信号而言，J/S为0dB就足够了，20%-33%的干扰占空比通常就足以阻断所有通信。J/S的计算公式：
$$
J/S=ERP_J-ERP_S-LOSS_J+LOSS_S+G_{RJ}-G_R
$$
式中：$ERP_J$为干扰机的有效辐射功率（dBm），$ERP_S$为希望得到的来自发射机的有效辐射功率（dBm），$LOSS_J$是干扰机到目标接收机的传播损耗（dB），$LOSS_S$是预期的信号发射机到目标接收机的传播损耗（dB），$G_{RJ}$是接收机天线在干扰机方向上的增益（dB），$G_R$是接收机天线在信号发射机方向上的增益（dB）。

对链路干扰的有效防护技术有：扩频调制、天线方向性、纠错码。

## 第3章 传统雷达

### 3.1 威胁参数

#### 3.1.1 典型的传统地空导弹

SA-2的典型参数：

|     参数     |  数值  |    参数     |  数值   |
| :---------: | :----: | :---------: | :-----: |
|   杀伤距离   |  45km  | 有效辐射功率 | 120dBm  |
|   最大高度   |  20km  |   旁瓣电平   |  -21dB  |
|   工作频率   | 3.5GHz |   脉冲宽度   |   1μs   |
|   发射功率   | 88dBm  | 脉冲重复频率 | 1400pps |
| 天线视轴增益 |  32dB  |   目标RCS   |   1m²   |
| 天线波束宽度 | 2°×10° |             |         |

其发射功率为600kW，转换为dB为87.8dBm。

非对称天线波束的增益计算公式：

$$
G=\frac{29000}{\theta_1 \times \theta_2}
$$
式中，G为视轴增益，$\theta_1 \ \theta_2$为两个正交方向上的3dB波束宽度。

公开文件给出的SA-2脉冲宽度（PW）为0.4\~1.2μs，取1μs作为典型值。

常规雷达的旁瓣电平为-13dB到-30dB，取中值-21dB。

#### 3.1.2 典型的传统截获雷达

前苏联P-12（Spoon Rest）雷达是典型的传统截获雷达，其主要参数：

|     参数     |  数值  |    参数     |  数值   |
| :---------: | :----: | :---------: | :----: |
|    作用距离   | 275km  | 有效辐射功率 | 112dBm |
|   最大高度   |  20km  |   旁瓣电平   | -21dB  |
|   工作频率   | 160MHz |   脉冲宽度   |  6μs   |
|   发射功率   | 83dBm  | 脉冲重复频率 | 360pps |
| 天线视轴增益 |  29dB  |   目标RCS   |  1m²   |
| 天线波束宽度 |   6°   |             |        |

对称天线波束的增益计算公式：

$$
G=\frac{29000}{BW^2}
$$
式中，G为视轴增益，BW为天线的3dB波束宽度。

#### 3.1.3 典型的高炮

前苏联“西尔卡（Schilka）”ZSU23-4自行高炮（AAA）是典型的传统传统高炮，其主要参数：

|     参数     |  数值  |    参数     |  数值  |
| :---------: | :---: | :---------: | :----: |
|   杀伤距离   | 25km  | 天线波束宽度 |  1.5°  |
|   最大高度   | 1.5km | 有效辐射功率 | 111dBm |
|   工作频率   | 15GHz |   旁瓣电平   | -21dB  |
|   发射功率   | 70dBm |   目标RCS   |  1m²   |
| 天线视轴增益 | 41dB  |             |        |

该雷达使用直径为1米的抛物面天线。抛物面天线的增益计算公式：

$$ G=-42.2+20log(D)+20log(F) $$

式中，G为天线视轴增益（dB），D为抛物面直径（m），F为工作频率（MHz）。

天线波束宽度计算公式：

$$20log\theta=86.8-20log(D)-20log(F) $$

式中，$\theta$为3dB波束宽度（度），D为抛物面直径（m），F为工作频率（MHz）。

在15GHz频率处，直径为1米的抛物面天线增益为41dB，波束宽度为1.5°。

### 3.2 电子战技术

讨论以下电子战技术：

* 探测、截获和辐射源定位
* 自卫干扰
* 远距离干扰
* 箔条和诱饵
* 反辐射导弹

每一类威胁的指标包括：

* 截获距离
* 干信比（J/S）
* 烧穿距离
* 诱饵模拟的RCS

### 3.3 雷达干扰

考虑两种位置相关的干扰技术：自卫干扰和远距离干扰。假设干扰功率不超过雷达的接收机带宽，且雷达采用同一天线发射和接收。

运用dB形式的公式需要注意：公式中的数值常数合并了换算系数；必须以特定的单位输入数值；输入值所用的单位都不同。

#### 3.3.1 干信比

雷达接收机接收的目标回波功率：

$$S=-103+ERP_R-40logR-20logF+10log\sigma+G$$

式中：$ERP_R$为雷达在目标方向的有效辐射功率（dBm），R为雷达至目标距离（km），F为雷达频率（MHz），$\sigma$为目标RCS（m²），G为雷达天线主波束视轴增益（dB）。

雷达接收到的干扰功率：

$$ J=-32+ERP_J-20logR_J-20logF+G_{RJ} $$

式中：$ERP_J$为干扰机在雷达方向的有效辐射功率（dBm），$R_J$为干扰机至雷达的距离（km），F为干扰机的发射频率（MHz），$G_{RJ}$为雷达天线在干扰机方向的增益（dB）。

#### 3.3.2 自卫干扰

自卫干扰的J/S公式：

$$ J/S=71+ERP_J-ERP_R+20logR-10log\sigma $$

式中：$ERP_J$为干扰机的有效辐射功率（dBm），$ERP_R$为雷达的有效辐射功率（dBm），R为雷达至目标距离（km），$\sigma$为目标RCS（m²）。

#### 3.3.3 远距离干扰

干扰机安装在专用电子战飞机上，干扰信号从雷达旁瓣进入雷达，J/S计算公式：

$$ J/S=71+ERP_J-ERP_R+40logR_T-20logR_J+G_S-G_M-10log\sigma $$

式中：$ERP_J$为干扰机的有效辐射功率（dBm），$ERP_R$为雷达的有效辐射功率（dBm），$R_T$为雷达至目标的距离（km），$R_J$为干扰机至雷达的距离（km），$G_S$为雷达旁瓣增益（dB），$G_M$为雷达主波束视轴增益（dB），$\sigma$为目标RCS（m²）。

#### 3.3.4 烧穿距离

J/S与雷达至目标的距离成正比，当J/S下降到雷达可以重新截获目标时，目标的距离定义为烧穿距离。根据上述公式可以计算出自卫干扰和远距离干扰的烧穿距离，烧穿J/S（雷达能重新截获目标时的J/S值）取决于干扰类型，通常取0dB是合适的。

### 3.4 雷达干扰技术

雷达干扰技术分为压制干扰和欺骗干扰。

#### 3.4.1 压制干扰

压制干扰包括阻塞干扰、瞄准式干扰、扫频瞄准式干扰。干扰效率为有效干扰ERP与总的干扰ERP之比，等同于雷达接收机带宽与干扰带宽之比。

#### 3.4.5 欺骗干扰

欺骗干扰要求干扰信号与目标信号到达时差小于微秒级，一般仅限于自卫干扰。包括距离欺骗、角度欺骗、频率欺骗。

距离欺骗包括距离波门拖离（RGPO）、距离波门拖近（RGPI）和覆盖脉冲。其中，RGPI干扰机跟踪雷达脉冲重复时间，用于对抗仅用脉冲前沿的能量实现距离跟踪的雷达。对恒定或参差PRF的雷达效果良好，对随机PRF的雷达无法跟踪。覆盖脉冲干扰机使用一个脉冲序列跟踪器，输出一个对准雷达回波脉冲的长脉冲，从而抑制雷达距离信息。

将干扰信号置于速度波门内，然后逐渐远离目标回波频率，形成速度波门拖离。有时需要速度波门拖离与距离波门拖离协同干扰。

对于圆锥扫描雷达，在目标信号最弱的时段发射一串与雷达同步的强脉冲，使雷达扫描轴偏离目标，形成角度欺骗，称为逆增益干扰。

如果雷达照射器不扫描，接收天线扫描，则干扰机无法获取雷达信号的强弱，如果干扰脉冲串比雷达脉冲串稍快或稍慢，仍可形成角度欺骗。

对边扫描边跟踪雷达（TWS），干扰机产生一串同步脉冲，可以使角度跟踪波门位置偏离目标。

AGC电路具有快速启动、慢速衰减的特性，如果在目标回波上施加一个很强的窄带干扰信号，则高电平干扰脉冲将截获AGC，导致目标回波大幅下降，不能在角度上正常跟踪。

#### 3.4.9 干扰单脉冲雷达

单脉冲雷达采用多个传感器来实现二维角度信息跟踪，传感器的输出在和、差通道中进行合并，和通道确立回波信号的电平，差通道提供角度跟踪信息。差响应在和响应的3dB带宽内通常是线性的。对抗单脉冲雷达的方法包括：编队干扰、距离抑制编队干扰、闪烁、地形弹射、交叉极化、交叉眼等。

编队干扰就是保持两架飞机在雷达的分辨单元内编队飞行。雷达分辨单元的横向尺寸和纵向尺寸分别为：

$$
W=2Rsin(BW/2) \\
D=c(PW/2)
$$
式中：W为分辨单元横向宽度（m），R为目标距离（m）BW为雷达天线3dB波束宽度，D为分辨单元纵向宽度（m），PW为雷达脉冲宽度（s），c为光速（3×10^8m/s）。

自卫干扰会强化单脉冲雷达的角度跟踪，但会抑制距离信息。距离抑制编队干扰是两架飞机以大致相同的功率实施干扰，可以抑制距离上分辨目标。飞机编队必须保持在雷达分辨单元的横向宽度内。

闪烁是位于雷达分辨单元内的两架飞机以适中的速率（0.5\~10Hz）交替干扰。由于导弹角度制导受环路带宽的限制，无法跟上目标的变化，将会飞偏到一边。

地形弹射是低高度飞行的目标以很大增益转发雷达信号，利用地面或水面的镜像信号使雷达跟踪到目标下方。

康登波瓣与主波束呈交叉极化，抛物面天线的曲率越大，康登波瓣越大。如果雷达受到与主波束呈交叉极化的强干扰信号照射时，康登波瓣有可能超过主波瓣。交叉极化利用抛物面天线的康登波瓣，对水平极化信号用垂直极化转发，垂直极化信号用水平极化转，使雷达捕获康登波瓣目标信号。这种干扰不适用于相控阵天线和采用极化滤波器的雷达天线。

干扰机上间隔足够大的两个收发天线A、B，两个路径保持180°±1的相位关系。交叉眼干扰是A点天线接收的信号放大20-40dB由B点转发，B点接收的信号经过放大和180°移相后由A点转发。使雷达接收到相位相差180°的两个信号，这会在雷达传感器中生成一个零点，是雷达角度跟踪偏离目标。

## 第4章 下一代威胁雷达

### 4.1 威胁雷达升级

电子战面对的威胁发生重大变化，电子战战术在以下几个方面发生变化：

* 导弹作用距离大幅提高，极大降低了防区外干扰的效果；
* 自卫干扰受到干扰寻的武器的影响；
* 诱饵和其他平台外设施的作用越来越大；
* 电子支援受到低截获概率雷达的影响。

### 4.2 雷达电子防护技术

基本的雷达防护技术及其针对的电子攻击技术包括：

* 超低旁瓣，探测和旁瓣干扰
* 旁瓣对消，旁瓣噪声干扰
* 旁瓣消隐，旁瓣脉冲干扰
* 抗交叉极化，交叉极化干扰
* 脉冲压缩，诱饵和非相参干扰
* 单脉冲雷达，多种欺骗干扰技术
* 脉冲多普勒雷达，箔条和非相参干扰
* 前沿跟踪，距离波门拖离
* 宽限窄电路，AGC干扰
* 烧穿模式，各种干扰
* 频率捷变，各种干扰
* 重频抖动，距离波门拖近和覆盖脉冲
* 干扰寻的，各种干扰

#### 4.2.2 超低旁瓣

视轴和旁瓣增益用dBi表示，相对旁瓣电平用dB表示

关于超低旁瓣，一系列合理的值：

  * 正常旁瓣电平比最大主波束（即视轴）增益低13~30dB，平均旁瓣增益最大为0~-5dBi；
  * 低旁瓣电平比视轴增益低30~40dB，平均旁瓣增益最大为-5~-20dBi；
  * 超低旁瓣电平比视轴增益低40dB以上，平均旁瓣增益最大为-20dBi以上。

对于雷达的旁瓣截获和旁瓣干扰，旁瓣增益下降10dB，导致作用距离下降为原来的1/sqrt(10)（即1/3.16）;20dB

的旁瓣隔离将使作用距离下降为原来的1/10。

#### 4.2.4 旁瓣对消

旁瓣对消（SLC），又称相干旁瓣对消（CSLC），采用辅助天线接收主瓣附近旁瓣的信号。辅助天线的增益大于旁瓣的增益，当辅助通道的信号大于主通道的信号时，可以判断来自旁瓣的干扰信号。利用辅助通道信号移相180°加到主通道上，来对消主通道的干扰干扰信号。相位控制需要高质量的锁相环，要求环路带宽窄，响应慢，只能处理连续噪声干扰。

每个干扰信号的对消都需要一个独立的天线和移相电路。

脉冲信号有很多不同的谱线，频率响应的主波束宽度为1/PW，其中PW为脉冲宽度；谱线间隔等于脉冲重复频率PRF=1/PRI，其中PRI为脉冲重复间隔。因此，从旁瓣进入的单脉冲信号可以饱和旁瓣对消电路，使其对噪声干扰不起作用。这就是在旁瓣干扰噪声上叠加脉冲信号的原因。

####  4.2.5 旁瓣消隐

旁瓣消隐（SLB），与旁瓣对消类似，采用辅助天线覆盖主要旁瓣的角度区域，不同的是旁瓣消隐主要对抗旁瓣脉冲干扰，在干扰脉冲期间关闭主通道信号。可以应用在任何类型的脉冲信号接收机中。

旁瓣消隐带来的问题是，雷达在其旁瓣出现脉冲干扰信号期间无法接收自身的回波信号。因此，干扰机可利用覆盖脉冲致使雷达失效，但要保证覆盖脉冲足够长，以叠加在雷达回波之上。

#### 4.2.12 AGC干扰

宽限窄电路包括一个宽带通道、一个限幅器，后接一个带宽与雷达脉冲匹配的窄带通道。窄脉冲干扰信号在宽带通道中被限幅，雷达AGC功能在窄带通道内完成，不会被窄脉冲干扰，因此可以对抗窄脉冲AGC干扰和高斯白噪声冲击脉冲。

理想的干扰噪声是高斯白噪声，利用高斯噪声信号在比雷达接收机带宽大得多的频带上对连续波信号进行调频，干扰信号每次通过雷达接收机的带宽，都会产生一个冲击脉冲，在雷达接收机中形成高斯白噪声。

#### 4.2.14 脉冲多普勒雷达

脉冲多普勒（PD）雷达的固有电子防护特性包括：

* 预计其回波在窄频率范围内，能剔除非相参干扰；

* 能发现干扰机的寄生输出；

* 能发现箔条的频率扩展（箔条云的闪烁引起）；

* 能发现分离的目标；

* 能够将距离变化率和多普勒频移关联。

PD雷达的处理器能形成一个距离-速度矩阵。速度单元由信道化滤波器组或快速傅里叶处理提供。速度通道的带宽为每个滤波器的带宽。滤波器带宽的倒数为相干处理间隔（CPI），这是雷达处理该信号的时间。雷达积累的脉冲数决定了其处理增益（在噪声电平之上）（dB）：

10log(CPI×PRF) 或 10log(PRF/滤波器带宽)

雷达的最大非模糊距离为：

$$ R_U=(PRI/2) \times c $$

雷达信号最大的多普勒频移为：

$$\Delta F = (\nu_R/c)\times2F$$

其中，$\Delta F $为多普勒频移（kHz），$\nu_R$为距离变化率（m/s），$F$为雷达工作频率（kHz）。

如果最大目标距离所对应的时间大于PRI，则产生距离模糊；如果最大目标速度对应的多普勒频移大于PRF，则产生频率模糊。

## 第5章 数字通信

### 5.2 传输比特流

数字通信的数据帧结构包括：

* 同步比特

* 地址比特

* 信息比特

* 奇偶校验或检错纠错比特

其中同步和地址比特约占总比特数的10%，奇偶校验或检错纠错比特约占信息比特的不到10%。

同步包括比特同步和帧同步。同步比特不一定位于帧头，也可伪随机地分布于整个帧中。

数字信号频谱主瓣的3dB带宽就足以支持数字信号在传输后的恢复。对于大多数的射频调制方式，3dB带宽约为0.88×传输比特率，而对于最小频移键控（MSK），3dB带宽只有0.66×传输比特率。

### 5.4 数字信号调制

一个传输波特携带一个比特的调制有脉冲幅度调制（PAM）、频移键控（FSK）、开关键控（OOK）、二元相移键控（BPSK）。一个传输波特携带多个比特的调制有正交相移键控（QPSK）、m元相移键控，如m=16，每个波特可以传输4个比特。

I&Q调制（指同相（In-Phase）和正交（Quadrature））描述了一类调制方式，每个传输波特的信号矢量可以用矢量终点在I&Q空间中的一个位置来表征，传输信号的状态由载波的幅度和相位确定。

### 5.5 数字链路规范

链路余量是指收到的信号功率中超出接收机灵敏度的部分。

$$ M=P_R - S $$

式中，M为链路余量（dB），P_R是接收机输入端的信号功率（dBm），S是位于接收天线输出端的接收系统灵敏度（dBm），包含了从天线到接收系统的线缆损耗。

$$ P_R = ERP - L + G_R $$

式中，ERP是发射天线输出的等效辐射功率，包含了有发射天线指向误差引起的增益下降，以及天线罩损耗；L是发射天线到接收天线的传输损耗，包含视距或双径传输损耗、绕射损耗、大气损耗和雨衰；G_R是接收天线增益，包含天线罩损耗和由天线指向误差引起的增益下降。

接收系统的灵敏度为：

$$S(dBm)=kTB(dBm)+NF(dB)+RFSNR(dB)$$

式中：kTB是接收机输入端内部噪声，在大气层内，通常表示为-114dBm+10log(带宽/1MHz)，这里假设接收机环境温度为290K。NF为系统的噪声系数，是接收系统引入的噪声导致接收机输出端的噪声高出kTB的量。RFSNR是检波前SNR。

灵敏度也可以用最小可分辨信号（MDS）来表示，此时RFSNR取0dB，即接收机输入端信号与噪声相等。

由于kTB与接收机的有效带宽有关，所以，由接收机的有效带宽及其噪声系数，即可确定接收机的灵敏度。

在数字链路中，RFSNR通过$E_b/N_0$与误码率联系起来。对特定调制方式，误码率由$E_b/N_0$决定。$E_b/N_0$是单个比特的能量与噪声功率密度（噪声等效带宽内每赫兹的噪声功率）之比。

$$E_b=S/R_b$$

式中，S是接收到的信号功率，$R_b$是比特率，这里指数据比特，不包括同步比特和校验比特。

$$N_0=N/B$$

式中，N是接收机中的噪声（即kTB+噪声系数），B是噪声等效带宽，近似等于符合速率。

$$E_b/N_0 = SB/NR_b$$

写成dB形式为：

$$E_b/N_0(dB) = RFSNR(dB)+(B/R_b)(dB) $$

最大通信距离就是在这个距离上，接收机收到的信号功率刚好等于灵敏度加上额定的工作余量。上式对传输损耗展开：

$$ P_R = ERP -32 - 20log(d)- 20log(F) + G_R $$

式中，d为链路距离，F为工作频率（MHz），令上式等于灵敏度加必要的链路余量，即可得最大通信距离：

$$  20log(d) = ERP -32 - 20log(F) + G_R - S - M $$

为了计算天线对准损耗，理想抛物面天线的3dB波束宽度为：

$$\alpha = 70 \lambda / D$$

式中，$\alpha$是3dB波束宽度（度），$ \lambda $ 是波长（m），D是天线直径（m）。频率表示式为：

$$\alpha = 21000 / (FD)$$

式中，F为工作频率（MHz）。

增益下降量与误差角和3dB波束宽度的关系为：

$$\Delta G = 12 (\theta / \alpha)^2$$

式中，$\Delta G $表示天线指向误差导致的增益下降量（dB），$\theta $表示天线指向误差（度）。写成频率、天线直径和指向误差之间的关系为：

$$\Delta G = -0.565 +20log(F) +20log(D) +\theta ^2$$

### 5.10 伪随机码

伪随机码有广泛的应用，包括：

* 密码

* 跳频序列

* Chirp信号的伪随机同步

* 直接序列码片生成

这些码是具有以下特性的最长二进制序列：

* 不重复比特数为$2^n-1$，n是生成码所用的移位寄存器的阶数
* 处于同步状态时，匹配比特的个数就是嘛中包含的比特数
* 处于不同步状态时，匹配比特的个数减去不匹配比特的个数等于-1

## 第6章 传统的通信威胁

### 6.3 单向链路

雷达和通信EW最大的不同就是，雷达统筹使用双向链路，通信为单向链路。

单向链路用dB表示的方程为：

$$ P_R = P_T + G_T - L + G_R $$

式中：$ P_R$是接收信号功率（dBm）,$ P_T $是发射机输出功率（dBm），$ G_T$是发射天线增益（dB），L是综合了所有因素的链路损耗（dB，取正数），$G_R$是接收天线增益（dB）。

用dBm表示天线的等效辐射功率并不准确，信号在天线口实际表现为功率密度，用毫伏/米表示。但如果在天线位置放置一个理想的各向同性天线（忽略近场效应），该天线的输出就是以dBm表示的信号强度，后面的讨论均是虚构一个理想天线。以dBm表示的信号强度和用毫伏/米表示功率密度之间的换算公式为：

$$P=-77+20log(E)-20log(F)$$

式中，P是到达天线的信号强度（dBm），E是到达天线的场密度（mV/m），F是频率（MHz）。

### 6.4 传输损耗模型

链路损耗指的是两个单位增益天线之间的链路损耗，与发射、接收天线的增益无关。

广泛使用的传播模型包括：适应于户外传播的Okumura和Hata模型，适用于室内传播的Saleh和SIR-CIM模型，还有描述由多径引起的短期波动的小比例衰落模型。在EW中，通常使用三种重要的近似模型：视距模型、双径模型、峰刃绕射模型。三种模型的适用条件：

* 通视传播路径
  * 低频、宽波束、近地面
    * 链路比菲涅尔区距离长——使用双径模型
    * 链路比菲涅尔区距离短——使用视距模型
  * 高频、窄波束、远离地面——使用视距模型
* 有地形遮挡的传播路径，计算峰刃绕射形成的附加损耗——使用视距模型

#### 6.4.1 视距模型

视距传播（LOS）损耗也叫自由空间损耗或扩散损耗，用于描述空间发射机和接收机之间的损耗，要求环境中没有明显的发射体，且传播路径与地面的距离远远大于信号的波长。

将收发天线等效为理想的全向天线，信号以球面波传播，球面的面积为$4\pi R^2$，全向接收天线的等效面积为$\lambda^2/4\pi$，则空间损耗为两者之比：

$$Loss=(4\pi)^2R^2/\lambda^2$$

式中，R为收发天线之间的距离，$\lambda$为信号的波长，两者采用相同的单位。

如果把损耗作为增益项，只需把上式取倒数即可。如果用频率计算，则上式变为：

$$Loss=(4\pi)^2R^2F^2/c^2$$

式中，F取Hz，c取m/s。如果距离取km，F取MHz，用dB表示的公式为：

$$L(dB)=32.44+20logR+20logF$$

#### 6.4.2 双径模型

当收发天线附近有一个巨大的反射面，且天线方向图宽到可以大面积地照射该反射面时，就要使用双径传播模型。

双径传播模型又称为40log(d)衰减或$d^4$衰减，双径损耗主要来自与直达波与地面/水面的反射波的相位对消，衰减量取决于链路距离和收发天线的高度，以非对数形式表示的双径损耗为：

$$L=d^4/(h_T^2 \times h_R^2)$$

式中，链路距离和天线高度使用一致的单位。dB形式的双径损耗：

$$L(dB)=120+40log(d)-20log(h_T)-20log(h_R)$$

式中，d取km，h取m。

不同频率下，双径传播模型要求最小的天线高度，如果任何一个天线的高度大于最小天线高度，则使用最小天线高度代替真实高度。下列环境的最小天线高度依次降低：

* 海面

* 优质土层上的垂直极化

* 劣质土层上的垂直极化

* 优质土层上的水平极化

* 劣质土层上的水平极化

理论上，当天线很低时，至少要求半个波长的高度。

对于复杂反射环境，如沿山谷传播，视距传播模型比双径传播模型更精确。

#### 6.4.5 菲涅尔区

菲涅尔区距离是相位对消开始超过扩散损耗时的距离。如果收发距离小于菲涅尔距离，则使用视距传输损耗模型；如果大于菲涅尔区距离，则使用双径传播损耗模型。其计算公式：

$$FZ=4\pi h_T h_R/\lambda$$

在这个距离上，视距衰减和双径衰减相等。这个公式可以简化为：

$$ FZ=[ h_T \times h_R \times F ]/24000 $$

式中，FZ取km，天线高度取m，F取MHz。

#### 6.4.3 峰刃绕射模型

计算山脊上的非LOS传播损耗时，通常先假设峰刃不存在，计算出对应的LOS损耗，然后再加上峰刃绕射（KED）衰减。

假设H是峰刃到收发天线视线的距离，$d_1$是发射机到峰刃的距离，$d_2$是接收机到峰刃的距离。只有$d_2\ge d_1$才发生KED，否则，接收机处于盲区，只能依靠对流层散射类连通。即使LOS越过山脊，峰刃也会引起损耗，除非LOS高出山脊过个波长。

KED的计算采取分段近似。先计算一个中间值：

$$\nu=H\sqrt{\frac{2(d_1+d_2)}{\lambda d_1d_2}}$$

再分段计算KED增益，dB形式的KED损耗取其相反数：

$$
KED(dB)=
\begin{cases}
0 &\quad 1<\nu  \\
20log(0.5+0.62\nu) &\quad 0<\nu<1 \\
20log(0.5^{0.4-0.95\nu}) &\quad -1<\nu<0 \\
20log(0.4-\sqrt{0.1184-(0.1\nu+0.38)^2} &\quad -24<\nu<-1 \\
20log(0.225/\nu) &\quad \nu<-24 \\
\end{cases}
$$

### 6.5 对敌方通信信号的截获

战术通信环境的描述通常为每毫秒内有10%的可用射频信道处于活动状态。

对于接收机的扫描速度，理论上只要使信号在接收机带宽内的驻留时间达到带宽的倒数即可，但系统软件对信号的识别和处理所需的时间要长的多。

### 6.6 通信辐射源定位

#### 6.6.1 三角定位

三角定位的最优几何关系为：从辐射源的位置看到的两个测向系统（DF）之间的角度为90°。

三角定位也可以由单个移动的测向站实施，要进行精确定位，同样需要示向线夹角达到90°，所以定位时间取决于DF平台移动速度和DF平台与目标的距离。

#### 6.6.2 单站定位

有两种情形可以利用单站定位：

一种是地基系统对低于30MHz信号的定位。利用电离层的反射，测出信号到达的方位角和俯仰角，根据电离层反射点的高度计算出距离。难点是反射点电离层的精确描述。

另一种是机载系统对地面辐射源的定位。机载系统测得辐射源的方位角和俯仰角，根据自身的位置和高度，计算辐射源的距离。

#### 6.6.3 其他定位方法

使用两个相距较远的DF站接收目标信号，比较信号参数，从数学上推导出可能的辐射源位置形成一条轨迹，通过增加第三个DF站，可计算出三条轨迹，其交点就是辐射源的位置。

#### 6.6.4 测量误差

DOA测量系统的误差通常用均方根（RMS）误差来表示。此时，假设误差是由随机变化的因素引起的。RMS误差可以分解为两部分：

$$(RMS误差)^2=(标准差)^2+(平均误差)^2$$

如果平均误差可以通过数学手段去掉，则RMS误差就等于标准差。如果误差服从正态分布，则标准差为34%，即在示向线的RMS范围内，68%的概率包含DOA的测量值。

如果两个测向系统对目标具有理想的几何关系，则定位精度可以用圆概率误差（CEP）表示，常用的有50%CEP和90%CEP。

如果两个测向系统对目标不具有理想的几何关系，则定位精度用椭圆概率误差（EEP）表示。有EEP计算CEP的公式：

$$CEP=0.75\times \sqrt{a^2+b^2}$$

式中，a、b分别是EEP的长短半轴。

对辐射源的定位误差包含测量误差、站址误差和参考方向误差。

对辐射源定位的线性误差与角度误差和距离有关：

$$线性误差=tan(角度误差)\times 距离$$

假设两个测向站为理想几何关系，角误差形成边长为$2\Delta$正方形区域。并假设系统误差从RMS中去掉，则由示向线和标准差（$1\sigma$）形成的区域34.13%包含信号DOA，$2\Delta$正方形区域46.6%包含辐射源位置，则50%CEP的半径为：

$$CEP=\sqrt{4\Delta^2 \times 1.074/\pi}$$

#### 6.6.9 中等精度的辐射源定位方法

对通信辐射源定位的两种中等精度方法是沃特森-瓦特方法和多普勒方法。中等精度对应的RMS典型值为2.5°。

##### 6.6.9.1 沃特森-瓦特测向方法

沃特森-瓦特测向系统中，三个接收机与一个圆形天线阵列相连，圆形天线阵列由偶数个天线和位于中心的参考天线组成。圆形阵列的直径约为1/4个波长。

外围的两个天线接到两个接收机，中心参考天线接到第三个接收机。处理过程中，将两个外部天线接收到的信号幅度差与中心参考天线接收到的信号幅度相除。这组信号在三个天线周围形成一个心形的方向图。

通过矩阵开关，把另一对天线接到两个接收机，得到另一个心形方向图。顺序切换天线对，就可以计算出信号的DOA。

这种方法适用于所有信号调制类型，在未校准的情况下，可以达到2.5°的RMS精度。

##### 6.6.9.2 多普勒测向方法

如果一个天线围绕另一个天线旋转，运动天线A收到的信号频率相对于固定天线B收到的信号频率呈正弦变化，辐射源的方向就是正弦波由负到正经过零点的方向。

实际中，多个天线布成圆形阵列并顺序接入接收机A，阵列中心的参考天线接入接收机B。每次切换，两个接收机进行相位比较，旋转几圈后，就可以测出信号的DOA。

这种方法只需3个外部天线加一个中心参考天线就可达到2.5°的RMS精度，但难以适应频率调制信号。

#### 6.6.13 高精度的方法

高精度的辐射源定位方法通常都是指干涉仪测向。干涉仪经过校准可以提供1°量级的RMS误差。

##### 6.6.13.1 单基线干涉仪

两个天线收到的信号进行相位比较，根据相位差即可确定信号的DOA。

波前到达两个天线的距离差为：

$$\Delta d=\Delta \phi \lambda /360$$

式中，$\Delta \phi$为相位差。则信号到达的角度为：

$$A=arcsin(\Delta d / D)$$

式中，D为两个接收天线的距离，即基线的长度。

单基线干涉仪提供180°的角度覆盖，其测角精度和角度模糊由基线长度决定，基线越长，测角精度越高，但存在测角模糊，当基线小于半波长时，不存在角度模糊。

##### 6.6.13.2 多基线干涉仪

多个基线的长度远大于半波长，同时使用多个基线测得的相位进行模运算，可以确定DOA，并解模糊。

##### 6.6.13.3 相关干涉仪

相关干涉仪使用较多的天线（通常5~9个），天线间距大于半波长（通常1~2个波长），对大量DOA数据进行相关处理，确定真实值。

#### 6.6.17 精确的辐射源定位方法

精确的辐射源定位方法提供的精度足以支持目标瞄准（几十米内）。常用的方法有时差（TDOA）方法和频差（FDOA）方法，都依赖于高精度的参考振荡器。

##### 6.6.17.1 TDOA

两个接收站对接收信号进行延迟相关处理，测得信号到达时间差，从而得到辐射源到两个测量站的距离差。一个固定的距离差在空间形成了一个双曲面，与地面相交形成双曲线，叫等时线。

使用第三个接收站，形成三条双曲线，其交点为辐射源的精确位置。

##### 6.6.17.2 FDOA

FDOA方法可以在运动平台上对地面固定辐射源进行定位。运动的接收机导致的多普勒频移为：

$$\Delta F= F\times V\times cos(\theta)/c$$

式中，V是接收机运动速度，$\theta$是接收机速度矢量和信号的DOA之间的真实球面角。

两个接收机接收到的信号频率差形成一个空间曲面，与地面相交形成一个频率差曲线。通过引入第三个运动的接收机即可定位辐射源。

##### 6.6.17.3 TDOA和FDOA相结合

一个平台上可同时按照TDOA和FDOA接收机，三个接收平台，形成三条基线，定义了六条经过辐射源的曲线（三条等时线和三条等频线），其定位精度要优于单独使用的精度。

### 6.7 通信干扰

接收机收到的干扰信号功率与有用信号功率之比称为干信比（J/S），用dB表示。
$$J/S=ERP_J-ERP_S-L_J+L_S+G_{RJ}-G_R$$

由于通信收发及通常采用鞭状天线，接收天线在有用信号发射机方向和干扰机方向上的增益一样，所以上式可简化为：

$$J/S=ERP_J-ERP_S-L_J+L_S$$

## 第7章 现代通信威胁

### 7.2 低截获概率通信信号

低截获概率（LPI）通信信号通常使用扩频实现，常用的扩频技术有：

* 跳频

* 线性调频

* 直接序列扩频

扩频解调必须与扩频调制同步。同步过程要求使用相同的伪随机函数对调制器和解调器进行控制，该函数基于数字码序列。

LPI信号必须是数字信号，对数字信号的干扰只需0dB的干信比，需要的干扰占空比可能远小于100%。

对数字接收机，无论干信比多少，误码率永远不会超过50%。在干信比为0dB时，误码率接近50%；在0dB干信比基础上继续提升干扰功率，误码率增加极少。一般认为，在超过毫秒量级时间段内的误码率超过33%（或20%），就无法对信息进行还原。

### 7.3 跳频信号

信号在一个频点的驻留时间称为跳周期。跳频速率是指一秒内跳变的次数。跳频范围是指可供选择的传输频率带宽。

跳频系统分为慢速跳频和快速跳频。慢速跳频在每个跳周期传输多个比特；快速跳频在每个数据比特期间多次改变频率。

慢速跳频使用锁相环频率合成器，需要对其反馈回路的带宽进行设计。频带越宽，合成器跳变速度越快；频带越窄，信号的质量越好。典型的频率合成器完成频率变换的时间近似为其跳周期的15%。在这个中断时间内系统是不可用的。

快速跳频使用直接频率合成器。信号在接收机内的驻留时间与接收机需要的带宽成反比，通常，驻留时间是带宽的倒数。同步接收机能够去除跳变，它的工作带宽可以是携载信息的信号带宽，而敌方接收机不能去除跳变，因此它的工作带宽必须更宽，其灵敏度大幅下降，使其很难探测到信号的存在。

跳频系统的抗干扰优势是跳频范围和信息带宽两者的比值。在相同的干信比条件下，跳频接收机收到的干扰信号要除以这个比值。

对跳频系统的干扰通常有三种：阻塞干扰、部分带宽干扰和跟踪干扰。

#### 7.3.5 阻塞干扰

阻塞干扰机覆盖目标系统的整个跳频范围。其优点是无需侦察跳频信号。其缺点，一是无差别干扰会对友方通信造成干扰；二是效率低下，每个通道的干扰功率=干扰总功率/所有可用的跳频通道数目。

#### 7.3.6 部分带宽干扰

部分带宽干扰仅覆盖跳频范围的部分频率。扫频干扰属于部分带宽干扰的特殊应用。确定频率覆盖范围的步骤：

  1. 确定整体的干信比，即没有跳变，在一个通道上的干信比；

  2. 将干信比（以dB表示）转换成线性形式，如30dB换成1000；

  3. 确定干扰频率覆盖范围，即对每个通道产生0dB的干信比和33%的干扰占空比。

例如：一个跳频系统的每个跳频通道带宽为25kHz，跳频范围为58MHz，如果整体的干信比为29dB，则覆盖794个通道，可使每个通道的干信比为0dB，干扰占空比为794/2320=34.2%。

 部分带宽干扰的使用要点：

  1. 0dB的干信比和33%的占空比能够产生有效干扰，这是对干扰机最高效的使用；
  2. 传输过程中的每一秒都必须达到要求的占空比；
  3. 干扰覆盖带宽必须在跳频范围内不断移动；
  4. 如果目标系统使用纠错码，需要提高干扰占空比。

#### 7.3.8 跟踪式干扰

跟踪式干扰能够在远小于跳周期的时间内确定跳频信号的频率，然后将干扰机设定在该频率，对剩下的跳周期进行干扰。如果干扰时间不小于跳周期的1/3，则干扰有效。

干扰机要分析所有信号的频率和位置，从中找出需要干扰的频率，同时要考虑传输延迟的影响。

宽带数字接收机使用FFT进行信号快速分析。FFT对中频数据进行信道化处理，信道数量等于样本数量的一半。I&Q是独立采样的，信道数量等于I&Q样本数量。

### 7.4 线性调频信号

线性调频的应用有两种方式：一是对数字信号进行线性扫描，扫频带宽比信息带宽大得多；二是对数字信号的每个比特进行线性调制。两种方式的处理增益都等于扫频范围和信号信息带宽的比值。

宽带线性扫描的扫描起点时间是随机的，从而阻止敌方接收机与之同步。

对每个比特进行线性调频，可以使用扫频振荡器或声表面波（SAW）调频发生器实现。这种技术可以使用两种方式携载数据：

* 并行二进制通道。逻辑1代表一个调频方向，逻辑0代表相反的调频方向。这种信号可以通过频谱分析仪确定线性调频的斜率和结束点，使用线性调频信号进行干扰，干扰信号斜率可以随机为正或负。
* 脉冲位置多样化单通道。在一个线性调频发生器上，逻辑1从某个频率开始，逻辑0从另一个频率开始。

### 7.5 直序扩频信号

直序扩频（DSSS）信号是一种使用二次数字调制进行频率展宽的数字信号。典型的主瓣带宽等于调制比特率的两倍。通常，调制码速率是信息比特率的100至1000倍，称为扩频因子，可以在相关接收机获得相应的处理增益。

对DSSS信号的干扰，可以使用DSSS发射器中频附近的简单连续波压制干扰；也可以说使用脉冲干扰，但如果目标系统使用交织纠错码，干扰就不起作用了。

### 7.7 对己方的误伤

使误伤最小化的途径包括：

* 干扰机到目标接收机的距离最小化，到友方接收机的距离最大化；
* 对目标使用的频率进行干扰，避开友方重要的指挥频率；
* 使用定向天线进行干扰，或使用友方通信的交叉极化；注，鞭状天线是垂直极化
* 使用LPI技术对友方通信进行保护，提供处理增益；
* 使用干扰对消技术降低干扰影响。

### 7.8 对LPI发射机的精确定位

对跳频信号，TDOA方法通过延迟确定相关峰需要耗时接近1秒，不能满足；FDOA只需对频率进行一次测量，具有可操作性。

对线性调频信号，TDOA和FDOA都不切实际。

对DSSS信号，由于扩频码未知，探测通道无法提供足够的信噪比来支撑信号定位。

## 第8章 数字射频存储器

### 8.1 DRFM的结构

DRFM的关键部件是ADC，ADC的采样率达到采样带宽的2.5倍，过采样是为信号的重建。ADC输出I&Q数字信号，这样可以获得信号的相位信息。DAC的位数要多于ADC的位数，从而保证射频信号重构过程中信号的质量。上下变频使用相同的本振以保持信号相位的相干性。

DRFM可分为宽带DRFM和窄带DRFM。

对宽带DRFM而言，ADC采样速度和位数之间是相互制约的关系。DRFM的带宽受ADC采样速率的限制，动态范围由ADC采样位数决定。克服两者矛盾可以采取的方法：

* 使用几个不同电压电平的单比特数字转换器；
* 在抽头延迟线的输出端放置几个多比特数字转换器。

对窄带DRFM，干扰频率范围被分成多个频段，每个频段由一个窄带DRFM覆盖，每个窄带DRFM调谐到一个独立的信号上。

### 8.5 相干干扰

DRFM的优势之一就是能够产生相干干扰信号。

对PD雷达而言，假设其相干处理间隔（CPI）与照射目标的时间相同，其处理增益等于CPI与PRF的积，即相干脉冲积累个数。

PD雷达单个多普勒滤波器的带宽很窄，可以等于相干处理间隔的倒数。只有相干干扰信号才能获得处理增益。

### 8.7 非相干干扰

频率捷变雷达的跳频范围可达到标称频率的10%。对抗频率捷变雷达的非相干干扰方法有两种：

一是将干扰功率分配到所有观测到的频点上。如果跳频范围内有n个频点，则J/S将下降10lg(n)。

二是将干扰功率分布在整个跳频带宽内。雷达的相干处理带宽为脉宽的倒数，通常非相干干扰要比雷达接收机带宽要宽。假设雷达的工作频率为5GHz，脉宽为1μs，相干带宽为1MHz，干扰带宽为5MHz，则J/S下降10lg(500/5)。

如果使用脉间跟随干扰，则可以获得全部的J/S，只损失DRFM的处理时间（含信号传输时间）。

### 8.9 雷达分辨单元

分辨单元内地额多个目标存在以下可能：

* 多个真实目标
* 一个真实目标和一个诱饵
* 一个真实目标和一个有干扰机生成的假目标

这种情况下雷达很难对真实目标进行跟踪。解决方法是使用脉冲压缩处理减小雷达的纵向分辨单元。

线性频率调制，也称Chirp调制，也可以是非线性频率调制。压缩比=调频带宽/雷达相干带宽。

二元相位调制，使用Barker码，或其他码，都属于最大长度线性移位寄存器序列码，是伪随机的，数字1和0两者数目的差值为0或-1。压缩比=码元长度。

### 8.10 复杂假目标

复杂目标的RCS包含很多散射点，这些散射点是由目标不同部位的外形造成的。每个散射点产生一个具有独特相位、幅度、多普勒频移和极化特性的回波。现代雷达，尤其是合成孔径雷达（SAR）和有源相控阵雷达（AESA）能够利用RCS对复杂目标进行特征描述。

目标RCS是所有散射点的综合，并且随视角变化而变化。此外还存在 其他目标特性，如喷气发动机调制（JEM）、旋转叶片调制（RBM）。

目标RCS能够通过RCS暗室进行测量或计算机分析得到。

RCS暗室测量可以使用比例模型，需要同比例提高雷达工作频率，以使目标尺寸和雷达信号波长的比例关系保持不变。

计算机分析首先对目标进行建模，然后根据各表面的形状和相对雷达的角度，计算各表面的RCS幅度和相位，还需要考虑各部分的材料以及表面特性，最后对所有表面进行综合。

使用DRFM产生复杂假目标的过程是：对收集到的数据进行处理，确定主要的散射点，对每个散射点的回波特征进行描述，包括相位、幅度、多普勒频移和位置（取决于视角）信息，将数据存储在数据库，由数据库驱动DRFM产生精确的、动态的目标回波。

### 8.13 DRFM的反应时间

对于相同的脉冲，对收到的第一个脉冲进行分析，通过恰当的调制产生干扰脉冲，对后续脉冲进行干扰。处理过程需要在脉间完成。

对相同的Chirp脉冲，对接收的数字化信号进行处理，测量出频率调制的斜率（将脉宽划分成时间增量，形成阶梯步进的方式描述）。处理过程需要在脉间完成。

对相同的相位编码脉冲，需要确定编码的时钟速率、编码中的0和1的顺序。处理过程需要在脉间完成。

对脉间变化的脉冲，对每个脉冲进行频率测量，确定脉冲的频率。处理过程在脉冲前沿的一小段时间内完成。

### 8.14 需要使用DRFM对抗的雷达技术

传统干扰机很难对抗这些雷达技术：

* 相参雷达
* 前沿跟踪
* 脉间跳频
* 脉冲压缩
* 距离变化率与多普勒频移相关
* 目标RCS分析

## 第9章 红外威胁与对抗

电子战中主要使用的红外频谱涉及三个部分：近红外（0.78\~3μm）、中红外（3\~50μm）、远红外（50\~1000μm）。

### 9.2 红外传播

第6章讨论的视距衰减，其计算公式源自光学。

射频在大气中的衰减随着频率的升高而增大，受大气成分影响存在两个峰值：一个在22GHz受水蒸气的影响；另一个在60GHz受氧气的影响。红外信号的大气衰减同样存在传输窗口。

红外传播四个主要的大气窗口：

* 近红外两个窗口：波长在1.5~1.8μm和2~2.5μm；
* 中红外：波长在3~5μm，其中有两个窗口；
* 远红外：波长在8~13μm。

### 9.6 红外传感器

喷气式飞机各部分大概的温度范围：

* 发动机压缩机叶片、尾喷管温度在1000~2000K，对应红外能量峰值1~2.5μm；
* 发动机尾焰温度在700~1000K，对应红外能量峰值3~5μm；
* 机体空气摩擦发热温度在300~500K，对应红外能量峰值8~13μm。

几种重要的传感器材料的峰值响应波长及其应用：

| 化学符号 | 材料   | 300K时 | 77K时 | 典型应用                                                     |
| -------- | ------ | ------ | ----- | ------------------------------------------------------------ |
| PbS      | 硫化铅 | 2.4    | 3.1   | 常温下对热目标进行跟踪                                       |
| PbSe     | 硒化铅 | 4.5    | 5     | 跟踪飞机尾焰                                                 |
| HgCdTe   | 碲镉汞 |        | 10    | 必须冷却至77K。用于导弹跟踪器和焦平面阵列。在中、长波窗口均能工作。 |
| InSb     | 锑化铟 | 3.5    | 3     | 跟踪飞机尾焰                                                 |

除硫化铅外所有的材料通过降温至77K（氮气在一个气压下的沸点）来提高灵敏度和信噪比，还能有效辨别来自太阳的能量。

双色跟踪器使用两个波长对目标进行跟踪，通过两个波长上的能力比率，可以计算出被跟踪目标的温度，区分出目标与曳光弹、太阳或其他高温干扰物。双色曳光弹可以在两个波长上产生正确的能量比率。

### 9.10 曳光弹

曳光弹的作用分为三种：

* 引诱，在热寻的导弹跟踪器可见的物理区域或波长范围内部署，将跟踪器注意力转向曳光弹；
* 迷惑，在热寻的导弹开始对目标跟踪前部署，使跟踪器首先看到曳光弹
* 冲淡，用来对抗具有成像功能或边跟边扫功能的导弹，使导弹在多个可信的目标中进行选择。

曳光弹的速度衰减取决于自身设计以及飞机速度，可达到300m/s^2，而导弹跟踪器的视野直径通常小于200m，要求曳光弹必须在足够短的时间内能量达到最大。

## 第10章 雷达诱饵

### 10.1 简介

雷达诱饵有三项基本任务：

* 饱和，生成很多假目标，饱和敌方的信息吞吐量；
* 诱骗，同被保护目标在雷达分辨单元内，诱使雷达跟踪诱饵；
* 探测，使雷达截获和跟踪诱饵，暴露雷达信号。

不同类型诱饵的应用：

| 诱饵类型   | 任务       | 要保护的平台 |
| ---------- | ---------- | ------------ |
| 投掷式     | 诱骗和饱和 | 飞机和舰船   |
| 拖曳式     | 诱骗和饱和 | 飞机         |
| 独立机动式 | 探测       | 飞机和舰船   |

无源诱饵通常使用角反射器产生RCS增强。圆边角反射器的雷达截面积公式为：

$$\sigma=15.59L^4/\lambda^2$$

有源诱饵通常使用直通转发器或主振放大器产生RCS增强。RCS用下式表示：

$$\sigma=\lambda^2 G/4\pi$$

式中：对于直通转发器，G为接收天线、发射天线和放大器增益之和；对于主振放大器，G等于发射天线有效辐射功率除以到达诱饵接收天线的雷达信号强度，由下式确定：

$$P_A=ERP_R-L_P$$

式中：$P_A$为到达诱饵接收天线的信号强度（dBm），$ERP_R$为雷达在诱饵方向的有效辐射功率（dBm），$L_P$为从雷达到诱饵的传输损耗（dB）。

写成dB形式：

$$\sigma=38.6-20log(F)+G(dB)$$

式中：$\sigma$为RCS（dBsm），F为雷达频率（MHz），G为诱饵综合增益（dB）。

## 第11章 电子支援与信号情报

### 11.2 SIGINT

信号情报（SIGINT）旨在从接收信号中提取重大军事信息，分为通信情报（COMINT）和电子情报（ELINT），分别通过截获和分析敌方通信和非通信信号生成情报。

电子支援（ES）分为通信ES和雷达ES。

COMINT是通过监听通信内容来确定敌方的能力和意图，通信ES是通过分析信号的特征来确定敌方的能力和意图。

ELINT确定敌方具备哪些能力，雷达ES则确定敌方正在使用的雷达类型以及辐射源的位置。

### 11.5 截获距离

雷达和数据链通常以视距模式传播，截获距离为：

$$20log(R_I)=ERP_T-32-20log(F)+G_R-S$$

式中：$R_I$为截获距离（km），$ERP_T$为目标辐射源在截获接收机方向上的有效辐射功率（dBm），$G_R$为接收天线在目标辐射源方向的增益，S为接收机系统的灵敏度（dBm）。

通信信号以视距模式或双径模式传播，双径模式下的截获距离：

$$40log(R_I)=ERP_T-120-20log(h_T)+G_R-S$$

式中：$h_T$为目标辐射源发射天线高度。

### 11.6 接收机

常用的ES或SIGINT接收机：

| 接收机类型      | 灵敏度 | 动态范围 | 带宽 | 信号数   | 特征与局限               |
| --------------- | ------ | -------- | ---- | -------- | ------------------------ |
| 晶体视频        | 低     | 高       | 宽   | 1        | 仅幅度                   |
| 瞬时测频（IFM） | 低     | 高       | 宽   | 1        | 仅频率                   |
| 超外差          | 高     | 高       | 窄   | 1        | 恢复调制                 |
| 信道化          | 高     | 高       | 宽   | 多个同时 | 恢复调制                 |
| 布拉格小盒      | 低     | 非常低   | 宽   | 多个同时 | 仅频率                   |
| 压缩            | 高     | 高       | 宽   | 多个同时 | 仅频率                   |
| 数字            | 高     | 高       | 灵活 | 多个同时 | 恢复调制、独特的分析能力 |

### 11.7 频率搜索问题

对频率的搜索，一个经验法则是检测信号的存在，因为信号必定会在接收机带宽内驻留一段时间，这个时间等于接收机有效带宽的倒数。

根据接收机带宽就可以确定搜索指定的频率范围（以合适的带内驻留时间）所需的时间。

## 更新记录

  * 2019-12-09：首次发布；