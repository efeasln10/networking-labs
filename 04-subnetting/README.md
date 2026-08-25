# Lab 4 - IPv4 Subnetting

## Amaç

IPv4 subnetting mantığını anlamak ve aşağıdaki değerleri
hesaplayabilmek:

- Network address
- First usable host
- Last usable host
- Broadcast address
- Kullanılabilir host sayısı
- CIDR gösterimi
- Subnet mask

## Örnek Network

192.168.10.0/24

Bu network /26 subnetlerine bölündü.

## Subnet 1

Network Address:
192.168.10.0

CIDR:
192.168.10.0/26

Subnet Mask:
255.255.255.192

First Host:
192.168.10.1

Last Host:
192.168.10.62

Broadcast:
192.168.10.63

Kullanılabilir Host:
62

## Subnet 2

Network Address:
192.168.10.64

CIDR:
192.168.10.64/26

Subnet Mask:
255.255.255.192

First Host:
192.168.10.65

Last Host:
192.168.10.126

Broadcast:
192.168.10.127

Kullanılabilir Host:
62

## Subnet 3

Network Address:
192.168.10.128

First Host:
192.168.10.129

Last Host:
192.168.10.190

Broadcast:
192.168.10.191

## Subnet 4

Network Address:
192.168.10.192

First Host:
192.168.10.193

Last Host:
192.168.10.254

Broadcast:
192.168.10.255

## Önemli Kavramlar

Bir /26 subnet:

2^6 = 64 toplam adres

64 - 2 = 62 kullanılabilir host adresi

Kullanılamayan iki adres:

- Network address
- Broadcast address

## İletişim Testi

İki bilgisayar farklı /26 subnetlerine yerleştirildi:

PC1:
192.168.10.10/26

PC2:
192.168.10.70/26

Her iki bilgisayar aynı switch'e bağlı olmasına rağmen,
farklı IP subnetlerinde oldukları ve aralarında router
bulunmadığı için iletişim kuramadılar.

## Öğrendiklerim

- CIDR gösteriminin subnet mask'i nasıl ifade ettiğini öğrendim.
- Subnet aralıklarının nasıl hesaplandığını öğrendim.
- Network ve broadcast adreslerinin nasıl çalıştığını öğrendim.
- Farklı subnetlerin iletişim kurabilmesi için neden Layer 3
  cihazına ihtiyaç duyulduğunu öğrendim.
- Fiziksel bağlantı ile Layer 3 bağlantısı arasındaki farkı gördüm.

## Kullanılan Araçlar

- Cisco Packet Tracer
