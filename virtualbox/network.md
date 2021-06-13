# NetWork

[VirtualBox官方解释](https://www.virtualbox.org/manual/ch06.html)

## 连接方式

### NAT

NAT：Network Address Translation，网络地址转换

Guest访问网络的所有数据都是由Host提供的，Guest并不真实存在于网络中，Host与网络中的任何机器都不能查看和访问到Guest的存在。

Guest可以访问Host能访问到的所有网络，但是对于Host以及网络上的其他机器，Guest又是不可见的，甚至主机也访问不到Guest。

*Guest为虚拟机，Host为主机*

Mac VMWare fusion可以双向访问

### Bridged

通过主机网卡，架设了一条桥，直接连入到网络中了。因此，它使得虚拟机能被分配到一个网络中独立的IP，所有网络功能完全和在网络中的真实机器一样。

优点：
- 设置简单

缺点：
- 占用内网IP
- 无法设置静态IP

### Internal

断开虚拟机之外的所有网络，同网段的虚拟机之间可以相互访问。

### Host-Only

使用虚拟网络适配器进行连接，不使用物理网络适配器。

**Virtual HDCP服务器会会默认分配IP，关闭后可以自定义IP**

## Compare

### IP

| 连接方式 | IP | 是否访问外网 |
| - | - | - |
| NAT | 范围不定，可设静态IP | 是 |
| Bridged | 自动分配，有内网网段决定 | 是 |
| Host-Only | 与虚拟网卡网段保持一致 | 否 |
| Internal | 范围不定，可设静态IP | 否 |

### PING

| 连接方式 | G -> H | H -> G | G -> OH | OH -> G | G -> G |
| - | - | - | - | - | - |
| NAT | √ | × | √ | × | × |
| Bridged | √ | √ | √ | √ | √ |
| Host-Only | 默认不能，需设置 | 默认不能，需设置 | 默认不能，需设置 | 默认不能，需设置 | √ |
| Internal | × | × | × | × | 同网段可以 |

*H为主机，G为虚拟机，OH为同网段其他主机*

*Host-Only的设置方法：将虚拟机的静态IP设置为虚拟网络适配器同网段的*

## Q&A

双向PING如何搭配才是最好的方案？

双网卡，一个为NAT，另一个为Host-Only。

https://www.cnblogs.com/townsend/p/10337976.html

NAT用于连接外网，Host-Only用于主机与虚拟机交互测试。