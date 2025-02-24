---
title: "路由命令"
date: "2010-06-19"
categories: 
  - "notes"
tags: 
  - "路由命令"
---

路由器的硬件组成： 1 CPU 2 ROM 保存了ROUTER的引导程序和一些版本较小、较旧的IOS。 3 FLASH 保存了IOS的映像文件(.BIN) 4 NVRAM 非易失性存储器。用于保存ROUTER当中的静态的配置文件。startup-config 5 RAM 随机存取存储器。用于保存路由表和正在运行的配置文件。running-config 路由器的启动过程：<!--more-->

ROM------------------------FLASH---------------NVRAM-------------------RAM 读取引导程序，自检。||开启并释放IOS文件||查看是否有配置文件||运行IOS，开始学习网络信息。

ROUTER的几种配置模式： 1 ROUTER>??? 用户模式。功能最少、权限最低的模式。只能对ROUTER进行简单的查看，并不能对ROUTER进行任何的修改、配置。是用户TELNET到ROUTER上后的缺省模式。 ENA 2 ROUTER#??? 特权模式。在特权模式下，可以对ROUTER进行全部的查看和简单的配置修改。CONF T 3 ROUTER(CONFIG)# 全局配置模式。在此模式下，可以对R进行最大化的配置修改，但不能查询R信息。 4 从高级模式返回到低级模式，可以使用命令“EXIT”或“END”或直接“CTRL+Z”。

R的简单配置(所有配置全部基于全局模式以后的情况下)： 1 修改R名称：HOSTNAME \*\*\*\*\*\* 2 设置一个EN密码：明文密码 ENABLE PASSWORD \*\*\*\*\*\* ??????????????????? 密文密码 ENABLE SECRET \*\*\*\*\*\* 两种配置出来的密码，不能是同一个密码。同时配置两个密码时，密文密码的级别比明文密码的级别高。

3 给CONSOLE口加密： (config)#line console 0??? 进入CON口的配置。 (config-line)#password \*\*\*\*\*\* 设置一个密码 (config-line)#login??? 激活密码

4 设置TELNET密码：(若R没有配置TELNET密码，则拒绝一切TELNET进程；R最多允许5个线程的登陆。)！！！ (config)#line vty 0 4 进入VTY线路的配置。 (config-line)#password \*\*\*\*\*\* 设置一个密码 (config-line)#login??? 激活密码

5 设置一个BANNER信息：给R设置一个欢迎界面信息。 (config)#banner motd # this is zhaoxin......#

6 给R设置IP地址： (config)#interface s0/s1/e0 (config-if)#ip address ip\_add ip\_mask (ocnfig-if)#no shutdown

7 保存R配置： #copy running-config startup-config

8 删除NVRAM中的信息： #erase nvram或者 #delete startup-config

9 重启R： #reload

10 关闭域中命令查找： #no domain-lookup

CDP协议??? CISCO发现协议。在任何的串行线路当中，网络设备之间会相互的发送CDP协议，用于学习和邻居设备之间的连接信息。包括邻居设备之间的类型、信息和平台等。当网络设备一开启，就会自动的想外部发送CDP信息。用于查询邻居信息，若该CDP信息没有得到任何邻居的反馈，则此设备会认为，该接口没有连接任何设备，接口会自动关闭。如果想关闭CDP协议，可以使用： (config)#no cdp run #show cdp neighbors 查看当前设备的邻居设备

在使用到串行线路的设备之间，需要为路由器设置时钟频率。时钟频率，用来规范路由器之间的传输速率。 DEC：数据通信设备。DTE：数据中断设备。 在串行线路当中，需要对DCE设备设置时钟频率。否则路由器之间无法相互通信。

使用命令： #show controllers serial 0/1??? 来查看DCE是在哪个接口中。 (config-if)#clock rate 64000??? 设置时钟频率. #show ip route???? 查看路由表 accessserver1>clear line tty 线程号后两位。

路由协议知识：

路由选择协议的划分： 一 静态路由选择协议：由网络管理员在路由器上手工添加路由信息以实现路由目的。适用于小型的网络中。

二 动态路由选择协议：根据网络结构或流量的变化，路由协议会自动调整路由信息以实现路由目的。 1 内部路由选择协议IGP。 使用AS来做区分。AS 自治系统，以一种路由方式或一种路由资源所组成的范围。 2 外部路由选择协议EGP。

要实现路由，路由器必须知道： 目的地址 源地址 所有可能的路由路径 最佳路由路径 管理路由路径

