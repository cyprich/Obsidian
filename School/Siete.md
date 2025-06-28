# Siete

Take vseobecne srandicky o sietach co som nevedel kam zaradit, alebo som zvazil ze by bolo fajn ich mat pokope  

Poznamocky v suboroch [PS1](./PS1.md) a [PS2](./PS2.md)

## Basic config

Defaultna Cisco konfiguracia ktora by mala byt vykonana vzdy pri novom zariadeni

```
hostname R1
banner motd "authorized access only!"
line console 0
    logging sync
no ip domain lookup
```

```
username student secret student
username admin privilege 15 secret admin
line console 0
    login local
line vty 0 15
    login local
service password-encryption
enable secret class
```

```
ip domain-name B303.sk
crypto key gen rsa modulus 1024
ip ssh version 2
line vty 0 15
    transport input ssh
```

## Cisla portov

| Port | Sluzba               | TCP alebo UPD? |
| ---- | -------------------- | -------------- |
| 20   | FTP (data)           | T              |
| 21   | FTP (control)        | T              |
| 22   | SSH                  | T              |
| 23   | Telnet               | T              |
| 53   | DNS                  | T & U          |
| 67   | DHCP (client)        | U              |
| 68   | DHCP (server)        | U              |
| 69   | TFTP                 | T              |
| 80   | HTTP                 | T              |
| 123  | NTP                  | U              |
| 443  | HTTPS                | T              |
| 514  | Syslog               | U              |
| 520  | RIP                  | U              |
| 546  | DHCPv6 (client)      | U              |
| 547  | DHCPv6 (server)      | U              |
| 1812 | RADIUS auth          | U              |
| 1813 | RADIUS accounting    | U              |
| 1985 | HSRPv1               | U              |
| 3455 | RSVP                 | T & U          |
| 5246 | CAPWAP (source)      | U              |
| 5247 | CAPWAP (destination) | U              |
| 8291 | WinBox               | T              |

## Multicast adresy

| Adresa IPv4  | Adresa IPv6 | Urcena pre                               |
| ------------ | ----------- | ---------------------------------------- |
| `224.0.0.10` | `FF02::10`  | EIGRP                                    |
| `224.0.0.5`  | `FF02::5`   | OSPF - vsetky smerovace v danom segmente |
| `224.0.0.6`  | `FF02::6`   | OSPF - DR/BDR smerovace v danom segmente |

## Administrative distances routing protokolov

| Protokol                      |  AD |
| ----------------------------- | --: |
| Priamo pripojena siet         |   0 |
| Staticka siet                 |   1 |
| EIGRP sumarna siet            |   5 |
| BGP z ineho AS                |  20 |
| EIGRP interna siet            |  90 |
| OSPF                          | 110 |
| IS-IS                         | 115 |
| RIP                           | 120 |
| On-Demand Routing (ODR)       | 160 |
| BGP z toho isteho AS          | 200 |
| DHCP                          | 254 |
| Absolutne nedoveryhodny zdroj | 255 |

## RFC

| Cislo RFC     | Popis   |
| ------------- | ------- |
| 1157          | SNMPv1  |
| 1305          | NTP     |
| 1901 - 1908   | SNMPv2c |
| 2328 a dalsie | OSPF    |
| 2273 - 2275   | SNMPv3  |
| 3164          | Syslog  |
