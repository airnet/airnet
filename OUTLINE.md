System Overview
================
This is an overview of the connection and communication patterns in the airnet system.

Identification & Global Node Store
=========

Every node in the network has a public and private key combination. Every node in the network has a replicated (using the gossip algorithm) dictionary of other known nodes in the network. Every node object contains the public key associated with the node. Every update event to a node document must be signed using the associated private key, or it will be rejected. The same goes for any communication between nodes in the network - every step of the way, the messages will be verified as signed using the public key stored in the nodes document.

It is not possible to tell if a node is online or offline in the system, however, when any node makes a request to another node, the timestamp of the last known communication will be updated in the nodes' local store. Furthermore a node can inform the network of its public connection address and availability with an update object - a signed and timestamped message with the node ID (public key) and its public access address. If a node wants to contact another node, it will first reference its public connection information from the global node store and make a direct connection attempt. If this attempt fails, it can emit a gossip ripple "locate" request to the network. These will be detailed later on.

Connection
==========
A new node will contact known existing nodes to start the process of known node exchange. Known nodes will be saved in the local data store for the next connection attempt.

On connection, the node will present its public key to the peer, with a signed timestamp of the connection. The peer is then expected to find the entry for the connecting node in its local node store, and verify the node is authentic by checking the signed timestamp. Timestamp verification mitigates any vulnerability to replay attacks, as the timestamp is expected to be within a short period of time before the request.

If the node does not find any entry for the remote peer, it will request a full node descriptor from the peer. The peer will then create, sign, and upload the new peer identification, the node will verify the identification, and gossip the new peer identification to the rest of the network. It will then accept the new peer as a connection and continue operation.

As a node can connect to several other nodes initially, and may not be known in the network, when receiving a new node definition, nodes will ignore the node definition if its timestamp is less than the existing timestamp of the node in the store. It will also not re-broadcast the update if it is considered "old".

Client Object Updates
=========
When a node wants to make a update to its public object, it can emit a object update request. This request must be signed with the nodes private key, and will contain only updated attribute fields.

This request will be gossiped around the network, and gossipers will ignore the request if it has already been processed.

Request IDs are created by taking the public key of the object, hashing the key, and combining it with the timestamp.

Routing
========
Direct messaging sessions can be created with other nodes in the network. As it is highly inefficient to gossip messages between nodes, the procedure is as follows:

  1. Send the initial "route" gossip request.
  2. The route request is passed around the network until the desired peer is found. Route requests have node identifiers added to a chain of nodes in the request, used for pathing.
  3. The request finds the final node, and the final node sends the final node chain down the list of nodes back to the originator, using a Routed Request.
  4. The originator waits for a set amount of time for all route requests to be processed and returned. It can be assumed the node is not online if the route request never comes back.
  5. Finally, the routed requests can begin.

Routed Requests
================

Once a node has a list of routes to a desired target, sorted by most efficient (shortest) to least efficient (longest), it can begin sending messages through these routes to the target.

A message is sent to the first node in the route, which looks at the routed request (signed by the originator's private key), finds the next node in its active connections, and passes it along. If at any point the next node in the chain has been lost, the node that cannot find the next node in the chain will create a new routed request going the opposite direction to return the failed route.

There is no proof of delivery for any message through the system. If a client receives a proof of failure (route request failure), it will discard that route and any others that contain the connection between the failed node and the disconnected node, and attempt the next. 

Key Exchanges
============

Secure communication can be built on top of the Routed Requests platform. A key exchange can simply be performed through the routed requests platform in order to facilitate secure communications between two nodes.

