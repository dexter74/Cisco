------------------------------------------------------------------------------------------------------------------------------------------------
# <p align='center'> Base du réseau </p>


------------------------------------------------------------------------------------------------------------------------------------------------
### A. Base du réseau
#### I. Puissance de 2
| 2<sup>8</sup> | 2<sup>7</sup> | 2<sup>6</sup> | 2<sup>5</sup> | 2<sup>4</sup> | 2<sup>3</sup> | 2<sup>2</sup> | 2<sup>1</sup> | 2<sup>0</sup> |
| ------------- | ------------- | ------------- | ------------- | ------------- | ------------- | ------------- | ------------- | ------------- |
|      256      |      128      |      64       |      32       |      16       |       8       |       4       |       2       |       1       |

#### II. Convertir  en Binaire 
| Réseau       | 128 | 64 | 32 | 16 | 8 | 4 | 2 | 1 |
| ------------ | --- | -- | -- | -- | - | - | - | - |
| 192          | 1   | 1  | 0  | 0  | 0 | 0 | 0 | 0 |
| 168          | 1   | 0  | 1  | 0  | 1 | 0 | 0 | 0 |
| 1            | 0   | 0  | 0  | 0  | 0 | 0 | 0 | 1 |
| 0            | 0   | 0  | 0  | 0  | 0 | 0 | 0 | 0 |
| 255          | 1   | 1  | 1  | 1  | 1 | 1 | 1 | 1 |

#### III. La règle Xor (1 Et 1 = 1)
| Description    | Adresse IP      | 1<sup>er</sup> Octet | 2<sup>nd</sup> Octet | 3<sup>ème</sup> Octet | 4<sup>ème</sup> Octet |
| -------------- | --------------- | -------------------- | -------------------- | --------------------- | --------------------- |
| Adresse IP     | 192.168.1.1     | 1 1 0 0 0 0 0 0      | 1 0 1 0 1 0 0 0      | 0 0 0 0 0 0 0 1       | 0 0 0 0 0 0 0 1       |
| Masque de S/R  | 255.255.255.0   | 1 1 1 1 1 1 1 1      | 1 1 1 1 1 1 1 1      | 1 1 1 1 1 1 1 1       | 0 0 0 0 0 0 0 0       |
| Adresse Réseau | 192.168.1.0     | 1 1 0 0 0 0 0 0      | 1 0 1 0 1 0 0 0      | 0 0 0 0 0 0 0 1       | 0 0 0 0 0 0 0 0       |
| Broadcast      | 192.168.1.255   | 1 1 0 0 0 0 0 0      | 1 0 1 0 1 0 0 0      | 0 0 0 0 0 0 0 1       | 1 1 1 1 1 1 1 1       |

<br /> 


------------------------------------------------------------------------------------------------------------------------------------------------
#### B. Exemple 1: Méthode Classic
#### I. CIDR
```
192.168.56.128/24
```

#### II. Convertir en Binaire
| Réseau       | 128 | 64 | 32 | 16 | 8 | 4 | 2 | 1 |
| ------------ | --- | -- | -- | -- | - | - | - | - |
| 192          | 1   | 1  | 0  | 0  | 0 | 0 | 0 | 0 |
| 168          | 1   | 0  | 1  | 0  | 1 | 0 | 0 | 0 |
| 56           | 0   | 0  | 1  | 1  | 1 | 0 | 0 | 0 |
| 128          | 1   | 0  | 0  | 0  | 0 | 0 | 0 | 0 |
| 255          | 1   | 1  | 1  | 1  | 1 | 1 | 1 | 1 |

#### III. La règle Xor (1 Et 1 = 1)

| Description    | Adresse IP      | 1<sup>er</sup> Octet | 2<sup>nd</sup> Octet | 3<sup>ème</sup> Octet | 4<sup>ème</sup> Octet |
| -------------- | --------------- | -------------------- | -------------------- | --------------------- | --------------------- |
| Adresse IP     | 192.168.56.128  | 1 1 0 0 0 0 0 0      | 1 0 1 0 1 0 0 0      | 0 0 1 1 1 0 0 0       | 1 0 0 0 0 0 0 0       |
| Masque de S/R  | 255.255.255.0   | 1 1 1 1 1 1 1 1      | 1 1 1 1 1 1 1 1      | 1 1 1 1 1 1 1 1       | 0 0 0 0 0 0 0 0       |
| Adresse Réseau | 192.168.56.0    | 1 1 0 0 0 0 0 0      | 1 0 1 0 1 0 0 0      | 0 0 1 1 1 0 0 0       | 0 0 0 0 0 0 0 0       |
| Broadcast      | 192.168.56.255  | 1 1 0 0 0 0 0 0      | 1 0 1 0 1 0 0 0      | 0 0 0 0 0 0 0 1       | 1 1 1 1 1 1 1 1       |


