
# Nadelen HTTP

1. HTTP is gecentraliseerd en daardoor wordt het kwetsbaar:

- kwaadaardige overheden en bedrijven hoeven maar 1 server af te luisteren en kunnen dan al heel veel data afluisteren
- kwaadaardige overheden en bedrijven hoeven maar 1 server plat te gooien om toegang tot een groot deel van het web te blokkeren
- als een domeinnaam registratie of hosting wordt beeindigd is een gedeelte van het web onbereikbaar
- als er ergens een datacenter of kabel stuk gaat.
- DDOS attacks

2. HTTP is inefficient: een populaire youtube video wordt steeds van een Google server gehaald terwijl hij waarschijnlijk al een keer gedownload is door een computer meer in de buurt.

3. HTTP maakt ons te veel afhankelijk van de backbone, bijvoorbeeld als Google Docs plat ligt kun je niet meer bij je bestanden. Dit geldt ook voor de meeste andere cloud apps.

# Oplossingen IPFS

## Content in plaats van adres

HTTP zoekt naar een adres, IPFS zoekt naar content. De naam van de content is een cryptografische hash die gebaseerd is op de content (git). Zo weet je zeker dat de naam altijd naar hetzelfde en ongewijzigde bestand verwijst.

De hashes worden opgeslagen in DHT (Distributed Hash Table). DHT haalt het bestand op bij de verschillende seeds en checkt of het de juiste content is. Grote files worden opgesplitst in kleine brokjes (objects) van 255K zodat een file van meerdere seeds opgehaald kan worden en dus per saldo sneller kan inladen.

## Directories

Een hash kan verwijzen naar een directory; de namen van de bestanden in die directory hoeven niet aangepast te worden, dit is dus een hele makkelijke manier om een bestaande folder van een webserver naar IPFS te verplaatsen.

## Hash - human readable naam mapping

Hashes zijn lang en lastig te onthouden; met IPFS kun je de bestaande DNS infrastructuur gebruiken om makkelijke namen te koppelen aan hashes. IPFS gaat in de toekomst waarschijnlijk ook Namecoin ondersteunen. Namecoin is een open source technologie die het mogelijk maakt om bepaalde componenten van het web te kunnen decentraliseren, waaronder DNS.

## Hash - human readable naam mutable

Omdat de naam van een bestand gebaseerd is op de inhoud van dat bestand verandert de naam van het bestand als je iets aan de inhoud wijzigt. Met IPNS kun je hash aanmaken die altijd naar de laaste versie van een bestand wijst, net zoals een branchname op git altijd naar de laatste versie verwijst.


## IPFS / HTTP gateway
Er zijn implementaties van IPFS in go en javascript; als je die installeert op je computer wordt je een IPFS node waarmee je bestanden en websites kunt publiceren op IPFS. Ook kan je node als seed functioneren voor de bestanden die je lokaal hebt opgeslagen.