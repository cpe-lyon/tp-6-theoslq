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
3. 
4. 
5. 
6. 
7. 
8. 
9. 
10. 
11. 

**Exercice 4. Donner un accès à Internet au client**

1.
2.

**Exercice 5. Installation du serveur**

1.
2.
3.
4.

**Exercice 6. Configuration du serveur DNS pour la zone tpadmin.local**

1.
2.
3.
4.
5.

**Exercice 7. Installation d'un serveur web**

**Exercice 8. Installation d'un serveur de temps**
