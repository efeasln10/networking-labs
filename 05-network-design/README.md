# Lab 5 - Network Tasarımı ve IP Adres Planlaması

## Amaç

192.168.50.0/24 adres bloğunu kullanarak küçük bir şirket için
IPv4 adresleme planı oluşturmak.

Şirketin üç departmanı bulunmaktadır:

- Yazılım
- İnsan Kaynakları
- Yönetim

## Gereksinimler

| Departman | Gerekli Host Sayısı |
|---|---:|
| Yazılım | 50 |
| İnsan Kaynakları | 20 |
| Yönetim | 10 |

## IP Adres Planı

### Yazılım

Network:
192.168.50.0/26

Subnet Mask:
255.255.255.192

Kullanılabilir Host Aralığı:
192.168.50.1 - 192.168.50.62

Broadcast:
192.168.50.63

Kullanılabilir Host:
62

---

### İnsan Kaynakları

Network:
192.168.50.64/27

Subnet Mask:
255.255.255.224

Kullanılabilir Host Aralığı:
192.168.50.65 - 192.168.50.94

Broadcast:
192.168.50.95

Kullanılabilir Host:
30

---

### Yönetim

Network:
192.168.50.96/28

Subnet Mask:
255.255.255.240

Kullanılabilir Host Aralığı:
192.168.50.97 - 192.168.50.110

Broadcast:
192.168.50.111

Kullanılabilir Host:
14

## Özet

| Departman | Network | CIDR | Kullanılabilir Host |
|---|---|---|---:|
| Yazılım | 192.168.50.0 | /26 | 62 |
| İnsan Kaynakları | 192.168.50.64 | /27 | 30 |
| Yönetim | 192.168.50.96 | /28 | 14 |

## İletişim Deneyi

Bilgisayarlar aynı switch'e bağlandı ancak farklı IP
subnetlerinden adresler verildi.

Switch bilgisayarların MAC adreslerini öğrenebildi fakat
bilgisayarlar Layer 3 seviyesinde birbirleriyle iletişim
kuramadı.

Bu deney sonucunda:

- Bir switch'in farklı cihazların MAC adreslerini öğrenebildiği,
- MAC adresini bilmenin farklı subnetler arasında IP iletişimi
  sağlayamadığı,
- Farklı IP networkleri arasında iletişim için routing gerektiği

gözlemlendi.

## Öğrendiklerim

- IP adresleme planı oluşturmayı öğrendim.
- Host gereksinimine göre uygun CIDR prefix seçmeyi öğrendim.
- Farklı departmanlar için farklı büyüklüklerde subnetler
  oluşturmayı öğrendim.
- Subnetting kullanarak departmanları mantıksal olarak
  birbirinden ayırmayı öğrendim.
- Layer 2 switching ile Layer 3 routing arasındaki farkı gördüm.

## Gelecekte Yapılabilecek Geliştirmeler

Bu network ileride aşağıdaki teknolojiler kullanılarak
geliştirilebilir:

- VLAN
- Inter-VLAN Routing
- DHCP
- Access Control List (ACL)
- Dynamic Routing Protocols
