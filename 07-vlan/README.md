# Lab 7 - VLAN ve Inter-VLAN Routing

## Amaç

Bir switch üzerinde farklı VLAN'lar oluşturmak, switch portlarını ilgili VLAN'lara atamak, trunk bağlantısı kurmak ve Router-on-a-Stick yöntemiyle farklı VLAN'lar arasında iletişim sağlamak.

Bu labda VLAN, Access Port, Trunk, 802.1Q, Subinterface, Default Gateway, ARP ve Inter-VLAN Routing kavramları birlikte uygulanmıştır.

## Topoloji

PC1 ───┐
       

PC2 ───┤ VLAN 10
  
Switch ─── Router
                G0/0

PC3 ───┤ VLAN 20

PC4 ───┘

Switch G0/1 ↔ Router G0/0 bağlantısı trunk olarak yapılandırılmıştır.

## VLAN Yapısı

| VLAN | Departman | Portlar |
|---|---|---|
| 10 | YAZILIM | Fa0/1, Fa0/2 |
| 20 | IK | Fa0/3, Fa0/4 |

## IP Adresleme

### VLAN 10 - YAZILIM

Network:

192.168.10.0/24

Gateway:

192.168.10.1




PC1:

IP Address: 192.168.10.10

Subnet Mask: 255.255.255.0

Default Gateway: 192.168.10.1




PC2:

IP Address: 192.168.10.11

Subnet Mask: 255.255.255.0

Default Gateway: 192.168.10.1

### VLAN 20 - IK

Network:

192.168.20.0/24

Gateway:

192.168.20.1

PC3:

IP Address: 192.168.20.10

Subnet Mask: 255.255.255.0

Default Gateway: 192.168.20.1

PC4:

IP Address: 192.168.20.11

Subnet Mask: 255.255.255.0

Default Gateway: 192.168.20.1

## VLAN Yapılandırması

Switch üzerinde iki VLAN oluşturuldu:

VLAN 10:

vlan 10

name YAZILIM

VLAN 20:

vlan 20

name IK

## Access Port Yapılandırması

PC1 ve PC2'nin bağlı olduğu portlar VLAN 10'a atandı:

interface range fastEthernet 0/1-2

switchport mode access

switchport access vlan 10

PC3 ve PC4'ün bağlı olduğu portlar VLAN 20'ye atandı:

interface range fastEthernet 0/3-4

switchport mode access

switchport access vlan 20

## Trunk Yapılandırması

Switch ile router arasındaki G0/1 bağlantısı trunk olarak yapılandırıldı:

interface gigabitEthernet 0/1

switchport mode trunk

Bu trunk bağlantısı üzerinden VLAN 10 ve VLAN 20 trafiği taşındı.

## Router-on-a-Stick

Router'ın G0/0 fiziksel interface'i üzerinde iki subinterface oluşturuldu.

### VLAN 10

interface GigabitEthernet0/0.10

encapsulation dot1Q 10

ip address 192.168.10.1 255.255.255.0

### VLAN 20

interface GigabitEthernet0/0.20

encapsulation dot1Q 20

ip address 192.168.20.1 255.255.255.0

Bu yapı sayesinde router tek fiziksel interface üzerinden iki farklı VLAN'a hizmet verdi.

## Routing Table

show ip route komutu ile routing table incelendi.

Router aşağıdaki networkleri doğrudan bağlı olarak öğrendi:

C 192.168.10.0/24 → GigabitEthernet0/0.10

C 192.168.20.0/24 → GigabitEthernet0/0.20

Ayrıca router'ın kendi interface adresleri Local (L) route olarak görüldü:

L 192.168.10.1/32 → GigabitEthernet0/0.10

L 192.168.20.1/32 → GigabitEthernet0/0.20

## İletişim Testleri

### PC1 → PC2

ping 192.168.10.11

Sonuç:

Başarılı.

PC1 ve PC2 aynı VLAN içerisinde bulunduğu için doğrudan Layer 2 seviyesinde iletişim kurabildi.

### PC3 → PC4

ping 192.168.20.11

Sonuç:

