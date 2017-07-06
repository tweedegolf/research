
[article](https://hpbn.co/webrtc/)
[serverless webrtc blog](http://blog.printf.net/articles/2013/05/17/webrtc-without-a-signaling-server/)
[serverless webrtc code](https://github.com/cjb/serverless-webrtc)
[signaling servers](https://www.leggetter.co.uk/real-time-web-technologies-guide/)
[tcp - udp](http://www.mrc.uidaho.edu/mrc/people/jff/443/Handouts/Ethernet/Specific%20Protocols/TCP%20vs%20UDP%20-%20Difference%20and%20Comparison%20_%20Diffen.pdf)
[sip.js](https://sipjs.com/) -> total replacement of traditional phone lines
[iot & rtc](http://iot.ieee.org/newsletter/march-2016/iot-realtime-communications.html)
[IMS](https://en.wikipedia.org/wiki/IP_Multimedia_Subsystem)
[VoLTE](https://en.wikipedia.org/wiki/Voice_over_LTE)
[Flash p2p](http://www.adobe.com/devnet/adobe-media-server/articles/p2p_apps_cirrus_lccs.html)


Codecs zijn royalty free
Met Flash had je een server licentie nodig (en voor een beter codec)

WebRTC is big business omdat alle online apparaten nu kunnen video en audio streamen (iOT, mobile devices)


ICE: Interactive Connectivity Establishment (RFC 5245)
    STUN: Session Traversal Utilities for NAT (RFC 5389)
    TURN: Traversal Using Relays around NAT (RFC 5766)
SDP: Session Description Protocol (RFC 4566)
DTLS: Datagram Transport Layer Security (RFC 6347)
SCTP: Stream Control Transport Protocol (RFC 4960)
SRTP: Secure Real-Time Transport Protocol (RFC 37


Signaling => get the constraint of peers


[demo](https://talky.io/)


Datagram Transport Layer Security (DTLS) is used to negotiate the secret keys for encrypting media data and for secure transport of application data.
Secure Real-Time Transport (SRTP) is used to transport audio and video streams.
Stream Control Transport Protocol (SCTP) is used to transport application data.


use cases:

- shared viewing platforms
- conferencing
- gaming (data)
- connect experts with consumers in one-on-one video conversations: tutoring,telehealth, personal training, personal shopping, and more
- VR & AR



WebRTC is appealing for developers for two main reasons:

The ability to integrate video conversations into the custom design of an application.
The ability to create custom tracking for each session to gain business intelligence.


Examples:

- CodeMentor



UDP => self-contained datagrams that can travel from source to destination themselves, without relying on earlier exchanges, stateless, no handshaking

UDP is een zogenaamd null-protocol:

- no guarantee of message delivery
- no guarantee of order of delivery
- no connection state tracking (stateless)
- no congestion control

UDP is very convinient for delivered via an unreliable service—no delivery guarantees, no failure notifications
