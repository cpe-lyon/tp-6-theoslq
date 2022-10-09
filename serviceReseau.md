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

```
7. 
```console

```
8. 
```console

```
9. 
```console

```
10. 
```console

```
**Exercice 4. Donner un accès à Internet au client**  

1.
```console

```
2.
```console

```
**Exercice 5. Installation du serveur**  

1.
```console

```
2.
```console

```
3.
```console

```
4.
```console

```
**Exercice 6. Configuration du serveur DNS pour la zone tpadmin.local**  

1.
```console

```
2.
```console

```
3.
```console

```
4.
```console

```
5.
```console

```
