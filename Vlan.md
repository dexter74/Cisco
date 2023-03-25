---------------------------------------------------------------------------------------------------------------------------------------------------
# <p align='center'> Création des Vlan 10 et 20 avec un Routeur </p>


---------------------------------------------------------------------------------------------------------------------------------------------------
## A. Présentation du Réseau
```
CIDR par défaut: 192.168.1.0/24
```

```
[Réseau] Nombre de SR : 2^2 =  4
[Hôte]   Nombre d'IP  : 2^6 = 64 (62 Hôtes)
```

|  ID | Adresse Réseau | Adresse IP de début | Adresse IP de Fin | Adresse de diffusion |
| --- | -------------- | ------------------- | ----------------- | -------------------- |  
|  1  | 192.168.1.0    | 192.168.1.1         | 192.168.1.62      | 192.168.1.63         |
|  2  | 192.168.1.64   | 192.168.1.65        | 192.168.1.126     | 192.168.1.127        |
|  3  | 192.168.1.128  | 192.168.1.129       | 192.168.1.190     | 192.168.1.191        |
|  4  | 192.168.1.192  | 192.168.1.193       | 192.168.1.254     | 192.168.1.255        |

L'adresse CIDR est 192.168.1.0/26 car on à consommée 2 Bits.


---------------------------------------------------------------------------------------------------------------------------------------------------
## B. Configuration de Packet Tracer
#### Always show label
```
Aller dans Options > Préférences
Always show Port labels in Logical Workspace
```

<br />

---------------------------------------------------------------------------------------------------------------------------------------------------
## C. Ajout du Matériel
```
Switch0 : 2950T
Switch1 : 2950T
Router0 : 2811

PC10-1  : Vlan 10
PC10-2  : Vlan 10
PC10-3  : Vlan 20

PC20-1  : Vlan 20
PC20-2  : Vlan 20
PC20-3  : Vlan 10

Laptop0: Vlan 99
```

<br />

---------------------------------------------------------------------------------------------------------------------------------------------------
## D. Relier le matériel
```
#Switch0-Router0 
Switch0 (Fa 0/1) - Switch1 (Fa 0/1)
Router0 (Fa 0/0) - Switch0 (Fa 0/2)

#Switch0
PC10-1  (Fa 0/0) - Switch0 (Fa 0/3)
PC10-2  (Fa 0/0) - Switch0 (Fa 0/4)
PC20-1  (Fa 0/0) - Switch0 (Fa 0/5)
PC20-2  (Fa 0/0) - Switch0 (Fa 0/6)

#Switch 1
PC10-3  (Fa 0/0) - Switch1 (Fa 0/2)
PC20-3  (Fa 0/0) - Switch1 (Fa 0/3)
Laptop0 (Fa 0)   - Switch1 (Fa 0/4) 
```

<br />

---------------------------------------------------------------------------------------------------------------------------------------------------
## X. Configuration des poste clients
```
PC10-1: 192.168.1.2 | 192.168.1.1
PC10-2: 192.168.1.3 | 192.168.1.1
PC10-3: 192.168.1.4 | 192.168.1.1

PC10-1: 192.168.1.65 | 192.168.1.1
PC10-2: 192.168.1.66 | 192.168.1.1
PC10-3: 192.168.1.67 | 192.168.1.1
```

<br />

---------------------------------------------------------------------------------------------------------------------------------------------------
## E. Configuration du Switch0

#### Raccourcis
```
CTRL+Z :Retour Privilège
```

#### Mode privilège
```
enable
```

#### Configuration Global
```
configure terminal
```

### Afficher la configuration A chaud et au démarrage
```
do show startup-config
do show running-config
```

### Afficher le résumé des interfaces
```
do show ip interface brief
```

### Afficher la configuration vlan
```
do show vlan
show vlan id 10
show vlan id 20
```

#### Protéger le routeur
```
enable secret admin
```

### Définir un Nom
```
hostname Switch0
```

### Définir une Bannière
```
banner motd login %
Vous tentez d’entrer sur mon $(hostname) %
l
```

### Supprimer Vlan X
```
no vlan 10
no vlan 20
```

### Création des Vlan 10 et 20
```
vlan 10
name Vlan10
exit

vlan 20
name Vlan20
exit
```

### Relier les interfaces au Vlans (Switch - PC)
```
interface FastEthernet 0/3
switchport access vlan 10
exit

interface FastEthernet 0/4
switchport access vlan 10
exit


interface FastEthernet 0/5
switchport access vlan 20
exit

interface FastEthernet 0/6
switchport access vlan 20
exit
```

#### Faire passer plusieurs VLAN sur le même Câble
Le trunk permet de faire passer plusieurs VLAN sur le même Câble
```
interface FastEthernet 0/1
switchport mode trunk
exit
```

### Faire passer les VLAN sur ce Switch
```
interface FastEthernet 0/1
switchport trunk allow vlan 10-20
exit
```

<br />

---------------------------------------------------------------------------------------------------------------------------------------------------
## F. Configuration du Switch1
#### Mode privilège
```
enable
```
#### Configuration Global
```
configure terminal
```
#### Protéger le routeur
```
enable secret admin
```
### Définir un Nom
```
hostname Switch1
```
### Définir une Bannière
```
banner motd login %
Vous tentez d’entrer sur mon $(hostname) %
l
```
### Supprimer Vlan X
```
no vlan 10
no vlan 20
```
### Création des Vlan 10 et 20
```
vlan 10
name Vlan10
exit

vlan 20
name Vlan20
exit
```
### Relier les interfaces au Vlans (Switch - PC)
```
interface FastEthernet 0/2
switchport access vlan 10
exit

interface FastEthernet 0/3
switchport access vlan 20
exit
```

#### Faire passer plusieurs VLAN sur le même Câble
Le trunk permet de faire passer plusieurs VLAN sur le même Câble
```
interface FastEthernet 0/1
switchport mode trunk
exit
```

### Faire passer les VLAN sur ce Switch
```
interface FastEthernet 0/1
switchport trunk allow vlan 10-20
exit
```

<br />

---------------------------------------------------------------------------------------------------------------------------------------------------
## G. L'administration des VLAN
### Configurer le VLan 99 sur le Switch1
```
enable
configure terminal
no vlan 99
vlan 99
name Administration
exit

interface FastEthernet 0/4
switchport access vlan 99
exit
```






---------------------------------------------------------------------------------------------------------------------------------------------------


### Configuration de l'adresse IP
```
ip address 192.168.1.254 255.255.255.0
```



### Sauvegarder la configuration de travaille dans le démarrage
```
write memory
```

#### Fusion de la configuration de Travaille et démarrage
``` 
copy running-config startup-config
```

