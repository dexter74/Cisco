-----------------------------------------------------------------------------------------------------------------------------------------------
<p align ='center'> <img src='https://user-images.githubusercontent.com/35907/227744660-e51df597-ba58-4fe8-ac26-cf169eacb185.png'> </p>



-----------------------------------------------------------------------------------------------------------------------------------------------
### Switch 2

#### A. Création des VLANS 
```
enable
configure terminal

vlan 10
name Vlan_10

vlan 20
name Vlan_20

vlan 99
name Admin

exit
```

#### B. Configuration de l'attributions des interfaces aux VLANS
##### I. Liaison entre Switch
```
interface FastEthernet 0/1
switchport mode trunk
switchport trunk allowed vlan 10,20,99
switchport trunk native vlan 99
no shutdown
exit
```

##### II . Attribution d'interface au VLAN
```
interface FastEthernet 0/10
switchport access vlan 10
no shutdown
exit

interface FastEthernet 0/11
switchport access vlan 20
no shutdown
exit
```
