# OSI 和 TCP/IP 参考模型

# 为什么要进行网络分层

早期计算机：系统封闭，所有部件都是同一厂商

- 优点：安全性高、性能强
- 缺点：兼容性差、更新周期慢

# OSI 参考模型：CPU、内存、硬盘、显卡、主板等标准化

- 某一层所做的改动不会影响其他层，利于设计、开发和故障排除。
- 通过定义在模型的每一层实现功能，鼓励产业标准化。
- 通过网络组件的标准化，允许多个供应商协同开发。
- 允许各种类型的网络硬件和软件互相通信，无缝融合。
- 促进网络技术快速迭代，降低成本。

# OSI 参考模型

- 应用层：各种应用程序、协议。
- 表示层：数据和信息的语法转换内码，数据压缩解压、加密解密。
- 会话层：为通信双方指定通信方式，并创建、注销会话。
- 传输层：提供可靠或不可靠的端到端传输。
- 网络层：逻辑寻址，路由选择。
- 数据链路层：将分组数据封装成帧，提供节点到节点的传输，差错控制。
- 物理层：在媒介上传输比特流，提供机械和电气规约。

# TCP/IP 参考模型

- 应用层：对应 OSI 参考模型的高层，为用户提供所需的各种服务器。例如：FTP、Telnet、DNS、SMTP 等。
- 传输层：为应用层提供端到端的通信功能。
- 网络层：定义逻辑地址，路由选择（路由和寻址）。
- 数据链路层：将分组数据封装成帧，提供节点到节点的传输。
- 物理层：在媒介上传输比特流，提供机械和电气规约。

# OSI 与 TCP/IP 模型对应的协议

<table>
  <tr>
    <td>OSI七层网络模型</td>
    <td>TCP/IP四层模型</td>
    <td align="center">网络协议</td>
  </tr>
  <tr>
    <td>应用层（Application）</td>
    <td rowspan="3">应用层</td>
    <td rowspan="3">HTTP、FTP、TFTP、DHCP、NTP、POP３、IMAP４、Telnet、SNMP、SMTP、DNS、LDAP、SSH</td>
  </tr>
  <tr>
    <td>表示层（Presentation）</td>
  </tr>
  <tr>
    <td>会话层（Session）</td>
  </tr>
  <tr>
    <td>传输层（Transport）</td>
    <td>传输层</td>
    <td>TCP、UDP</td>
  </tr>
  <tr>
    <td>网络层（Network）</td>
    <td>网络层</td>
    <td>IP、ICMP、ARP、RARP、OSPF、VRRP、IGMP、IS-IS、IPsec、BGP</td>
  </tr>
  <tr>
    <td>数据链路层（DataLink）</td>
    <td rowspan="2">网络接口层</td>
    <td rowspan="2">PPP、PPTP、以太网</td>
  </tr>
  <tr>
    <td>物理层（Physical）</td>
  </tr>
</table>
