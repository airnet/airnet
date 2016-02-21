Airnet Protocol
================

The airnet protocol uses [ProtoBuf](https://code.google.com/p/protobuf/) for high level communications.

Protobuf has been ported to a variety of languages - here are a few:

   - [Node.JS](https://github.com/chrisdew/protobuf)
   - [Ruby](https://github.com/localshred/protobuf)
   - [Second Ruby Implementation](https://github.com/macks/ruby-protobuf)
   - [Yet another Ruby Implementation](https://github.com/protobuf-ruby/beefcake)
   - [PHP](https://github.com/drslump/Protobuf-PHP)
   - [JavaScript - even in the browser!](https://github.com/dcodeIO/ProtoBuf.js/)

The airnet system is intended to work in any environment - from radios to in the browser.

Airnet sits on top of other network protocols like 6LowPan, TCP, UDP, and the Tor network. It is transport agnostic, and through the use of plugins, new transports can be implemented.

Connection
==========

A airnet "connection" can be made a few different ways:

 - transceiver connection over radio
 - internet connection (TCP/IP)
 - any other binary transport

A connection is not considered established until a handshake is complete. Once the handshake is complete, both nodes synchronize gossip-based node tables and regular communication can commence.

Airnet connections are similar to UDP in that there is no guarantee that a packet will reach its destination. Future revisions of the protocol may support TCP-like retransmission and checksum capabilities.

Handshake
========

 1. A: Beacon
 2. B -> A: Init challenge.
 3. A -> B: Challenge response & return challenge.
 4. B -> A: Association confirmation.

There are some rules in certain situations:

 - If a network connection is made the client making the connection should send the beacon first.
 - Beacons should have an expiry.
