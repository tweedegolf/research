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

WebRTC gebruikt UDP voor het verzenden van de data, dus UDP over IP, UDP/IP.