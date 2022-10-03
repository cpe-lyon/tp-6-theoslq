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