R必须知道未和其直接相连的目的地址 IP协议是被路由选择协议

可路由协议ROUTED PROTOCOL(被路由协议)：利用网络层完成通信的协议，允许数据包从主机以寻址方案转发到另一主机。例如IP、IPX、APPLETALK。 路由协议ROUTING PROTOCOL，本质是创建和维护路由表，可路由协议利用他实现路由功能。例如：RIP、IGRP、EIGRP、OSPF、BGP、IS-IS等。

在R下直接连接终端，这种网络叫做存根网络。在存根网络中，比较适合使用静态路由。在一些网络比较简单，或不经常变动拓扑的网络中，也比较适合使用静态路由。

静态路由的配置： (config)#ip route 目的网段 目的网段子网掩码 下一跳地址

静态路由是一条单方向的路径，必须在对段的R上配置一条相反的路径。

静态路由的优点：因为不使用任何算法，对设备的使用消耗资源比较低。配置简单、快捷、安全性高。是优先级别最高的路由选择协议。

静态路由的缺点：不能自动使用网络拓扑的变化，不能工作在网络环境较大的网络当中。适合使用在存根网络当中。

ip route 0.0.0.0 0.0.0.0 外出接口IP/接口名称(可以是S0、S1、E1这样的接口名称) 这样的路由条目叫做缺省路由。 以外出接口方式设计静态路由比下一跳方式设计的静态路由优先级别更高。

路由协议：用于路由器选择路径和管理路由表。一旦选择了一条路径后，路由器就利用路由选择协议，选择最优路径。

自治系统：使用相同的路由准则的网络的集合。 IGP：在一个自治系统内运行。 EGP：连接不同的自治系统。

路由的AD(Administrative Distance)：路由选择协议的管理距离。 管理距离说明各种路由选择协议的优先级别。 C直连???? 0 DEFAULT ROUTING缺省路由/外出接口式??? 0 STATIC??? 1 RIP 120 IGRP 100 OSPF 110 EIGRP 90 AD值越小，代表该协议越优先。

度量值(Metric):路由协议算法由度量得出度量值。根据度量值判定路由最优路径，来创建和维护路由表。Metric越小，越优先。

收敛时间(Convergence Time):路由器之间相互学习路由表的过程叫做收敛，当所有的路由器路由表一致时，叫做收敛完成，从开始收敛到收敛完成所花费的时间，叫做收敛时间。收敛时间也可以作为评价一个网络好坏的标准。收敛越快，网络性能越好。

路由协议的分类： 距离矢量型：定期将路由表复制给相邻的路由器并进行矢量堆加。 距离矢量型的路由选择协议采用定时更新。每经过一定的时间后，开始学习和发送路由信息。 发送路由表的时候是整张路由表COPY给邻居路由器，这会导致路由表越来越大。 会因为收敛过慢，导致路由表不稳定，产生路由回路。

距离矢量型路由选择协议的几个通用特性： 1 采用定时更新，周期性的发布路由更新信息。例如RIP为30S，IGRP为90S。 2 包含邻居信息。邻居通常意味着共享相同的某条数据链路的所有路由器。 3 使用广播的方式发布路由更新信息。 4 每次发布的路由表都是整张表。但大多数情况下，邻居路由器接收到更新信息后，只保留自己的路由表中不存在的部分。

消除路由回路的机制： 1 抑制定时器。给数据定义一个最大时间，若数据没有在定义时间内传出网络，则证明，网络产生回路，数据被丢弃。 2 创建一个最大跳数。给数据定义一个最大的跳数(Max Hop)，若数据没有在最大跳数内传出网络，则证明，网络产生回路，数据被丢弃。 3 水平分割。给数据定义一个方向。路由器不容许再次发送同一方向或接口所转发或发出的数据。

RIP(Routing Information Protocol) RIP的特点：是一种典型的距离矢量型的路由选择协议。采用定时更新，每30秒更新一次。采用跳数作为度量值，HOPS越少，路径越优先。最大支持15跳。

