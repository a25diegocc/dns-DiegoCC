# Tarea 1

## 1- Primer Apartado

---

; <<>> DiG 9.20.29-1~deb13u1-Debian <<>> xunta.gal
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 59914
;; flags: qr rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 0, ADDITIONAL: 0

;; QUESTION SECTION:
;xunta.gal.                     IN      A

;; ANSWER SECTION:
xunta.gal.              28800   IN      A       85.91.64.109

;; Query time: 40 msec
;; SERVER: 127.0.0.11#53(127.0.0.11) (UDP)
;; WHEN: Wed Oct 07 15:39:37 UTC 2026
;; MSG SIZE  rcvd: 43

---

## 2- Segundo apartado

### A) Fichero /etc/bind/named.conf.options

---

options {
	directory "/var/cache/bind";

	// If there is a firewall between you and nameservers you want
	// to talk to, you may need to fix the firewall to allow multiple
	// ports to talk.  See http://www.kb.cert.org/vuls/id/800113

	// If your ISP provided one or more IP addresses for stable 
	// nameservers, you probably want to use them as forwarders.  
	// Uncomment the following block, and insert the addresses replacing 
	// the all-0s placeholder.

	forwarders {
	    192.168.20.10;
	};

	//========================================================================
	// If BIND logs error messages about the root key being expired,
	// you will need to update your keys.  See https://www.isc.org/bind-keys
	//========================================================================
	dnssec-validation auto;

	auth-nxdomain no;    # conform to RFC1035
	listen-on-v6 { any; };
};

---

### B) Saida do comando dig @localhost santiagodecompostela.gal

---

; <<>> DiG 9.20.29-1~deb13u1-Debian <<>> santiagodecompostela.gal
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 16670
;; flags: qr rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 0, ADDITIONAL: 0

;; QUESTION SECTION:
;santiagodecompostela.gal.      IN      A

;; ANSWER SECTION:
santiagodecompostela.gal. 300   IN      A       195.57.25.148

;; Query time: 28 msec
;; SERVER: 127.0.0.11#53(127.0.0.11) (UDP)
;; WHEN: Wed Oct 07 15:41:56 UTC 2026
;; MSG SIZE  rcvd: 58

---

## 3- Tercer apartado

---

### A) Arquivo de zona

---

$TTL    86400
@       IN      SOA     darthvader.starwars.lan. admin.starwars.lan. (
                        2025100701  ; Número de serie
                        3600        ; Actualización (Refresh)
                        1800        ; Reintento (Retry)
                        1209600     ; Caducidade (Expire)
                        86400 )     ; TTL mínimo

; Servidores de nomes
@       IN      NS      darthvader.starwars.lan.
@       IN      NS      darthsidious.starwars.lan.

; Rexistros A dos servidores de nomes
darthvader     IN      A       192.168.20.10
skywalker      IN      A       192.168.20.12
skywalker      IN      A       192.168.20.111
luke           IN      A       192.168.20.22
darthsidious   IN      A       192.168.20.11
yoda           IN      A       192.168.20.24
yoda           IN      A       192.168.20.25
c3p0           IN      A       192.168.20.26 
palpatine      IN      CNAME   darthsidious.starwars.lan.
@              IN   MX  10   c3p0.starwars.lan.
lenda          IN   TXT     "Que a forza te acompañe"

---

### B) Contido arquivo /etc/bind/named.conf.local

---

zone "starwars.lan"{
    type master;
    file "/etc/bind/db.starwars.lan";
};
zone "db.20.168.192.in-addr.arpa"{
    type primary;
    file "/etc/bind/db.192";  
};

---

## 4-Cuarto apartado

---

### A) Contido zona inversa

---

$TTL    86400
@       IN      SOA     darthvader.starwars.lan. admin.starwars.lan. (
                        2025100701  ; Número de serie
                        3600        ; Actualización (Refresh)
                        1800        ; Reintento (Retry)
                        1209600     ; Caducidade (Expire)
                        86400 )     ; TTL mínimo

; Servidores de nomes
@       IN      NS      darthvader.starwars.lan.
; Rexistros A dos servidores de nomes
10      IN      PTR     darthsidious.starwars.lan.
12      IN      PTR     skywalker.starwars.lan.
111      IN      PTR    skywalker.starwars.lan.
22      IN      PTR     luke.starwars.lan.
24      IN      PTR     yoda.starwars.lan.
25      IN      PTR     yoda.starwars.lan.
26      IN      PTR     c3p0.starwars.lan.

---

## 5- Qunto apartado

---

### A) nslookup darthvader.starwars.lan localhost



Server:         localhost
Address:        127.0.0.1#53

Name:   darthvader.starwars.lan
Address: 192.168.20.10

### B) nslookup skywalker.starwars.lan localhost

Server:         localhost
Address:        127.0.0.1#53

Name:   skywalker.starwars.lan
Address: 192.168.20.111
Name:   skywalker.starwars.lan
Address: 192.168.20.101

### C) nslookup starwars.lan localhost

Server:		127.0.0.11
Address:	127.0.0.11#53

Name:	starwars.lan
Address: 192.168.1.10


### D) nslookup -q=mx starwars.lan localhost

Server:         localhost
Address:        127.0.0.1#53

starwars.lan    mail exchanger = 10 c3p0.starwars.lan.

### E) nslookup -q=ns starwars.lan localhost

Server:         localhost
Address:        127.0.0.1#53

starwars.lan    nameserver = darthsidious.starwars.lan.
starwars.lan    nameserver = darthvader.starwars.lan.

### F) nslookup -q=soa starwars.lan localhost

Server:         localhost
Address:        127.0.0.1#53

starwars.lan
        origin = darthvader.starwars.lan
        mail addr = admin.starwars.lan
        serial = 2
        refresh = 604800
        retry = 86400
        expire = 2419200
        minimum = 604800

### G) nslookup -q=txt lenda.starwars.lan localhost

Server:         localhost
Address:        127.0.0.1#53

starwars.lan
        origin = darthvader.starwars.lan
        mail addr = admin.starwars.lan
        serial = 2
        refresh = 604800
        retry = 86400
        expire = 2419200
        minimum = 604800

root@darthvader:/var/cache/bind# nslookup -q=txt lenda.starwars.lan localhost
Server:         localhost
Address:        127.0.0.1#53

lenda.starwars.lan      text = "Que a forza te acompanhe"

### H) nslookup 192.168.20.11 localhost
 
Server: localhost
Address: 127.0.0.1#53

lenda.starwars.lan text = "Que a forza te acompanhe"

root@darthvader:/var/cache/bind# nslookup 192.168.20.11 localhost
11.20.168.192.in-addr.arpa name = darthsidious.starwars.lan.