**Services Réseau**

**Exercice 1. Adresse IP (rappels)**  
Réseau interne : 172.16.0.0/23, on utilise  du VLSM afin de ne pas gaspiller des adresses IP.
Numéro du Sous-réseaux | Adresse de sous-réseau | Adresse de broadcast | Adresse première machine | Adresse dernière machine
------------ | ------------- | ------------ | ------------ | ------------
Sous-réseau 3 : 52 machines | 172.16.0.0     /26 | 172.16.0.63 | 172.16.0.1 | 172.16.0.62
Sous-réseau 1 : 38 machines | 172.16.0.64    /26 | 172.16.0.127 | 172.16.0.65 | 172.16.0.126
Sous-réseau 6 : 37 machines | 172.16.0.128   /26 | 172.16.0.191 | 172.16.0.129 | 172.16.0.190
Sous-réseau 4 : 35 machines | 172.16.0.192   /26 | 172.16.0.255 | 172.16.0.193 | 172.16.0.254
Sous-réseau 5 : 34 machines | 172.16.1.0     /26 | 172.16.1.63 | 172.16.1.1 | 172.16.1.63
Sous-réseau 2 : 33 machines | 172.16.1.64    /26 | 172.16.1.127 | 172.16.1.65 | 172.16.1.126
Sous-réseau 7 : 25 machines | 172.16.1.128   /27 | 172.16.1.159 | 172.16.1.129 | 172.16.1.158

**Exercice 2. Préparation de l'environnement**  

