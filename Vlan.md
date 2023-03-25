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

#### Raccourcis
```
CTRL+Z :Retour Privilège
```

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
Switch1 : 2950T
Switch2 : 2950T

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
# Liaison entre Switch
Switch2 (Fa 0/1) - Switch1 (Fa 0/1)

#Switch 1
Router  (Fa 0/0) - Switch1 (Fa 0/2)
PC10-1  (Fa 0/0) - Switch1 (Fa 0/3)
PC10-2  (Fa 0/0) - Switch1 (Fa 0/4)
PC20-1  (Fa 0/0) - Switch1 (Fa 0/5)
PC20-2  (Fa 0/0) - Switch1 (Fa 0/6)


#Switch 2
PC10-3  (Fa 0/0) - Switch2 (Fa 0/2)
PC20-3  (Fa 0/0) - Switch2 (Fa 0/3)
Laptop0 (Fa 0)   - Switch2 (Fa 0/4) 
```

<br />

---------------------------------------------------------------------------------------------------------------------------------------------------
## X. Configuration des poste clients
```
PC10-1  : 192.168.1.X | 192.168.1.1
PC10-2  : 192.168.1.X | 192.168.1.1
PC10-3  : 192.168.1.X | 192.168.1.1

PC20-1  : 192.168.1.X | 192.168.1.1
PC20-2  : 192.168.1.X | 192.168.1.1
PC20-3  : 192.168.1.X | 192.168.1.1

Laptop0 :
```

<br />

---------------------------------------------------------------------------------------------------------------------------------------------------
## E. Configuration du Router
### I. Configuration de base
```
enable
configure terminal
hostname Router
```

### II. Création des Interfaces Virtuelles
```
interface FastEthernet 0/0.10
encapsulation dot1q 10
ip address 192.168.1.1 255.255.255.192
no shutdown
exit

interface FastEthernet 0/0.20
encapsulation dot1q 20
ip address 192.168.1.65 255.255.255.192
no shutdown
exit

interface FastEthernet 0/0.99
encapsulation dot1q 99
ip address 192.168.1.129 255.255.255.192
no shutdown
exit

end
```



<br />



---------------------------------------------------------------------------------------------------------------------------------------------------
## F. Configuration du Switch1
#### I. Mode configuration
```
enable
configure terminal
hostname Switch1
```

#### II. Création des VLAN (10, 20 et 99)
```
no vlan 10
vlan 10
name Utilisateur

no vlan 20
vlan 20
name Invite

no vlan 99
vlan 99
name Administrateur
exit
```

#### III. Attribution des VLANS à des interfaces Physiques (Ports)
```
interface range FastEthernet 0/3 - 4
switchport mode access
switchport access vlan 10
no shutdown
exit
```

```
interface range FastEthernet 0/5 - 6
switchport mode access
switchport access vlan 20
no shutdown
exit
```


#### IV. Trunk
Permettre le passage des Vlans entre équipements. (Ex: Switch, Routeurs)
Les Trunks autorisés sont:
```
- Routeur - Switch0
- Switch0 - Switch1
```

```
interface range FastEthernet 0/1 - 2
switchport mode trunk
exit
```

#### V. Configuration IP Virtuelle pour le Switch
```
interface vlan 99
ip address 192.168.1.100 255.255.255.0
no shutdown
exit
```

### VI. Activation de la diffusion de configuration (VTP)
```
vtp domain Switch0
vtp mode server
vtp version 2
```
<br />

---------------------------------------------------------------------------------------------------------------------------------------------------
## X. Configuration du Switch2

<br />