RIP的记时器： 1 更新记时器(UPDATE TIMER) 默认为30S,在CISCO路由器中,加入了一个随机变量,使得CISCO路由器中RIP的更新时间为从25.5秒到30之间的一个随机值. ?每30秒,RIP会发送/接收一次路由更新 2. 失效记时器(INVALID TIMER) 默认为180S. ?无论什么时候,当有一条新的路由被建立,则该记时器则被初始化为180S,当再次接受到这条路由更新的时候,记时器又会被初始化为180S.当180S内还没有接收到该条更新时,则这条路由的跳数将会被更改为16,即标记为”不可达”. 3 抑制定时器(HOLDDOWN TIMER) 默认为180秒.\* CISCO专用记时器. ?若一条路由更新的跳数大于路由表已记录的跳数，则将会导致路由器进入抑制时间，并将该路由标记为可能失效(possibly down). 4 刷新记时器(FLUSH TIMER) 默认为240秒.但CISCO的路由器中设置为60秒. ?若路由器240S以后都没有收到路由更新，则标记该路由为”不可达”。并删除该路由表。

RIP V1的特性： 1 路由更新发送使用UDP520端口。 2 使用跳数作为METRIC值，最大支持15跳，第16跳将被标记为不可达。 3 发送路由更新信息时,使用广播的方式发送. 注意：RIP V1不支持VLSM和CIDR。(因为RIP在做路由通告的时候，是使用广播地址通告，且不会通告子网内容)

RIP V2的特性: 1 支持VLSM、CIDR。支持不连续的子网。 2 采用多播路由更新，使用组播地址224.0.0.9 3 支持认证 使用国际标准TEXT，CISCO设备支持MD5加密。 RIP V1与V2的共同特性 发送数据时，使用UDP端口520 默认管理距离都是120

v1配置命令： (config)#router rip (config-router)#network 直连网段 #show ip protocol 显示路由选择协议更新的详细信息。 #debug ip rip

CISCO路由器默认情况下没有起用V2，要使用V2用下列命令： (config)#router rip (config-router)#version 2 (config-router)#network 直连网段号 #clear ip router \*??? 立即强制更新路由表

配置被动接口: (CONFIG-ROUTER)PASSIVE-INTERFACE INTERFACE\_NAME 如果一个接口被配置成被动接口,则本接口只接收路由更新,而不会发送路由更新.被动接口可以在所有的路由选择协议中使用.

启用单播更新: (config-router)neighbor neighbor\_ip

启用触发更新: (config-if)ip rip triggered 注意:1 触发更新只在串行线路中有效.2 必须在物理链路的两端都进行触发更新的配置.

解决不连续子网的办法： 1 更换协议 2 配置辅助地址 配置第二地址: (config-if)ip add ip\_add ip\_submask secondary 当配置了第二地址之后,不需要再在路由选择协议中对本地址进行通告.

更改RIP的记时器: (config-router)timers basic \* \* \* \* 共4个值,必须都写上.

针对RIP的DEBUG命令: Debug ip rip\[event\] Debug ip rip trigger

RIP V2与V1的兼容性: (config-if)ip rip send version 1/2 (config-if)ip rip receive version 1/2

配置RIP V2的认证： (config)#key chain \*\*\*(指定一个名字) (config-keychain)#key 1（指定一个编号） (config-keychain-key)#key-string cisco（指定一个认证用的密码） 在一个网络中，KEY CHAIN的值可以不同，但KEY和KEY-STRING必须相同 在接口上开启认证： (config-if)#ip rip authentication mode md5 在接口上开启MD5认证 (config-if)#ip rip auth key-chain \*\*\* 指定认证KEY-CHAIN的名字，必须与前面配置的一样。 当在两个相连的R上配置认证时，KEY CHAIN可以不一样，但KEY值和KEY-STRING必须一样。

#show ip interface brief??? 查看所有接口的状态 (config-line)#logging sy

关闭自动汇总 (config-router)#no auto-summary

RIP的手动汇总 (config-if)#ip summary-address rip subnet subnet\_mask 链路状态型

平衡混合型 EIGRP CISCO私有的技术，由IGRP发展而来。 IGRP 内部网关路由选择协议。 EIGRP 增强型内部网关路由选择协议。 EIGRP 的特性： 1 支持多种被路由协议。使用PDM机制。 2 快速收敛 3 100%的无环路无类别路由协议，支持CIDR、VLSM。 4 配置简单 5 使用部分的、增量的、有界的更新 6 支持等开销及非等开销的负载均衡（需要手工配置） 7 使用组播及单播来发送数据包代替广播 8 可以在网络中任何一个节点上进行汇总??????

EIGRP的三张表： 1 邻居表。保存所有的直连运行EIGRP的邻居路由器。 2 拓扑表。保存所有从EIGRP邻居所学到的路由信息。 3 路由表。只列出从拓扑表计算得来的最好的路由表。