1. 
![image](https://user-images.githubusercontent.com/97438358/193532644-1de9721c-90c2-4dc7-a73a-f09cf76b754a.png)
![image](https://user-images.githubusercontent.com/97438358/193533893-ddac7d8a-b502-4ad9-bb0e-770c09e44dc9.png)  
2.
L'interface lo correspond à loopback, boucle locale.  
![image](https://user-images.githubusercontent.com/97438358/193534322-179c95ad-057b-454e-8ca1-94e103ac914d.png)  
3.
```console
User@localhost:~$ sudo apt remove cloud-init
```  
4.
```console
User@localhost:~$ sudo hostnamectl set-hostname serveur
```
![image](https://user-images.githubusercontent.com/97438358/193543933-4fb36568-5ca9-421c-b667-cbbac7757021.png)  
**Exercice 3. Installation DHCP**

1. 
```console
User@serveur:~$ sudo apt install isc-dhcp-server
User@serveur:~$ systemctl status isc-dhcp-server
```
2.
```console
User@serveur:~$ sudo nano /etc/netplan/50-cloud-init.yaml
network:
  ethernets:
      ens192:
          dhcp4: true
          match:
              macaddress: 00:50:56:89:3a:1d
          set-name: ens192
       ens224:
           dhcp4: true
           addresses: [192.168.100.1/24]
           nameservers:
               addresses: [8.8.8.8, 1.1.1.1]
  version: 2
```
3. 
```console
User@serveur:~$ sudo cp dhcpd.conf dhcpd.conf.bak
default-lease-time : temps par défaut
max-lease-time 600 : temps maximal du bail (date de fin de validité)
```
4. 
```console
User@serveur:~$ sudo nano /etc/default/isc-dhcp-server
```
<img width="128" alt="image" src="https://user-images.githubusercontent.com/97438358/194771389-bf2949c4-119b-406f-8995-5966a35b35d4.png">  

5. 
```console
User@serveur:~$ dhcpd -t
User@serveur:~$ sudo systemctl restart isc-dhcp-server
```
6. 
```console
Sur la VM client :
User@client:~$ sudo apt remove cloud-init
User@client sudo hostnamectl set-hostname client.tpadmin.local
User@client sudo nano /etc/hosts
```
<img width="731" alt="image" src="https://user-images.githubusercontent.com/97438358/194774096-af808f77-bee5-4464-8854-91ea06262b79.png">  

7. 
```console
DHCP REQUEST = Client demande adresse IP au Serveur
DHC PACK = Confirme reception de la demande au Serveur
DHCP DISCOVER = Serveur attribut IP au Client
DHC POFFER = Réponse Serveur au Client
```
8. 
```console
/var/lib/dhcp/dhcpd.leases : Demandes bails
dhcp-lease-list : Liste bails, et leur expiration
```
9. 
```console
PING 192.168.100.100 (192.168.100.100) 56(84) bytes of data.
64 bytes from 192.168.100.100: icmp_seq=1 ttl=64 time=0.235 ms
64 bytes from 192.168.100.100: icmp_seq=2 ttl=64 time=0.245 ms
64 bytes from 192.168.100.100: icmp_seq=3 ttl=64 time=0.237 ms
64 bytes from 192.168.100.100: icmp_seq=4 ttl=64 time=0.268 ms
64 bytes from 192.168.100.100: icmp_seq=4 ttl=64 time=0.253 ms
64 bytes from 192.168.100.100: icmp_seq=4 ttl=64 time=0.241 ms
etc...
```
10. 
```console
L'interface est bien modifiée
```
**Exercice 4. Donner un accès à Internet au client**  

1.
```console
User@serveur:~$ sudo nano /etc/sysctl.conf
net.ipv4.ip_forward=1
User@serveur:~$ sudo sysctl net.ipv4.ip_forward
net.ipv4.ip_forward=1
```
2.
```console
User@serveur:~$ sudo iptables --table nat --append POSTROUTING --out-interface ens192 -j MASQUERADE

PING 1.1.1.1 (1.1.1.1) 56(84) bytes of data.
64 bytes from 1.1.1.1: icmp_seq=1 ttl=53 time=9.24 ms
64 bytes from 1.1.1.1: icmp_seq=2 ttl=53 time=9.37 ms
64 bytes from 1.1.1.1: icmp_seq=3 ttl=53 time=8.73 ms
64 bytes from 1.1.1.1: icmp_seq=3 ttl=53 time=8.84 ms
64 bytes from 1.1.1.1: icmp_seq=3 ttl=53 time=8.91 ms
etc...
```
**Exercice 5. Installation du serveur**  

1.
```console
User@serveur:~$ apt install bind9
```
2.
```console
User@serveur:~$ sudo nano /etc/bind/named.conf.options
```
<img width="511" alt="image" src="https://user-images.githubusercontent.com/97438358/194774872-b91cb3aa-74d1-4bb7-8f06-8a94c8deec58.png">  

3.
```console
PING www.google.fr (216.58.209.227) 56(84) bytes of data.
64 bytes from par10s29-in-f3.1e100.net (216.58.209.227): icmp_seq=1 ttl=111 time=8.57 ms
64 bytes from par10s29-in-f3.1e100.net (216.58.209.227): icmp_seq=2 ttl=111 time=8.62 ms
64 bytes from par10s29-in-f3.1e100.net (216.58.209.227): icmp_seq=3 ttl=111 time=8.89 ms
64 bytes from par10s29-in-f3.1e100.net (216.58.209.227): icmp_seq=4 ttl=111 time=8.56 ms
64 bytes from par10s29-in-f3.1e100.net (216.58.209.227): icmp_seq=3 ttl=111 time=8.73 ms
64 bytes from par10s29-in-f3.1e100.net (216.58.209.227): icmp_seq=4 ttl=111 time=8.61 ms
```
4.
```console
User@serveur:~$ lynx fr.wikipedia.fr
```
**Exercice 6. Configuration du serveur DNS pour la zone tpadmin.local**  

1.
```console
User@serveur:~$ sudo nano /etc/bind/named.conf.local 
```
<img width="550" alt="image" src="https://user-images.githubusercontent.com/97438358/194775085-e454eae1-3b40-472d-a70d-3daa51743f42.png">  
2.
```console
User@serveur:~$ sudo nano /etc/bind/db.local
```
<img width="556" alt="image" src="https://user-images.githubusercontent.com/97438358/194775382-2f034e5e-3f87-46b6-b624-fe18dcb31a4d.png">  

3.
```console
User@serveur:~$ sudo nano /etc/bind/named.conf.local
```
<img width="542" alt="image" src="https://user-images.githubusercontent.com/97438358/194775528-ed8d397f-0f1f-4b05-b6c8-0b55493e1b56.png">  

```console
User@serveur:~$ cp /etc/bind/db.127 db.192
User@serveur:~$ sudo nano db.192
```
<img width="540" alt="image" src="https://user-images.githubusercontent.com/97438358/194775732-f4f1ca21-f79d-4048-9a5a-c24a45df1662.png">  

4.
```console
User@serveur:~/etc/bind$ named-checkconf named.conf.local
User@serveur:~/etc/bind$ named-checkzone tpadmin.local /etc/bind/db.tpadmin.local
zone tpadmin.local/IN: loaded serial 2
OK
User@serveur:~/etc/bind$ named-checkzone 100.168.192.in-addr.arpa /etc/bind/db.192.168.100
zone 100.168.192.in-addr.arpa/IN: loaded serial 1
OK
```
5.
```console
PING serveur.tpadmin.local (192.168.100.1) 56(84) bytes of data.
64 bytes from serveur.tpadmin.local (192.168.100.1): icmp_seq=1 ttl=64 time=0.348 ms
64 bytes from serveur.tpadmin.local (192.168.100.1): icmp_seq=2 ttl=64 time=0.313 ms
64 bytes from serveur.tpadmin.local (192.168.100.1): icmp_seq=3 ttl=64 time=0.456 ms
64 bytes from serveur.tpadmin.local (192.168.100.1): icmp_seq=4 ttl=64 time=0.238 ms
64 bytes from serveur.tpadmin.local (192.168.100.1): icmp_seq=4 ttl=64 time=0.438 ms
64 bytes from serveur.tpadmin.local (192.168.100.1): icmp_seq=4 ttl=64 time=0.267 ms
```
