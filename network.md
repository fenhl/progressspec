Progress communication protocol
===============================

version Fenhl/1

This protocol is used for communication between Progress clients and servers. If two servers communicate with each other, one takes on the role of a client.

The version consists of two parts: the github username (or organization name) of the author, and a second part which is author-specific. For Fenhl's versions, this is a number that increases with each update. If the author-specific version name of a version by Fenhl includes any non-numeric characters, MUST be treated as absolutely incompatible with this version.

Progress uses TCP on port 4468 by default. The two parties send each other messages encoded in UTF-8 and terminated with a single line feed (U+A).

Each message starts with a keyword, and optionally continues with one or more arguments separated from each other and the keyword with a single space (U+20).

keywords
========

hi
--

server to client

Sent after establishing a new connection to let the client know which version the server is using.

arguments:

*   the version, e.g. "Fenhl/1". If this is not "Fenhl/1", the client MUST
    immediately disconnect, and SHOULD present the user (if any) with an
    appropriate error message. If this is "Fenhl/" followed by a number greater
    than 1, the client SHOULD attempt to automatically update to the latest
    version, unless prohibited by the user.
