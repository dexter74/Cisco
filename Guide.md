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

#### C. Création du Dispositifs LAN / VLAN
```
Network > Interfaces > Devices > Add device configuration

[General device options]
 - Device type      : Network Device
 - Existing device  : lan1

[Advanced device options]
 - Send ICMP redirects
 - Enable multicast support (IGMP)

-----------------------------------------------------------------------------------

Network > Interfaces > Devices > Add device configuration

[General device options]
Device type: Vlan (802.1q)
Base device: lan1

[Advanced device options]
 - Ingress QoS mapping
 - Egress QoS mapping
 - Accept local
 - Send ICMP redirects
 - Enable multicast support (IGMP) 
```


#### D. Création des Interfaces
```
LAN
Vlan1: vlan1.2
```