Başarılı.

PC3 ve PC4 aynı VLAN içerisinde bulunduğu için doğrudan Layer 2 seviyesinde iletişim kurabildi.

### PC1 → PC3

ping 192.168.20.10

Sonuç:

Başarılı.

İlk ping paketinde Request timed out görülürken sonraki üç paket başarıyla ulaştı.

Sonuç:

4 paket gönderildi, 3 paket alındı, %25 paket kaybı.

İlk paketteki kayıp, iletişimin başlangıcında gerekli ARP çözümlemelerinin gerçekleşmesiyle ilişkilendirildi.

## ARP Analizi

PC1 üzerinde arp -a komutu çalıştırıldığında aşağıdaki kayıtlar görüldü:

192.168.10.1 → Router'ın MAC adresi

192.168.10.11 → PC2'nin MAC adresi

PC3 farklı subnet'te bulunduğu için PC1'in ARP tablosunda PC3'ün MAC adresi bulunmadı.

PC1, PC3'e ulaşmak için PC3'ün MAC adresini doğrudan öğrenmek yerine default gateway olan 192.168.10.1'in MAC adresini ARP ile öğrendi.

Bunun nedeni PC3'ün farklı bir subnet'te bulunmasıdır.

## Paket Akışı

PC1'in PC3'e gönderdiği paket:

PC1

↓

Access Port

↓

VLAN 10

↓

Switch

↓

Trunk

↓

Router G0/0.10

↓

Routing

↓

Router G0/0.20

↓

Trunk

↓

Switch

↓

VLAN 20

↓

PC3

IP seviyesinde:

192.168.10.10 → 192.168.20.10

olarak kalan paket, Layer 2 seviyesinde router üzerinden geçerken farklı Ethernet frame'leri içerisinde taşındı.

PC1'in ilk frame'inin hedef MAC adresi PC3 değil, router'ın MAC adresiydi.

## TTL Analizi

PC1'in PC3'e gönderdiği ping yanıtlarında:

TTL=127

değeri görüldü.

PC3 tarafından gönderilen paket router üzerinden geçtiği için TTL değerinin 128'den 127'ye düştüğü gözlemlendi.

Bu durum paketin bir Layer 3 router üzerinden geçtiğini göstermektedir.

## Öğrenilen Kavramlar

- VLAN
- Broadcast Domain
- Access Port
- Trunk Port
- IEEE 802.1Q
- Subinterface
- Router-on-a-Stick
- Inter-VLAN Routing
- Default Gateway
- ARP
- MAC Address
- Routing Table
- Connected Route
- Local Route
- TTL

## Öğrendiklerim

- VLAN'ların switch üzerinde farklı broadcast domainleri oluşturduğunu öğrendim.
- Access portların belirli bir VLAN'a ait uç cihazları bağlamak için kullanıldığını gördüm.
- Trunk portların birden fazla VLAN'ın trafiğini tek fiziksel bağlantı üzerinden taşıdığını öğrendim.
- 802.1Q encapsulation'ın VLAN bilgisinin trunk üzerinden taşınmasında kullanıldığını gördüm.
- Router-on-a-Stick yöntemiyle tek fiziksel router interface'i üzerinde birden fazla VLAN için subinterface oluşturmayı öğrendim.
- Farklı VLAN ve subnetlerde bulunan cihazların router aracılığıyla iletişim kurabildiğini gözlemledim.
- Farklı subnetlerdeki cihazlar arasında iletişim kurulurken kaynak cihazın hedef cihazın MAC adresini değil, default gateway'in MAC adresini kullandığını gözlemledim.
- Routing table'ın router'ın farklı networklere nasıl ulaşacağını belirlediğini gördüm.
- Layer 2'de MAC adreslerinin, Layer 3'te ise IP adreslerinin iletişimdeki rollerini daha net anladım.

## Kullanılan Komutlar

enable
configure terminal
show vlan brief
show interfaces trunk
show interfaces status
show ip interface brief
show ip route
show running-config
arp -a
ping

## Kullanılan Araçlar

- Cisco Packet Tracer
- GitHub
