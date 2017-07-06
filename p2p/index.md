# P2P

Dit praatje gaat over p2p en ik behandel 2 p2p technologieën:

- ipfs
- webrtc

Behalve dat het allebei implementaties zijn van p2p hebben ze verder niets met elkaar te maken. Beide onderwerpen zijn enorm complex en uitgebreid en omvatten een boom aan protocollen en onderliggende technologieën dus in dit praatje kan ik niet al te diep op alles ingaan.

## WebRTC

Ik begin met WebRTC. WebRTC is een open web standaard die in 2011 voor Chrome is bedacht voor een browser plugin die het mogelijk maakt om te bellen en te chatten via gmail. Rond 2013 was WebRTC in Chrome en Firefox geimplementeerd, in 2015 volgde Edge en op dit moment is Apple bezig het in te bouwen in Webkit / Safari (het is al beschikbaar in de Tech Preview).

WebRTC staan voor Web Real Time Communication. Met WebRTC is het mogelijk om audio en video streams (bijvoorbeeld van de je webcam) rechtstreeks naar een peer (bijvoorbeeld een andere browser) te streamen. Daarnaast kun je data streamen, dus alles wat niet audio of video is.

RTC was al sinds 2008 / 2009 mogelijk met Flash maar nu is het dus ingebouwd in de meeste browsers en is er een vrij eenvoudige Javascript API beschikbaar.


### Toepassingen

