Progress communication protocol
===============================

version Fenhl/2

This protocol is used for communication between Progress clients and servers. If two servers communicate with each other, one takes on the role of a client.

The version consists of two parts: the github username (or organization name) of the author, and a second part which is author-specific. For Fenhl's versions, this is a number that increases with each update. If the author-specific version name of a version by Fenhl includes any non-numeric characters, MUST be treated as absolutely incompatible with this version.

Progress uses TCP on port 4468 by default. The two parties send each other messages encoded in UTF-8 and terminated with a single line feed (U+A).

Each message starts with a keyword, and optionally continues with one or more arguments separated from each other and the keyword with a single space (U+20).

keywords
========

auth
----

both ways

Sent by the client when the user wants to log in. The server responds with this, detailing which authentication methods it supports. The client does not send any arguments.

arguments:

*   an authentication method from the list below that is accepted by the server,
    e.g. "pwd".
*   optionally, more authentication methods.

authentication methods:

*   pwd: a password (any of Unicode except U+0, U+A and U+20) sent in plaintext.

hi
--

server to client

Sent after establishing a new connection to let the client know which version the server is using.

arguments:

*   the version, e.g. "Fenhl/2". If this is not "Fenhl/2", the client MUST
    immediately disconnect, and SHOULD present the user (if any) with an
    appropriate error message. If this is "Fenhl/" followed by a number greater
    than 2, the client SHOULD attempt to automatically update to a version of
    itself that supports the version, unless prohibited by the user.
