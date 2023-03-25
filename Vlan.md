---------------------------------------------------------------------------------------------------------------------------------------------------
# <p align='center'> Création des Vlan 10 et 20 avec un Routeur </p>


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
Router0 : 2811
Switch0 : 2950T
Switch1 : 2950T

PC10-1  : VLAN 10
PC10-2  : VLAN 10
PC10-3  : VLAN 20

PC20-1  : VLAN 20
PC20-2  : VLAN 20
PC20-3  : VLAN 10

Laptop0: VLAN 99
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
PC10-1  : 192.168.1.2 | 192.168.1.1
PC10-2  : 192.168.1.3 | 192.168.1.1
PC10-3  : 192.168.1.4 | 192.168.1.1

PC20-1  : 192.168.1.5 | 192.168.1.1
PC20-2  : 192.168.1.6 | 192.168.1.1
PC20-3  : 192.168.1.7 | 192.168.1.1

Laptop0 :
```

<br />

---------------------------------------------------------------------------------------------------------------------------------------------------
## E. Configuration du Switch0
```
enable
configure terminal

no vlan 10
vlan 10
name Utilisateur
exit

no vlan 20
vlan 20
name Invite
exit

no vlan 99
vlan 99
name Administrateur
exit

interface range FastEthernet 0/1 - 2
switchport mode trunk
exit
interface range FastEthernet 0/3 - 4
switchport mode access
switchport access vlan 10
no shutdown
exit
interface range FastEthernet 0/5 - 6
switchport mode access
switchport access vlan 20
no shutdown
exit
interface vlan 99
ip address 192.168.1.100 255.255.255.0
no shutdown
exit

vtp domain Switch0
vtp mode server
vtp version 2
```
<br />

---------------------------------------------------------------------------------------------------------------------------------------------------
## F. Configuration du Switch1

<br />

---------------------------------------------------------------------------------------------------------------------------------------------------
## G. Configuration du Router0

<br />

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

#### Raccourcis
```
CTRL+Z :Retour Privilège
```

