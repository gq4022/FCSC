# Clair connu
Pour cette première WU je vous présente ce challenge, ma foi assez simple mais compliqué sur les bords.<br/>
On nous donne un fichier output.txt et un script Python.<br/>
## Apperçu script
```py
import os
from Crypto.Util.number import long_to_bytes
from Crypto.Util.strxor import strxor

FLAG = open("flag.txt", "rb").read()

key = os.urandom(4) * 20
c = strxor(FLAG, key[:len(FLAG)])
print(c.hex())
```
Le fichier flag.txt a été importé avec comme file IO rb (read binary).<br/>
On sait que la key fait 4 caractères * 20. La fonction urandom(4) retourne 4 bytes (octets) randoms en format Little-endian.<br/>
En réalité, la key ne fait pas 80 caractères. Le blocksize de la key équivaut à 4 caractères vu que la séquence est répétée 20x d'affilé.<br/>
Connaissant le format des flags FCSC. On a déjà notre known plaintext, notamment "FCSC".<br/>
## Apperçu output
```
d91b7023e46b4602f93a1202a7601304a7681103fd611502fa684102ad6d1506ab6a1059fc6a1459a8691051af3b4706fb691b54ad681b53f93a4651a93a1001ad3c4006a825
```
Je présume que c'est le flag qui a été xoré + encodé en hexadécimal.<br/>
## Solution
Je commence par un xor basique en me munissant des éléments que j'ai.<br/>
Tout d'abord je fous un fromhex au niveau du flag codé.<br/>
Je le xor avec la key FCSC encodé en UTF8.<br/>
Ensuite je pose un To hex et je chope les 4 premiers octets étant donné que la known plaintext fait 4 caractères de long.<br/>
Je remplace la key "FCSC" par les 4 octets et je l'encode en hexadécimal.<br/>
On voit qu'on a flag ce challenge.
## Avis
Quand je faisais le chall je pensais que la key faisait 80 caractères. D'où le urandom(4)\*20.<br/>
Mais au final non car il n'y a aucun cycle, et ça se devine facilement.<br/>
Le block size de la key est de 4 caractères vu que la key se répète 20x.<br/>
Donc chaque block de 4 octets a la même propriété xor.<br/>