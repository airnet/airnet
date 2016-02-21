Handshake Messages
==================

See PROTOCOL.md for a high level explanation of these messages.

All messages have a one byte preamble which determines the message type.

Beacon
------

A beacon message advertises the existence of a participant. Intended for over-the-air radio packets.

-   message preamble: 0x01
-   timestamp: 64 bits unsigned integer
-   identifier: 256 bits identifier (sha256), hash of public key

Note: no signature is put in this beacon as it should be small (320 bits).

Init Challenge
--------------

A participant sends this challenge to another participant identified by their beacon.

-   message preamble: 0x02
-   timestamp: 64 bits unsigned int
-   target\_identifier: 256 bits identifier of the target (from beacon)
-   source\_identifier: 256 bits identifier of the source (from responder)
-   challenge: 256 bits random challenge

Challenge Response & Return Challenge
-------------------------------------

A participant responds to the Init Challenge with this response.

-   message preamble: 0x03
-   timestamp: 64 bits unsigned int
-   target\_identifier: 256 bits identifier of the target (from beacon)
-   source\_identifier: 256 bits identifier of the source (from responder)
