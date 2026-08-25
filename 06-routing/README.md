# Lab 6 - Router ve Routing Table

## Amaç

Farklı IP subnetlerinde bulunan cihazların router kullanarak birbirleriyle iletişim kurmasını sağlamak ve router'ın routing table yapısını incelemek.

Bu labda daha önce öğrenilen IP adresleme, subnetting, ARP ve default gateway kavramları routing ile birlikte kullanılmıştır.

## Topoloji

PC1 ─── Switch1 ─── Router ─── Switch2 ─── PC2

## IP Adresleme

| Cihaz | Interface | IP Adresi | Subnet Mask | Default Gateway |
|---|---|---|---|---|
| PC1 | Fa0 | 192.168.10.10 | 255.255.255.0 | 192.168.10.1 |
| Router | G0/0 | 192.168.10.1 | 255.255.255.0 | - |
| Router | G0/1 | 192.168.20.1 | 255.255.255.0 | - |
| PC2 | Fa0 | 192.168.20.10 | 255.255.255.0 | 192.168.20.1 |

## Router Yapılandırması

Router'ın G0/0 interface'i:

192.168.10.1/24

Router'ın G0/1 interface'i:

192.168.20.1/24

Interface'ler `no shutdown` komutu kullanılarak aktif hale getirildi.

## Routing Table

`show ip route` komutu ile router'ın routing table'ı incelendi.

Router aşağıdaki networkleri doğrudan bağlı (`Connected`) olarak öğrendi:

C 192.168.10.0/24 → GigabitEthernet0/0
C 192.168.20.0/24 → GigabitEthernet0/1

Buradaki `C`, Connected anlamına gelmektedir.

Router ayrıca kendi interface IP adresleri için Local (`L`) route'lara da sahiptir.

## İletişim Testleri

### PC1 → Router G0/0

`ping 192.168.10.1`

Sonuç:

4 packets received
0% packet loss

### PC1 → Router G0/1

`ping 192.168.20.1`

Sonuç:

4 packets received
0% packet loss

### PC1 → PC2

`ping 192.168.20.10`

Sonuç:

4 packets received
0% packet loss

PC1 ve PC2 farklı subnetlerde bulunmasına rağmen router aracılığıyla başarılı şekilde iletişim kurabildi.

## ARP Analizi

PC1 üzerinde `arp -a` komutu çalıştırıldığında router'ın MAC adresinin ARP tablosunda bulunduğu görüldü.

Örneğin:

192.168.20.1 → 0060.5c0c.4d02

PC2'nin `192.168.20.10` adresi ise PC1'in ARP tablosunda bulunmadı.

Bunun nedeni PC1 ve PC2'nin farklı subnetlerde olmasıdır.

PC1, PC2'nin MAC adresini doğrudan ARP ile öğrenmek yerine paketi default gateway olan router'a gönderdi.

## Paket Akışı

PC1'den PC2'ye gönderilen paket kabaca şu yolu izledi:

PC1
 
  ↓
 
Switch1
 
 ↓
 
Router G0/0
 
 ↓

Routing Table
 
 ↓

Router G0/1
 
 ↓

Switch2
 
 ↓

PC2

PC1, hedef IP adresinin kendi subnetinde olmadığını belirledi ve paketi default gateway'e gönderdi.

Router routing table'ına bakarak:

192.168.20.0/24 → G0/1

yolunu kullandı ve paketi PC2'ye iletti.

## Önemli Kavramlar

Bu labda aşağıdaki kavramlar birlikte gözlemlendi:

- Router
- Routing Table
- Default Gateway
- ARP
- MAC Address
- IP Address
- Subnet
- Connected Route
- Local Route

## Öğrendiklerim

- Router'ın farklı IP networklerini birbirine bağladığını öğrendim.
- Router'ın doğrudan bağlı networkleri otomatik olarak routing table'a eklediğini gördüm.
- Farklı subnetlerdeki cihazların iletişimi için default gateway'in önemini gördüm.
- PC1'in farklı subnet'teki PC2'nin MAC adresini doğrudan ARP ile öğrenmediğini gözlemledim.
- PC1'in bunun yerine router'ın MAC adresini kullandığını gördüm.
- Layer 2 ve Layer 3 iletişiminin router üzerinden nasıl gerçekleştiğini gözlemledim.

## Kullanılan Komutlar

show ip interface brief
show ip route
arp -a
ping

## Kullanılan Araçlar

- Cisco Packet Tracer
  
