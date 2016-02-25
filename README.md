![airnet](http://i.imgur.com/bcj4OWt.png)
======

decentralized, secure, p2p mesh networking

goals:
  - transport agnostic (radio transceiver, internet, etc.)
  - pathfinding through the network to facilitate point -> point communication
  - direct connections encouraged but not required, helps to navigate around firewalls
  - peer to peer routing allows for devices to work together to route over airgaps (radio)
  - allow for secure communication through the network using key exchange protocols

uses:
  - swarms of UAVs communicating via radio and internet multiplex
  - decentralized app networks, internet of things

protocol
=======

the airnet protocol defines a transport agnostic way of communicating. no direct connection is assumed. devices may communicate through reliable transports (TCP/IP), or through wireless transceivers.

a target is identified by the sha256 hash of its public key. a connection can be opened to the target routing packets through the network via a SOCKS5 proxy, where the target hostname is specified as "hash.air"

messages are queued and sent as a route is available to the target.

zeromq is used as the message queue backing the system, and for all connections.

protobuf is used for all non-handshake communications.
