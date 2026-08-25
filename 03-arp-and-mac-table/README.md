# Lab 3 - ARP ve MAC Address Table

## Amaç

ARP tablosu ile switch MAC address table arasındaki farkı anlamak
ve bu tabloların nasıl dinamik olarak oluşturulduğunu gözlemlemek.

## Topoloji

PC1 ─── Switch ─── PC2

## Deneyler

### ARP Tablosu

İletişim öncesinde ve sonrasında ARP tablosu incelendi.

Başlangıçta PC2'nin MAC adresi ARP tablosunda bulunuyordu.

ARP tablosu temizlendikten sonra herhangi bir ARP kaydı bulunmadığı
gözlemlendi.

PC2'ye ping gönderildikten sonra PC2'nin MAC adresi tekrar ARP
tablosunda göründü.

### MAC Address Table

Switch'in MAC address table'ı temizlendi ve ardından tekrar
incelendi.

Trafik oluşturulduktan sonra switch'in MAC adreslerini dinamik
olarak öğrendiği gözlemlendi.

Örnek:

PC1 → Fa0/1

PC2 → Fa0/2

## Kullanılan Komutlar

```text
arp -a
arp -d
show mac address-table
