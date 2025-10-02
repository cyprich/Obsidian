# PS3

Pocitacove Siete 3

## Uvod

Predmet zamerany na reatime multimedialnu (najma hlasovu) komunikaciu nad uz existujucou sietou  
Konvergovana siet  
"Close to realtime"  
Protokol **SIP**  
Komunikator - program (?) ktory umoznuje komunikaciu  
RTT max 250 ms

## DNS a system ENUM

### Doman Name System

_"Nieco na styl telefonneho zoznamu pre internet"_

RFC 1034 and 1035

Port 53

- TCP ak sprava >512 B, alebo sa prenasaju cele zony
- UDP v ostatnych pripadoch

DNS over TLS (DoT)

- RFC 7858, 8310
- TCP 853
- Podporovane servery - `1.1.1.1`, `9.9.9.9`

DNS over HTTPS

- RFC 8484

DNS je hierarchicka struktura nazvov, ktora vyjadruje ich prislusnost do istej oblasti

- Krajina, stat, vlastnik, interna struktura vlastnika
- Napr. `sk` - `uniza.sk` - `fri.uniza.sk` - `kis.fri.uniza.sk`

Zakladne komponenety DNS

- Menne (nazvove) servery (name servers)
  - Zdrojove zaznamy = **resource records**
  - Server zodpoveda za zonu
  - Server ktory spravuje zonu sa nazyva **autoritativny**
- Resolver
  - Stub - hlupy - uplne spolieha na server a ocakava definitivnu odpoved
  - Rekurzivny - nehlupy - spracuje aj ciastocnu odpoved, ak ho server "odkaze" na iny server
- Kazdy uzol ma svoje vlastne meno (hostname) a patri do istej domeny
  - hostname + domena = Fully qualified domain name (FQDN)
  - Komponenty FQDN sa oddeluju bodkou a volaju sa _labels_ - max 63 znakov dlhy, `[a-z][A-Z][0-9]`, standardne len ASCII znaky, da sa aj inak
  - FQDN celkovo max 255 znakov
- Zdrojovy zaznam - resource record
  - Oznacuje prvok databazy v DNS
  - Casti
    - vlastnik
    - typ
    - trieda
    - ttl
    - rdata
  - Typy
    - SOA - Start of Authority - popis zony, musi byt prave raz - domenove meno, identita spravcu, seriove cislo, casoe udaje pre sync
    - A - IPv4
    - AAAA - IPv6
    - NS - Name Server - FQDN autoritativneho DNS servera danej zony
    - CNAME - canonical name - alias
    - MX - Main Exchanger - postovy server
    - PTR - Pointer - reverzny lookup
    - SRV - Service
    - NAPTR - Naming Authority Pointer
  - Trieda
    - IN - Internet - jedina pouzivana dnes
    - CH - Chaosnet - experimentalna siet na MIT
    - HS - Hesiod - tiez uz nepouzivane
- Zony
  - Podstrom domenoveho stromu
  - Ako root celeho stormu je `.` (bodka)
  - Napr. `sk` oddeli `telecom.sk` nech si spravuju sami, `uniza.sk`, `edu.sk`, ...