EIGRP使用IP数据包来转发路由信息。协议字段值为88。 EIGRP建立邻居关系必须具备两个条件，第一，K值相同，第二。AS号相同。 EIGRP使用224.0.0.10这个IP地址发送HELLO数据包。 使用的度量值共有5个： 带宽、延时、可靠性、负载、MTU最大传输单元。 5个K值对应以上的5个度量值，默认为10100，默认情况下只考虑带宽和延时。 UPDATE 更新 QUERY 查询 REPLY 回复 HELLO ACK 确认

EIGRP的几个基本概念： AD通告距离：下一跳通告的到目标网络的开销 FD可行距离：本地到达目标网络的度量值 FDmin最小可行距离：到达目标网络的最小度量值 FC可行条件：AD<FDmin Successor后继者：通告最佳路由的下一跳路由器 Feasible Successor可行后继者：满足可行条件的下一跳路由器

配置EIGRP认证：点对点的认证，此配置针对接口配置 MD5加密的认证 (config-if)#ip authentication mode eigrp as号 MD5 开启EIGRP的MD5认证 (config-if)#ip authentication key-chain eigrp as号 keyid??? 设置一个MD5的密码 OSPF特性： 开放标准 最短路径优先 链路状态型协议 与EIGRP一样，使用IP数据包发送更新，IP报文协议号为89 只支持IP协议环境/EIGRP支持IP、IPX、APPLETALK 支持等价负载均衡/EIGRP支持等开销、非等开销。 OSPF也有三张表 邻居表 列出所有邻居 拓扑表 保存所在区域的所有链路状态信息 路由表 只列出最好的路由信息 LSA 链路状态通告

????????????? OSPF???????????????????? EIGRP

邻居表????? 列出所有邻居?????????? 列出所有邻居

拓扑表????? 所在网络的地图???????? 邻居的路由表

??????????? 使用SPF算法??????????? 使用DUAL算法 ???????????????????? 得到最佳路由 路由表????? 路由表????????????????? 路由表

OSPF需要一个层次化的网络结构，它包含两种不同的层次区域 ·传输区域(骨干AREA 0) ·普通区域

注意 骨干区域必须是区域0，所有的普通区域必须要连接到骨干区域，不同的非骨干区域间的通信必须要经过骨干区域。

划分区域的目的在于： 1 减小路由表的大小 2 限制LSA的扩散LINK-STATE 链路状态 3 加快OSPF的收敛速度 4 增强OSPF的稳定性

OSPF???? 的COST是10的8次方除以带宽单位是BPS EIGRP??? 的COST是10的7次方除以带宽单位是BPS

OSPF的五种数据包

HELLO???? 用来发现链路状态变化，建立邻居关系 DATABASE DESCRIPTION??? 数据库描述。描述自己的数据库(广告) LINK-STATE REQUEST 向邻居请求一段链路状态数据库 LS UPDATE??? LSA的集合 LS ACK???? 确认信息

与EIGRP一样，中间三个需要确认。

OSPF建立邻居关系的匹配条件(hello数据包中包含的信息)： 1??? ROUTER ID 如果有手工指定，则使用手工指定的。在没有的时候去看LOOPBACK接口上的IP，如果有多个LO接口，则使用最大的IP地址，如果没有LO接口，则使用物理接口中IP地址最大的一个。 2??? HELLO/DEAD INTERVALS 发送/接受HELLO数据包的时间间隔。 3??? NEIGHBORS 列出所有已知的直连邻居 4??? AREA ID 区域的ID号 5??? ROUTER PRIORITY 路由器的优先级 6??? STUB AREA FLAG 末节网络的标识。

DR 指定路由器 BDR 备份指定路由器

OSPF使用224.0.0.5/224.0.0.6发送更新包 224.0.0.6代表发送给ALL DR ROUTERS 只发送给DR路由器 224.0.0.5代表发送给ALL SPF ROUTERS 发送给所有路由器 在一个OSPF网络中，所有的非DR、BDR路由器，只与DR或BDR路由器建立邻居关系。DR和BDR路由器建立邻居关系。

配置命令： (config)#router ospf 进程号 |在一个R上可以运行多个OSPF进程。不同R上可以使用不同进程号。 (config-router)#network 网段号 通配符掩码 AREA 区域号 | AREA0是必须的！！！

OSPF的认证和汇总： ABR area border router：区域边界路由器 手工指定一个router id (config-router)#router-id \*.\*.\*.\* 强行刷新OSPF进程，以使ROUTER ID生效。 #clear ip ospf process