------------------------------------------------------------------------------------------------------------------------------------------------
#### B. Exemple 1: Méthode avec le nombre Magic
#### I. CIDR
```
172.25.30.50 / 23
```

#### II. Convertir le Masque de S/R (255.255.254.0)
```
23 Bits = 8 + 8 + 7
7 Bits = 128 + 64 + 32 + 16 + 8 + 4 + 2 = 254
```

#### III. Le nombre Magic
```
On prend le dernier Octet côté réseau soit 30  (172.25.30.XX)
On prend le dernier Octet côté Hôte   soit 254 (255.255.254.0)
```

```
On Soustrait: 256 - Octet hôte => 256 - 254 = 2
On multiple le résulat jusqu'à arrivé à la valeur de l'octet du réseau au plus proche(30)
2 x 15 = 30
Le multiple de 2 le plus de 30 et 30 car 2 x 15 = 30.
Le Net ID est 172.25.30.0
```

```
Pour le broadcast on prendre le multiple suivant soit 2 x 16 = 32
A ce chiffre on soustrait 1 soit 32 - 1 ) 31
L'adresse de broadcast est 172.25.31.0
```



------------------------------------------------------------------------------------------------------------------------------------------------
#### C. Exercice  (Studi)
#### I. CIDR + Utilisateur
```
192.168.100.0 / 19
Technicien: 300
Vendeur: 120
```

#### II. Convertir en Binaire
| Réseau       | 128 | 64 | 32 | 16 | 8 | 4 | 2 | 1 |
| ------------ | --- | -- | -- | -- | - | - | - | - |
| 192          | 1   | 1  | 0  | 0  | 0 | 0 | 0 | 0 |
| 168          | 1   | 0  | 1  | 0  | 1 | 0 | 0 | 0 |
| 100          | 0   | 1  | 1  | 0  | 0 | 1 | 0 | 0 |
| 0            | 0   | 0  | 0  | 0  | 0 | 0 | 0 | 0 |
| 255          | 1   | 1  | 1  | 1  | 1 | 1 | 1 | 1 |


#### III. La règle Xor (1 Et 1 = 1)

```
Broadcast: On passe les bits hôtes à 1. (3ème et 4ème octet)
         : On calcul le 3ème bits
```

| Description        | Adresse IP      | 1<sup>er</sup> Octet | 2<sup>nd</sup> Octet | 3<sup>ème</sup> Octet | 4<sup>ème</sup> Octet |
| ------------------ | --------------- | -------------------- | -------------------- | --------------------- | --------------------- |
| Adresse IP         | 192.168.100.0   | 1 1 0 0 0 0 0 0      | 1 0 1 0 1 0 0 0      |  0 1 1 0 0 1 0 0      | 0 0 0 0 0 0 0 0       |
| Masque de S/R      | 255.255.224.0   | 1 1 1 1 1 1 1 1      | 1 1 1 1 1 1 1 1      |  1 1 1 0 0 0 0 0      | 0 0 0 0 0 0 0 0       |
| Adresse Réseau     | 192.168.96.0    | 1 1 0 0 0 0 0 0      | 1 0 1 0 1 0 0 0      |  0 1 1 0 0 0 0 0      | 0 0 0 0 0 0 0 0       |
| Broadcast          | 192.168.127.255 | 1 1 0 0 0 0 0 0      | 1 0 1 0 1 0 0 0      |  0 1 1 1 1 1 1 1      | 1 1 1 1 1 1 1 1       |


#### IV. Calculer le nombre de machines (Global)
```
Nous avons 19 Bits pour le réseau. (CIDR)
 > 32 Bits - 19 Bits (Réseau) = 13 Bits (Hôtes) 
 > La puissance de 2 pour 13 Bits. (2^13 - 2)
 > Soit 8190 Machines disponibles
```
| 8 Octets | Réseau  | Hôte    |
| -------- | ------- | ------- |
| 32 Bits  | 19 Bits | 13 Bits |




#### V. Calculer la quantité de Bits nécessaire
| Nom de l'équipe | IP nécessaire | Bits Hôtes       | Hôtes | Bits Réseau (Restant) | Masque de S/R   |
| --------------- | ------------- | ---------------- | ------| --------------------  | --------------- |
| Technicien      | 300           | (2<sup>9</sup>)  | 510   | 32 - 9 = 23 (254)     | 255.255.254.0   |
| Vendeur         | 120           | (2<sup>7</sup>)  | 126   | 32 - 7 = 25 (128)     | 255.255.255.128 |

