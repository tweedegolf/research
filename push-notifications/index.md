
Notification API
Push API
Provider

Deze presentatie gaat over push notifications: dit zijn notificaties op de client van het feit dat er een server push heeft plaatsgevonden.

Notification API

Een notificatie kan zich op verschillende manieren manifesteren, afhankelijk van het apparaat en de status van het apparaat en het besturingssysteem. Mogelijke verschijningsvormen van een notificatie zijn:

een melding op het homescreen (van een mobiel apparaat)
een geluidje
een popup
alleen iOS: een badge bij het app icoontje, of als er al een badge getoond wordt het nummertje van de badge ophogen
een combinatie van bovenstaande

Een notificatie wordt door het besturingssysteem gegenereerd. Voor Android en iOS is er een API beschikbaar om vanuit een native app een notificatie te genereren. Ook voor browsers is er een Notification API die inmiddels in vrijwel iedere moderne browser is geimplementeerd. Opvallend, maar niet verwonderlijk is dat met uitzondering van Chrome de mobiele versies van deze browsers geen notificaties ondersteunen.

Een native of webapp kan dus zelf een notificatie genereren, bijvoorbeeld een Ramadan app kan een notificatie geven wanneer het tijd is voor een gebed (om maar niet weer de Todo app van stal te halen).

Push API

Een push is zoals gezegd een actie vanuit de server. Om een push van een server te kunnen ontvangen moet er continue een verbinding naar de server open staan. Het is niet efficient als iedere app zijn eigen socket verbinding onderhoud. Daarom wordt de verbinding opgezet en onderhouden door 1 enkel background process dat de binnenkomende pushes distribueert naar de juiste apps. Met de Push API kun je je app registreren aan dit background process. Voor web-apps hebben op dit moment alleen Firefox en Chrome de Web Push API geimplementeerd.

Om veiligheidsredenen, met name voor native apps, kan niet zomaar iedere server een push verbinding opzetten en dus is er voor ieder os en iedere browser een eigen push service. Voor iOS apps is dat de APNs (Apple Push Notification service), voor Android apps en Chrome browsers GCM (Google Cloud Messages, nieuw: Firebase Cloud Messages) en voor Firefox browsers en FirefoxOS apps is er de Mozilla Push Service.

Native apps moeten vantevoren geregistreerd worden bij een push service. Tijdens deze registratie worden de benodigde certificaten (APNs) of keys (GCM) gegenereerd die je met je code mee moet compileren om daarmee de communicatie met de service op te kunnen zetten. Web apps hoef je niet vantevoren te registeren.

De communicatie tussen deze services en de (web) app gaat op basis van unieke tokens. Zo’n token is zeg maar een uniek adres voor een app op een specifiek device. Dezelfde app op een ander device heeft dus ook een ander token.

Zodra de app opstart meldt hij zich aan bij de service en deze geeft een token terug. Bij webapps geeft de service een subscription object terug met een property “endpoint”; dit is een capability url (USVString) wat in feite een combinatie is van de url van de service en het token.

Nadat de app zich heeft geregistreerd kan de server naar de app pushen. Een push gaat niet rechtstreeks naar de app maar via een background process op het os dat de binnenkomende pushes naar de juiste apps doorstuurt. Voor web apps is dit background process een ServiceWorker.

[diagram flow 1]

Provider
Een push service kan dus een push sturen naar geregistreerde apps. Maar de push zelf wordt niet geïnitieerd door de push service maar door een provider. Een provider is een server die bepaalt of er iets gebeurd is dat de moeite waard is om naar de clients, dus de apps te pushen.

Een app moet zich dus ook registreren bij de provider omdat de provider aan de push service moet laten weten naar welke clients gepushed moet worden. We krijgen dan het volgende diagram:

[diagram flow 2]

Een app registreert zich dus eerst bij een push service waarna het een token terugkrijgt waarmee de app zich vervolgens registreert bij de provider.





De communicatie tussen de provider en de push service is ook beveiligd; voor APNs moet de provider een certificaat meesturen en voor GCM moet een key meegestuurd worden. Voor communicatie met de Mozilla Push Service is niet op zich nietkan de provider het endpoint gebruiken en voor de encryptie van een eventuele payload moet een key en een secret meegestuurd worden.



Background push (geen alert)


Parse server









Notifications:
Local → applicatie heeft zelf iets te melden n.a.v. een user / device / netwerk event
Remote → provider heeft iets te melden

Can:
Display message
Show badge (alleen macOS en iOS)
Play sound
Show something on home screen (check of dit ook kan met web)
Intent (iOS + Android)

Delivery:
Device based
Topic based (ook apn/web?) → volgens mij is dit alleen iets in GCM, maar je kunt ook met iOS of Web je abonneren op een topic; je device token wordt dan waarschijnlijk toegevoegd aan een topic tabel

Support Web Push:
Chrome Linux, macOS, Windows, Android (not iOS!)
Firefox Linux, macOS, Windows (not Android!)
Opera N/A
Safari 9 N/A (maybe 10)
Edge N/A

Error: registration.pushManager is undefined


De plaats en het uiterlijk van een notificatie hangt af van het soort app (browser, native app) en van het device (mobile, desktop)


Starting in iOS 8, you can optionally create actionable notifications by registering custom actions for a notification type. When an actionable notification arrives, the system creates a button for each registered action and adds those buttons to the notification interface. These action buttons give the user a quick way to perform tasks related to the notification. For example, a remote notification for a meeting invite might offer actions to accept or decline the meeting. When the user taps one of your action buttons, the system notifies your app, giving you an opportunity to perform the corresponding task.





Web:

Notification: creëert een nieuwe notificatie (geïmplementeerd in vrijwel iedere browser)
Push: krijgt notificaties binnen van de provider via GCM/APNs/anders en een ServiceWorker(zie boven)




Provider → Push Service → Application (zie flow diagram)


Met MessageChannel kun je de payload van een notification van de ServiceWorker naar de application sturen: je hoeft dan dus niet een notificatie te tonen maar met de payload van de notification kun je bijvoorbeeld data van de applicatie updaten.

Note: het is nog verplicht een notificatie te laten zien in FF en Chrome

Links:
https://github.com/chrisdavidmills/push-api-demo
https://developer.mozilla.org/en-US/docs/Web/API/Channel_Messaging_API/Using_channel_messaging






https://github.com/mozilla-services/autopush
https://github.com/argon/node-apn (werkt nog niet met de nieuwste API van vorig jaar)


https://developer.apple.com/notifications/


https://mozilla-push-service.readthedocs.io/en/latest/




