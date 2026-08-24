# Lab 2 - Default Gateway

## Konu

Default gateway'in kullanım amacını anlama ve cihazlar farklı ağlarda
olunca iletişimin nasıl değiştini gözlemleme.

## Topoloji

PC1 ─── Switch ─── Router
                         │
                         └── PC2

## IP Addressing

| Cihaz | IP Addresi | Subnet Mask | Default Gateway |
|---|---|---|---|
| PC1 | 192.168.10.10 | 255.255.255.0 | 192.168.10.1 |
| PC2 | 192.168.20.10 | 255.255.255.0 | 192.168.20.1 |

## Testler

### Test 1 - Aynı Ağ

Aynı alt ağdaki cihazlar iletişim kurabiliyordu.

### Test 2 - Farklı Ağ

PC1 ve PC2 farklı IP ağlarına yerleştirildi.

İletişim için bir router ve doğru yapılandırılmış bir default gateway gerekiyordu.

### Test 3 - Default Gateway'i Değiştirmek

The default gateway değiştirildi ve bağlantı tekrar test edildi.

## Ne Öğrendik?

- Yerel alt ağın dışındaki cihazlara ulaşmak için default gateway kullanılır.
- Router farklı IP ağlarını birbirine bağlar.
- Farklı alt ağlarda olan cihazlar Layer 3 cihazları olmadan doğrudan iletişime geçemez.
- Yanlış default gateway yapılandırılması farklı ağlarla iletişime engel olabilir.

## Araçlar

- Cisco Packet Tracer
