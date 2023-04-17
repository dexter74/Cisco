------------------------------------------------------------------------------------------------------------------------------------------------
## <p align='center'> Création d'un Vlan sous OpenWRT et un switch Netgear </p>

------------------------------------------------------------------------------------------------------------------------------------------------

#### Présentation de l'architecture:
```
Livebox: 192.168.1.0/24
 |-> Routeur: 192.168.1.100/24
```

------------------------------------------------------------------------------------------------------------------------------------------------
#### Configuration du Routeur
##### A. Configuration par défaut
```
Tous les ports physiques (Lan, extsw) sont reliés sur le bridge.
```
##### B. Libération des interfaces
```
Network > Interfaces > Devices > Br-lan > Configure
Bridge ports: Déselectionner les interfaces
```

#### C. Création du LAN
```
Network > Interfaces > Devices > Add device configuration

[General device options]
 - Device type      : Network Device
 - Existing device  : lan1

[Advanced device options]
 - Send ICMP redirects
 - Force IGMP version
```

