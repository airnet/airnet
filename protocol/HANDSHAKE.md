Handshake Messages
==================

See PROTOCOL.md for a high level explanation of these messages.

All messages have a one byte preamble which determines the message type.

Beacon
------

A beacon message advertises the existence of a participant. Intended for over-the-air radio packets.

-   message preamble: 0x01
-   version number: (currently 0x01)
-   timestamp: 64 bits unsigned integer
-   identifier: 256 bits identifier (sha256), hash of public key

Note: no signature is put in this beacon as it should be small (320 bits).

Init Challenge
--------------

A participant sends this challenge to another participant identified by their beacon.

-   message preamble: 0x02
-   version number: (currently 0x01)
-   timestamp: 64 bits unsigned int
-   challenge: 256 bits random challenge
-   request\_key: 8 bit boolean true/false, if true next message will contain the full public key

Challenge Response & Return Challenge
-------------------------------------

A participant responds to the Init Challenge with this response.

-   message preamble: 0x03, 0x04 if with public key
-   timestamp: 64 bits unsigned int
-   request\_key: 8 bit boolean true/false, if true next message will contain full public key
-   challenge: 256 bits random challenge
-   challenge\_nonce: 256 bits random nonce prepended to sent challenge
-   challenge\_response: 256 bits signature of (challenge\_nonce + challenge)
-   public\_key: if preamble is 0x04 then this field will be filled with the public key

Beacon Challenge Response
-------------------------

A beacon responds to the Init Challenge with this response.

-   message preamble: 0x03, 0x04 if with public key
-   timestamp: 64 bits unsigned int
-   request\_key: 8 bit boolean true/false, if true next message will contain full public key
-   challenge: 256 bits random challenge
-   challenge\_nonce: 256 bits random nonce prepended to sent challenge
-   challenge\_response: 256 bits signature of (challenge\_nonce + challenge)
-   public\_key: if preamble is 0x04 then this field will be filled with the public key

Association Confirmation
------------------------

Both clients send the following message to finalize the connection.

-   message preamble: 0x05
-   timestamp: 64 bits unsigned int
-   session\_nonce: 64 bit random nonce that the peer should use in all future messages in the session
