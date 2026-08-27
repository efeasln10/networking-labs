# Lab 8 - DHCP ile Otomatik IP Dağıtımı

## Amaç

Bir Cisco router'ı DHCP Server olarak yapılandırmak ve network üzerindeki cihazların IP adreslerini otomatik olarak almasını sağlamak.

Bu labda DHCP, IP Address, Subnet Mask, Default Gateway, DNS Server, DHCP Pool, DHCP Binding ve DORA süreci kavramları incelenmiştir.

## Topoloji

```text
           Router
            G0/0
              |
              |
            Switch
           /   |   \
          /    |    \
        PC0   PC1   PC2
```

Router G0/0 ile Switch arasındaki bağlantı kullanılarak PC'lerin DHCP üzerinden IP adresi alması sağlanmıştır.

## IP Adresleme

Network:

192.168.10.0/24

Router G0/0:

IP Address: 192.168.10.1
Subnet Mask: 255.255.255.0

Router aynı zamanda DHCP Server olarak yapılandırılmıştır.

## DHCP Yapılandırması

Router üzerinde DHCP havuzu oluşturuldu:

ip dhcp pool LAN

DHCP'nin kullanacağı network:

network 192.168.10.0 255.255.255.0

Default Gateway:

default-router 192.168.10.1

DNS Server:

dns-server 8.8.8.8

Router'ın kendi IP adresinin DHCP tarafından başka bir cihaza verilmesini önlemek için:

ip dhcp excluded-address 192.168.10.1

komutu kullanıldı.

## DHCP Pool

DHCP pool'unun kontrolü için:

show ip dhcp pool

komutu kullanıldı.

DHCP pool içerisinde:

Network:

192.168.10.0/24

Default Gateway:

192.168.10.1

DNS Server:

8.8.8.8

olarak yapılandırıldı.

İlk yapılandırmada network komutu eksik olduğu için DHCP pool doğru şekilde çalışmadı.

Daha sonra:

network 192.168.10.0 255.255.255.0

komutu eklenerek sorun giderildi.

## PC Yapılandırması

PC'lerin IP Configuration bölümünden DHCP seçeneği kullanıldı.

PC0 DHCP üzerinden başarılı şekilde IP adresi aldı.

PC0 tarafından alınan bilgiler:

IP Address:

192.168.10.2

Subnet Mask:

255.255.255.0

Default Gateway:

192.168.10.1

DHCP Server:

192.168.10.1

Bu bilgiler router tarafından otomatik olarak sağlandı.

## DHCP Binding

Router üzerinde:

show ip dhcp binding

komutu kullanılarak DHCP tarafından dağıtılan IP adresleri kontrol edildi.

Elde edilen sonuç:

IP Address:

192.168.10.2

Client MAC Address:

0006.2A7D.32B8

Type:

Automatic

Bu kayıt, router'ın 192.168.10.2 IP adresini MAC adresi 0006.2A7D.32B8 olan cihaza DHCP üzerinden tahsis ettiğini göstermektedir.

## Bağlantı Testi

PC0 üzerinden router'ın IP adresine ping gönderildi:

ping 192.168.10.1

Sonuç:

Başarılı.

Bu test PC0 ile router arasındaki iletişimin çalıştığını doğruladı.

## DHCP DORA Süreci

DHCP istemcisinin IP adresi alması sırasında temel olarak dört aşama gerçekleşir.

### 1. DHCP Discover

PC, network üzerinde DHCP Server arar.

PC → DHCP Server

"Network üzerinde DHCP Server var mı?"

### 2. DHCP Offer

DHCP Server, istemciye kullanılabilir bir IP adresi teklif eder.

DHCP Server → PC

"Sana bu IP adresini verebilirim."

### 3. DHCP Request

PC, teklif edilen IP adresini istediğini bildirir.

PC → DHCP Server

"Bu IP adresini kullanmak istiyorum."

### 4. DHCP ACK

DHCP Server isteği onaylar.

DHCP Server → PC

"Tamam, bu IP adresini kullanabilirsin."

Bu dört aşama:

Discover → Offer → Request → ACK

şeklindedir ve DORA olarak adlandırılır.

## Troubleshooting

Lab sırasında PC'nin DHCP Request Failed hatası vermesi üzerine problem sistematik olarak incelendi.

Öncelikle router interface durumu kontrol edildi:

show ip interface brief

Router G0/0:

192.168.10.1
up/up

durumundaydı.

Daha sonra switch üzerindeki VLAN ve port durumu kontrol edildi.

Son olarak DHCP yapılandırması incelendi.

show running-config | section dhcp

komutu sonucunda DHCP pool içerisinde network komutunun eksik olduğu tespit edildi.

Eksik olan:

network 192.168.10.0 255.255.255.0

komutu eklendikten sonra DHCP başarılı şekilde çalıştı.

Bu süreçte troubleshooting sırasında problemin Layer 1 → Layer 2 → Layer 3 ve servis yapılandırması şeklinde sistematik olarak incelenmesinin önemi görüldü.

## Öğrenilen Kavramlar

- DHCP
- DHCP Server
- DHCP Pool
- DHCP Client
- DHCP Binding
- DHCP DORA
- Discover
- Offer
- Request
- ACK
- IP Address
- Subnet Mask
- Default Gateway
- DNS Server
- Excluded Address
- Automatic IP Configuration
- Network Troubleshooting

## Öğrendiklerim

- DHCP'nin cihazlara otomatik olarak IP yapılandırması sağladığını öğrendim.
- DHCP ile IP Address, Subnet Mask, Default Gateway ve DNS Server gibi bilgilerin otomatik olarak dağıtılabildiğini gördüm.
- Cisco router'ın DHCP Server olarak yapılandırılabileceğini öğrendim.
- DHCP Pool oluşturmayı ve network, default-router ve dns-server seçeneklerini yapılandırmayı öğrendim.
- Router'ın kendi IP adresinin DHCP tarafından dağıtılmasını önlemek için excluded-address kullanılabileceğini öğrendim.
- show ip dhcp binding komutuyla hangi IP adresinin hangi cihaza tahsis edildiğini kontrol etmeyi öğrendim.
- DHCP'nin Discover, Offer, Request ve ACK aşamalarından oluşan DORA sürecini öğrendim.
- DHCP problemi yaşandığında problemi sistematik olarak incelemenin önemini gördüm.
- DHCP yapılandırmasındaki network komutunun DHCP'nin doğru subnet üzerinden IP dağıtması için gerekli olduğunu öğrendim.

## Kullanılan Komutlar

enable

configure terminal

interface gigabitEthernet 0/0

ip address 192.168.10.1 255.255.255.0

no shutdown

ip dhcp excluded-address 192.168.10.1


ip dhcp pool LAN

network 192.168.10.0 255.255.255.0

default-router 192.168.10.1

dns-server 8.8.8.8

show ip interface brief

show ip dhcp pool

show ip dhcp binding

show running-config | section dhcp

PC Komutları:

ipconfig

ipconfig /all

ping 192.168.10.1

## Kullanılan Araçlar

- Cisco Packet Tracer