```
/23 = 8 + 8 + 7 = 128 + 64 + 32 + 16 + 8 + 4 + 2
/25 = 8 + 8 + 9 = 8 + 1
```

#### VI. Calcule le réseau

On commence par la première plage IP : 192.168.96.0.


Technicien: 300 Postes
```
- Bits Hôte: 2^9 = 512 IP - 2 = 510 Hôtes
- Calculer la quantité de réseau nécessaire: 512 / 256 = 2
- Il faut 2 Réseaux.
- La dernière IP sera donc 192.168.97.254
- L'adresse de Broadcast sera 192.168.97.255
```

| Description        | Adresse IP      | 1<sup>er</sup> Octet | 2<sup>nd</sup> Octet | 3<sup>ème</sup> Octet | 4<sup>ème</sup> Octet |
| ------------------ | --------------- | -------------------- | -------------------- | --------------------- | --------------------- |
| Adresse IP (Début) | 192.168.96.0    | 1 1 0 0 0 0 0 0      | 1 0 1 0 1 0 0 0      |  0 1 1 0 0 0 0 0      | 0 0 0 0 0 0 0 0       |
| Masque de S/R      | 255.255.254.0   | 1 1 1 1 1 1 1 1      | 1 1 1 1 1 1 1 1      |  1 1 1 1 1 1 1 0      | 0 0 0 0 0 0 0 0       | 
| Plage de Fin       | 192.168.97.254  | 1 1 1 1 1 1 1 1      | 1 1 1 1 1 1 1 1      |  0 1 1 0 0 0 0 1      | 1 1 1 1 1 1 1 0       |
| Broadcast          | 192.168.97.255  | 1 1 1 1 1 1 1 1      | 1 1 1 1 1 1 1 1      |  0 1 1 0 0 0 0 1      | 1 1 1 1 1 1 1 1       |


Vendeur: 120 Postes
```
Bits hôte: 2^7 = 128 IP - 2 = 126 Hôtes
Broadcast = 128 - IP réseau = 127
```

Le broadcast sera donc (255 / 2) - 1 = 127

| Description        | Adresse IP      | 1<sup>er</sup> Octet | 2<sup>nd</sup> Octet | 3<sup>ème</sup> Octet | 4<sup>ème</sup> Octet |
| ------------------ | --------------- | -------------------- | -------------------- | --------------------- | --------------------- |
| Adresse IP (Début) | 192.168.98.1    | 1 1 0 0 0 0 0 0      | 1 0 1 0 1 0 0 0      | 0 1 1 0 0 0 0 1       | 0 0 0 0 0 0 0 1       |
| Masque de S/R      | 255.255.255.128 | 1 1 1 1 1 1 1 1      | 1 1 1 1 1 1 1 1      | 1 1 1 1 1 1 1 1       | 1 0 0 0 0 0 0 0       |
| Plage de Fin       | 192.168.98.126  | 1 1 0 0 0 0 0 0      | 1 0 1 0 1 0 0 0      | 0 1 1 0 0 0 0 1       | 0 1 1 1 1 1 1 0       |
| Broadcast          | 192.168.98.127  | 1 1 0 0 0 0 0 0      | 1 0 1 0 1 0 0 0      | 0 1 1 0 0 0 0 1       | 0 1 1 1 1 1 1 1       |


127 = 64 + 32 + 16 + 8 + 4 + 2 + 1


Résumé de l'exercice:
```
-----------------------------------------------------------------------------------
192.168.100.0 / 19
192.168.100.0 > 11000000.10101000.01100100.00000000
255.255.224.0 > 11111111.11111111.11100000.00000000 /19
192.168.96.0  > 11000000.10101000.01100000.00000000 Premier réseau de la plage

-----------------------------------------------------------------------------------
Technicien: 300

Bit Hôte    : 2^9 = 512 IP - 2 = 510 Hôtes (1 bits consommé sur le 4ème octet)
Bit Réseau  : 512 / 256 = 2 Réseaux nécessaire ( XXX.XXX.97.0 et XXX.XXX.98.0)

IP Réseau   : 192.168.96.0
IP de début : 192.168.96.1
IP de Fin   : 192.168.97.254 
IP Broadcast: 192.168.97.255

Vendeur: 120
Bit Hôte    : 2^7= 128 IP - 2 = 126 Hôtes
Bit Réseau  : 2^1 = 2 Réseaux
Broadcast   : 128 - 1 Adresse réseau = 127

IP réseau   : 192.168.98.0
IP de début : 192.168.98.1
IP de Fin   : 192.168.98.126
IP Broadcast: 192.168.98.127
-----------------------------------------------------------------------------------
```