1. video conferencing ([appear.in](https://appear.in))
2. monitoren van IP bewakingscamera's
3. monitoren van machines d.m.v. meerdere speciale sensoren, b.v. infrarood camera, temperatuur, richtmicrofoons op bepaalde onderdelen van de machine
4. teleleren ([codementor.io](https://www.codementor.io/))
5. teleconsult; loodgieter kijkt mee met klant om te bepalen welke materialen ze moet kopen voordat ze ter plaatse gaat


Al deze toepassingen zitten in het vaarwater van de tradionele telecommunicatie vandaar dat naast de bekende tech bedrijven (Google, Mozilla, Opera, Microsoft en Apple) grote telecom / hardware bedrijven hebben meegewerkt aan het tot stand komen van de standaard en het implementeren van WebRTC in hun apparaten.

- Ericson
- Cisco

Inmiddels zijn er al een grote hoeveelheid bedrijven die goed geld verdienen met het aanbieden van services die ofwel WebRTC gebruiken ofwel daar een service voor leveren.

### Hoe werkt het

#### UDP

WebRTC gebruikt UDP voor het verzenden van de data, dus UDP over IP, UDP/IP. UDP is een protocol met weinig overhead. UDP pakketjes zijn self-contained ze kunnen zelf hun weg vinden van zender naar ontvanger, zonder afhankelijk te zijn van eerder verzonden pakketjes.

TCP levert een betrouwbare stroom van pakketjes in de juiste volgorde en als een pakketje kwijtraakt worden alle pakketjes die na dat pakketje binnen zijn gekomen gebufferd en wacht de stream tot het kwijtgeraakte pakketje opnieuw verstuurd is.

UDP daarentegen is een zogenaamd null-protocol:

- no guarantee of message delivery
- no guarantee of order of delivery
- no connection state tracking (stateless)
- no congestion control

Dit lijkt onhandig maar dit is juist erg geschikt voor streaming data over een lijn met wisselende bandbreedte. Want met streaming audio en video is betrouwbaarheid van de stream minder belangrijk dan de timeliness, de stiptheid, de timing. Als bij een video conference de audio stream bijvoorbeeld net als bij TCP zou gaan wachten tot een kwijtgeraakt pakketje opnieuw gestuurd wordt, of totdat de net-congestie is opgelost, raakt de audio uit sync met de video.


#### NAT

Het probleem van UDP is dat de meeste computers achter een NAT apparaat zitten (bv. een router). NAT is bedacht om het opraken van IP4 adressen uit te stellen; de locale ranges kunnen immers hergebruikt worden:

- 10.0.0.0 - 10.255.255.255 => 16777216 lokale adressen
- 172.16.0.0 - 172.31.255.255=> 1048576 lokale adressen
- 192.168.0.0 - 192.168.255.255 => 65536 lokale adressen

Het uitsturen van een UDP datagram is geen probleem: in ieder IP datagram (package) wordt het interne ip adres van de zender omgezet naar het externe publieke adres (bv 192.168.0.10 => 92.111.112.10) en van ieder UDP datagram wordt de interne poort omgezet naar de externe poort (bv 1337 => 15436).

Het probleem is het ontvangen van UDP pakketjes. Een TCP verbinding begint met een handshake en de verbinding wordt pas weer afgesloten na close message (of een timeout). De router kan de routing van extern adres en poort naar intern adres en poort voor TCP pakketjes dus tijdelijk opslaan en kan er zo vanuitgaan dat ieder pakketje dat binnenkomt op een bepaalde externe poort naar de opgeslagen interne poort gestuurd moet worden.

UDP is echter stateless dus moeten er technieken gebruikt worden om UDP pakketjes naar het juiste adres en de juiste poort te kunnen routen.


#### STUN

STUN staat voor Session Traversal Utilities for NAT en doet precies wat de naam doet vermoeden; het maakt een sessie aan die net als bij TCP een externe adres:poort tuple koppelt aan een interne adres:poort tuple. Verder worden er regelmatig keep-alive pakketjes gestuurd omdat UDP stateless is en dus geen eind commando heeft. Feitelijk maakt STUN een UDP stream stateful.

De STUN functionaliteit draait op een externe server, de STUN server. Een client maakt connectie met een STUN server en de STUN server retourneert de externe adres:poort tuple van de client; deze gegevens kan de client vervolgens naar de peer sturen waarmee ze een p2p verbinding wil opzetten.

De functionaliteit van STUN is dus vrij eenvoudig en je zou zelf een STUN server kunnen schrijven. Google biedt een aantal gratis STUN servers aan maar er zijn meer gratis STUN servers, zie [hier](https://gist.github.com/zziuni/3741933).


#### TURN

Helaas is STUN niet voor alle complexe NAT configuraties geschikt; in ongeveer 9% van de gevallen lukt het niet om met STUN een p2p verbinding op te bouwen. Daarvoor is TURN in het leven geroepen: Traversal Using Relays around NAT. Ook hier doet het weer precies wat de naam aangeeft: de UDP pakketjes worden via een externe server verstuurd. Feitelijk is het daarmee dus geen p2p meer!

Omdat de UDP stream via de TURN server loopt verbruikt een TURN server veel bandbreedte en worden er ook zwaardere eisen gesteld aan de hardware i.v.m. een STUN server. TURN server werken dan ook allemaal met een betaald account.


#### ICE Trickle

ICE staat voor Interactive Connectivity Establishment en dit is het proces van het tot stand brengen van een p2p verbinding. De ICE agent werkt met zogenaamde ICE candidates, dat zijn mogelijke verbindings routes.

Het trickle principe zorgt ervoor dat de p2p verbinding over de korst mogelijke route wordt opgebouwd:

- eerst wordt gekeken of de peers zich in hetzelfde lokale netwerk bevinden, dat is de makkelijkste en kortste route
- zo niet, dan wordt gekeken of er een STUN server geconfigureerd is en wordt geprobeerd of de verbinding met de door de STUN server teruggestuurde adres:poort tuples tot stand gebracht kan worden
- als dat niet lukt met STUN wordt er een relay verbinding opgebouwd via de TURN server

Dit trickle proces kan zich tijdens een actieve p2p verbinding herhalen als dat nodig is, bijvoorbeeld als er een storing is in het netwerk.

De ICE agent zorgt er ook voor dat er een keep alive signaal wordt verstuurd. De ICE agent is onderdeel van de WebRTC API waardoor de bovenstaande functionaliteit grotendeels weggeabstraheerd is.

#### Signaling

Een belangrijk onderdeel van p2p is het signaling mechanisme. Bij een p2p verbinding via de telefoon is het signaling mechanisme je ringtoon. In WebRTC is geen singaling ingebouwd en dat is een bewuste keuze omdat je op deze manier van bestaande signaling services gebruik kunt maken.

Je kunt bijvoorbeeld een WebRTC webclient verbinden aan een SIP (Session Initiation Protocal) service zodat je een PSTN (Public Switched Telephone Network) telefoon van een peer kunt laten overgaan.

Je kunt ook vrij eenvoudig je eigen signaling service bouwen, bijvoorbeeld door gebruikers van een chat zich te laten registreren met een username en hun email adres. Als je een p2p verbinding wilt opbouwen kun de username als telefoonnummer gebruiken en het email adres als signaal (de server verstuurd een email: "gebruiker X wil met je chatten". Of als de gebruiker al online is kan de server een push bericht sturen.

Een singaling service zorgt er ook voor dat de stream in het juiste formaat wordt verstuurd. Een peer die een verbinding wil maken met andere peer communiceert daarom welke codecs, formaten en afmetingen (video) er ondersteund worden. De ander peer slaat deze informatie op en verstuurd communiceert zijn eigen constraints.

Het converteren van de streams naar de juiste formaten, afmetingen en codecs is onderdeel van WebRTC en in de browser geimplementeerd: daar heb je dus geen omkijken naar.

#### Nog meer complexe zaken onder de motorkap


### Conclusie

WebRTC is een complexe technologie die ondanks het feit dat het p2p is niet zonder externe server kan.