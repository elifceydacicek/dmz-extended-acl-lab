# DMZ ve Genişletilmiş ACL (Extended ACL) Ağ Güvenliği Laboratuvarı

Cisco Packet Tracer üzerinde oluşturulan bu laboratuvarda ağa DMZ bölgesi eklenmiş, DMZ'den iç ağa (192.168.1.0/24) erişim `DMZ-IN` kuralıyla engellenmiş ve dış ağdan (209.165.200.0/24) DMZ sunucusuna (172.16.1.10) erişim `100` numaralı Genişletilmiş ACL üzerinden yapılandırılıp test edilmiştir.

## Ağ ve Güvenlik Özeti

* **DMZ Arayüzü ve Sunucusu:** `Router0` üzerindeki `GigabitEthernet0/2` (172.16.1.1/24) arayüzü aktif hale getirilmiş ve `Switch1` üzerinden 172.16.1.10 IP adresli DMZ sunucusuna bağlantı sağlanmıştır.
* **İç Ağ İzolasyonu (`DMZ-IN`):** DMZ ağından (172.16.1.0/24) iç ağa (192.168.1.0/24) giden trafiği engellemek için `DMZ-IN` kuralı tanımlanmış ve `g0/2` arayüzüne `inbound` yönünde uygulanmıştır.
* **Dış Ağdan DMZ Erişimi (`ACL 100`):** `100` numaralı Genişletilmiş ACL listesinde `deny ip any any` kuralının öncesine `permit ip 209.165.200.0 0.0.0.255 host 172.16.1.10` kuralı eklenmiş ve yapılandırma `write memory` ile kaydedilmiştir.

## Yapılan Testler ve Doğrulamalar

1. **DMZ -> İç Ağ Engelleme Testi:** DMZ sunucusundan 192.168.1.10 adresine yapılan `ping` istekleri engellenmiş (`Destination host unreachable` / `%100` paket kaybı) ve `show access-lists DMZ-IN` çıktısında eşleşme sayacı (`match(es)`) artmıştır.
2. **Dış Ağ -> DMZ Erişim Testi:** `Server0` (209.165.200.10) üzerinden 172.16.1.10 adresine yapılan `ping` başarılı olmuş ve `show access-lists 100` çıktısında ilgili `permit` kuralının sayacı (`match(es)`) güncellenmiştir.
3. **Simülasyon Modu (PDU) İzleme:** `Server0` ile DMZ sunucusu arasındaki ICMP paketinin `Router0` ve `Switch1` üzerinden gidip geri döndüğü zarf akışıyla doğrulanmıştır.

## Dosyalar

* `dmz-extended-acl-lab.pkt`: Cisco Packet Tracer topoloji ve yapılandırma dosyası.
