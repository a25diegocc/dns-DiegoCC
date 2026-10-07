# Tarea 1

## Primer Pregunta


; <<>> DiG 9.18.39-0ubuntu0.24.04.5-Ubuntu <<>> xunta.gal
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 6411
;; flags: qr rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 0, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 4000
;; QUESTION SECTION:
;xunta.gal.			IN	A

;; ANSWER SECTION:
xunta.gal.		900	IN	A	85.91.64.109

;; Query time: 11 msec
;; SERVER: 10.0.4.1#53(10.0.4.1) (UDP)
;; WHEN: Wed Oct 07 11:42:23 CEST 2026
;; MSG SIZE  rcvd: 54

## Segundo apartado

### Fichero /etc/bind/named.conf.options

options {
    directory "/var/cache/bind";

    forwarders {
        192.168.20.10;
    };

    forward only;

    dnssec-validation auto;
    listen-on-v6 { any; };
};

---

### Saida do comando dig @localhost santiagodecompostela.gal