在查看路由信息的时候，看到O IA??? 表示区域间路由。 #show ip ospf database summary \*.\*.\*.\* 查看IP汇总、LSA的信息。 STUB NETWORK 末节网络

配置汇总： (config-router)#area 区域号 range 网络号 子网掩码 配置认证：认证可以针对区域间做认证。 点对点的认证：此配置针对接口配置 一 MD5加密的认证 (config-if)#ip ospf authentication message-digest 开启OSPF的MD5认证 (config-if)#ip ospf message-digest-key KEYID md5 密码??? 设置一个MD5的密码 二 简单口令认证 (config-if)#ip ospf authentication-key 密码??? 只有这一条命令

全区域认证： (config-router)#area 区域号 authentication message-digest???? 开启区域\*中的MD5认证 有类别路由协议与无类别路由协议间的重发布。重发布配置在边界路由器上。 重发布分为单向重发布/双向重发布两种。 EIGRP的默认内部管理距离是90，外部管理距离是170。 在边界路由器上，要配置多个路由协议，在做重发布的时候，希望在哪个路由协议里发布另一个协议，那么被发布的这个协议就是源协议。在配置时，需在目的协议中发布源路由协议。 (config-router)redistribute 源路由协议\[AS号\] metric 目标路由协议度量值 subnets

在将任何协议重发布到OSPF中时，在重发布命令的最后要加上SUBNETS，否则将无法实现对子网的支持。

NAT技术：

NAT 一个私有地址和一个共有地址的转换 NAT过载 多个私有地址和一个共有地址的转换 PAT使用端口号来标识一个唯一的私有地址。

PC--------------NAT-------------INTERNET ????? 10.1.1.0/24?????? 61.1.1.0/24 做NAT的前期准备： PC(config)#ip route 0.0.0.0 0.0.0.0 10.1.1.2 NAT(config)#ip route 0.0.0.0 0.0.0.0 61.1.1.2 ISP(config)#ip route 10.1.1.0 255.255.255.0 61.1.1.1 先做好两条缺省路由和一条静态路由，再开始做NAT。

静态NAT 一个内部IP对应一个外部IP 在做NAT的R上配置： (config)#ip nat inside source static PC地址IP 外出IP地址 然后在使用内部地址的接口上进行配置： (config-if)#ip nat inside 在使用外部地址的接口上进行配置： (config-if)#ip nat outside NAT过载 多个内部IP对应一个外部IP。NAT过载必须要配合ACL来使用。 (config)#access-list 1 permit 源网络 通配符掩码 先创建一个允许被NAT的范围,只针对内部的IP。 (config)#ip nat inside source list ACL号 interface 出口接口 overload 再配置PAT

动态NAT 多个内部IP对应多个外部IP。也必须配合ACL来用。 先创建一个允许被NAT的范围,只针对内部的IP。 (config)#access-list 1 permit 源网络 通配符掩码 定义一个地址池 (config)#ip nat pool 地址池名 外部开始IP 外部结束IP netmask 子网掩码 应用PAT (config)#ip nat inside source list ACL名 pool 地址池名 overload

在配置NAT之后，如果没有路由协议，那么必然是不通的。 验证的时候，在做NAT的路由器上使用DEBUG IP NAT，在ISP的路由器上使用DEBUG IP PACKET来查看NAT结果。

DHCP 配置一个DHCP地址池 Router(config)#ip dhcp pool test 定义所要分配的IP地址段 Router(dhcp-config)#network 10.1.1.0 255.255.255.0 定义缺省网关 Router(dhcp-config)#default-router 10.1.1.1 定义DNS地址 Router(dhcp-config)#dns-server 218.30.19.40 定义租约期限 Router(dhcp-config)#lease 7 排除一些IP不分配： (config)#ip dhcp excluded-address 开始IP 结束IP 手工开启DHCP服务： (config)#no/ service dhcp 配置设备通过DHCP得到IP地址： (config-if)#ip add dhcp 注意：DHCP不能做在串行接口上，只能做在以太网接口上。作为DHCP的路由器在发送DHCP信息的接口上，必须配置IP地址。默认情况下，DHCP服务是开启状态。

交换知识：

在连接两个交换机时，必须使用交叉线。

交换机的工作方式：

一 对称式：设备上所有的接口速率一致。包括UPLINK口。 UPLINK口，又叫做级联口，用于交换机做堆叠工作。 简单的理解堆叠就是将多个交换机相互连接后构成一个大的广播域。 一般来说，对称式设备在级联时，可以直接使用平行线。

