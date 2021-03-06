---
# __KIẾN TRÚC SOURCE CODE CỦA PFSENSE__
---

# Overview
- pfsense bao gồm giao diện WebUI + FreeBSD kernel 

- Core xử lý của pfsense hoàn toàn không khác biệt so với FreeBSD kernel. Phần khác biệt là WebUI.(https://forum.level1techs.com/t/plain-freebsd-vs-pfsense/117680/5)

- Phiên bản DPDK của pfsense là pfsense TNSR. Tuy nhiên, do core xử lý của pfsense hoàn toàn dựa trên FreeBSD nên f-stack cũng có thể coi là phiên bản DPDK của pfsense (https://forum.netgate.com/topic/130121/f-stack)

- Hiện tại, pfsense bao gồm các features (modules sau):
  - network firewall: L2-L4 filters (support cả stateful và stateless)

  - application firewall: sử dụng các package Snort, Suricata

  - NAT

  - Routing

  - Bridging

  - VLAN

  - VPN

  - IPSec

  - Traffic shaping

  - LB: network (L2-L4) LB và application LB (sử dụng các package HAPROXY, varnish)

  - HA

- Trên github, source code của pfsense gồm 3 phần:
  - pfsense: The web UI, backend configuration code and build tools

  - FreeBSD-ports: Phần mềm quản lý, hỗ trợ cài đặt các package trên FreeBSD

  - FreeBSD-src: Source code FreeBSD kernel được pfsense customize (các firewall L2-L4, implement các giao thức network như DHCP, VPN, HA ...)

- Việc develop pfsense có thể chia thành 2 mức:
  - Mức network firewall: customize trực tiếp vào FreeBSD-src

  - Mức application firewall: develop như là một software độc lập dựa trên các API giao tiếp network của FreeBSD, sau đó đóng gói theo chuẩn của FreeBSD => có thể cài đặt vào pfsense thông qua FreeBSD-ports (ví dụ: snort, suricata)

- Tài liệu này tập trung vào cấu trúc source code của FreeBSD-src

# Cấu trúc source code của FreeBSD-src
- include: kernel header files

- sys: kernel source code
  - sys/protosw.h: Định nghĩa struct trừu tượng ```struct protosw``` cho các network protocol xử lý data theo mô hình socket. Trong đó bao gồm các hook function pointers pr_input, pr_output để xử lý các packet chiều IN/OUT qua protocol.

  - net: Impelement các interfaces cho network devices
    - if_var.h: Định nghĩa struct trừu tượng ```struct ifnet``` cho tất cả các network interfaces. Trong đó, bao gồm các hook function pointers if_input, if_bridge_input và if_output, if_bridge_output để xử lý các packets chiều IN/OUT.

    - netisr.h: Định nghĩa struct trừu tượng ```struct netisr_handler``` thể hiện handler của các network protocols.Trong đó, bao gồm hook function pointer nh_handler trỏ đến hàm xử lý packet của các network protocols.

    - if_ethersubr.c: Interface for ethernet. Implement và đăng ký (attach) các hooks ether_input và ether_output để xử lý packets.

    - if_bridge.c: Interface for bridge. Implement và đăng ký (attach) các hooks bridge_input và bridge_output thực hiện chức năng bridge.

    - if_vlan.c: Interface for bridge. Implement và đăng ký (attach) các hooks vlan_input và vlan_output để xử lý packets trong vlan.

    ...


  - netinet: Implement IPv4 network stack

    - if_ether.c, if_ether.h: Implement protocol ARP. Đăng ký hook function arp_intr để xử lý ARP packets

    - ip.h, ip_var.h, ip_input.c, ip_output.c ...: Implement protocol IPv4. Đăng ký các hook function ip_input, ip_direct_input, ip_output để xử lý các IPv4 packets

    - ip_carp.h, ip_carp.c: Impelement protocol CARP (chức năng tương đương VRRP). Đăng ký các hook function carp_input, rip_output để xử lý các CARP packets. Đây chính là core thực hiện chức năng keepalived, HA cho pfsense

    - ip_icmp.h, ip_imcp.c: Impelement protocol ICMP. Đăng ký các hook function icmp_input, icmp_output để xử lý các ICMP packets. 

    - udp.h, udp_usrreq.c ...: Impelement protocol UDP. Đăng ký các hook function udp_input, udp_output để xử lý các UDP packets. 

    - tcp.h, tcp_input.c, tcp_output.c ...: Impelement protocol TCP. Đăng ký các hook functions tcp_input, tcp_output để xử lý các TCP packets. 

    - sctp.h, sctp_input.c, sctp_output.c ...: Impelement protocol SCTP. Đăng ký các hook functions sctp_input, sctp_output để xử lý các SCTP packets. 

    ...


  - netinet6: Implement IPv6 network stack

    - nd6.h, nd6.c: Impelement protocol NDP.

    - ip6.h, ip6_var.h, ip6_input.c, ip6_output.c ...: Implement protocol IPv6. Đăng ký các hook function ip_input, ip_direct_input, ip_output để xử lý các IPv6 packets

    - udp6_var.h, udp6_usrreq.c ...: Impelement protocol UDP trên IPv6 stack


  - netipsec (ipsec.h, ipsec6.h, ipsec.c, ipsec_input.c, ipsec_output.c ...): Implement IPSEC network stack


  - netpfil: Implement network filters

    - ipfw: ip packet filters. Implement L2-L4 filters (bao gồm các stateful và stateless tables)
 

    - pf: packet filters (pf được FreeBSD phát triển để thay thế cho ipfw. Hiện tại, có thể sử dụng cả ipfw và pf trên FreeBSD, nhưng ipfw không còn được maintain bởi FreeBSD)

  - opencrypto: framework for drivers of cryptographic hardware


 