二 非对称式：UPLINK口相对于其他接口来说，速率更大。 一般非对称式设备级联时，使用的是交叉线。

交换机的转发方式：

一 存储转发 在转发数据前，必须接受到整个数据帧。然后，交换机读取目的或源地址，并将数据进行过滤后，进行转发。 可以对数据进行错误检查，不纠错。

二 直通转发(贯穿式转发) 在接受到数据帧时，只读取目的地址，然后在完成的数据帧到达之前，就将数据进行转发。

比较如下

?????? 存储转发???????????????? 直通转发

接受完整的数据帧?????????? 接受部分数据帧 ??? 支持错误检查???????????? 不支持错误检查 ???? 延迟相对大??????????????? 延迟相对小

直通转发又分为两种方式： 1.快速转发交换：一接收到目的地址就立即转发。 2.自由分段(无碎片)交换：只接受前64字节，并对其进行检验后转发。

交换机工作状态： 半双工。一般当交换机下连接有HUB时，交换机处于半双工状态(使用CSMA/CD机制)。 全双工。只用于点到点的网络中。而且所有的接入节点都必须支持全双工。 STP(生成树协议)的几个概念： 1 根网桥 2 根端口 3 指定端口 根端口和指定端口都用于转发数据流量 非指定端口也叫做阻塞端口，或者丢弃端口。接受到数据流量后将数据丢弃。 根端口的确定是基于带宽的。

STP的各种状态： 阻塞状态(bloking)：不转发数据帧，接受BPDU。 侦听状态(listening):不转发数据帧，侦听数据帧。 学习状态(learning):不转发数据帧，学习地址。 转发状态(forwarding):转发数据帧，学习地址。 禁止状态(disabled):不转发数据帧，不接受BPDU。

每个网络中都有一个根网桥 每个非根网桥都有一个根端口 每个网段都有一个指定端口 所有的非指定端口不使用

STP的收敛 如果整个网络中所有的接口，不是BLOCKING状态，就是转发状态时，我们就认为STP收敛完成了。如果网络拓扑改变了，那么STP就会重计算，在这个时候是无法转发数据的。

VLAN(虚拟局域网)：使用交换机，将物理上处于同一个地域的PC或服务器，划分到不同的逻辑域中。在一个VLAN中的设备只能与本VLAN中的设备相互通信。VLAN与VLAN之间无法通信。 VLAN间相互通信，必须使用路由。 VLAN主要针对网络的可扩展性、安全性、可管理性。 VLAN的优点： 1 提供一套灵活、可变动的管理方式。 2 大大的增加了安全性。首先，VLAN可以限制一个VLAN组中成员的数量；其次，可以防止外来用户在未经许可的情况下接入本地网络；第三，可以把所有未使用的端口单独划分为一个VLAN。

默认情况下，交换机中的VLAN1，我们称之为“管理VLAN”，可以把VLAN1看作是一个接口，并给VLAN1配置IP地址，VLAN1上的IP被叫做“管理IP”。

静态VLAN： 在配置VLAN时需要注意的几点： 1 29XX系列交换机最大支持到1024个VLAN。但一般取决于交换机型号和交换机接口数量。 2 交换机中，始终有一个缺省的VLAN，编号为VLAN 1。交换机下所有接口，默认全部在VLAN 1中。 3 交换机之间相互通信，使用的数据封装协议，为802.1Q或ISL。 BPDU 桥接数据协议单元

创建一个VLAN #vlan database 进入VLAN配置模式 (config-vlan)vlan 编号 name VLAN名 建立一个VLAN (config-if)#switchport mode access 更改工作模式为访问模式 (config-if)#switchport access VLAN编号 将接口绑定到某个VLAN中

VTP协议 VLAN TRUNK PROTOCOL VLAN干道协议 简单的说，VTP就是在VLAN间做中继。中继，就是指使用两点间的一条物理线路，来承载多个逻辑链路，多个逻辑链路之间，互不干扰。 使用VTP，我们可以在一台交换机上设置好多个VLAN后，在其他与本交换机直连的交换机上，通过简单的配置，使其可以学习到VLAN的配置信息。 在配置VTP前，首先要配置一个域。 (vlan)#vtp domain 域名 更改为客户机模式 (vlan)#vtp client 进入接口，更改接口模式为干道 (config-if)#switchpot mode trunk

?6
