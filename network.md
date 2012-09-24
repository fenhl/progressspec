Progress communication protocol
===============================

version Fenhl/3

This protocol is used for communication between Progress clients and servers. If two servers communicate with each other, one takes on the role of a client.

The version consists of two parts: the github username (or organization name) of the author, and a second part which is author-specific. For Fenhl's versions, this is a number that is incremented by 1 with each change to the protocol. If the author-specific version name of a version by Fenhl includes any non-numeric characters, MUST be treated as absolutely incompatible with this version.

Overview
========

The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD", "SHOULD NOT", "RECOMMENDED",  "MAY", and "OPTIONAL" in this document are to be interpreted as described in RFC 2119.

Progress uses TCP on port 4468 by default. The two parties send each other messages encoded in UTF-8 and terminated with a single line feed (U+A).

Each message starts with a command, and optionally continues with one or more arguments separated from each other and the command with a single space (U+20).

Commands
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

*   the version, e.g. "Fenhl/3". If this is not "Fenhl/3", the client MUST
    immediately disconnect, and SHOULD present the user (if any) with an
    appropriate error message. If this is "Fenhl/" followed by a number greater
    than 3, the client SHOULD attempt to automatically update to a version of
    itself that supports the version, unless prohibited by the user (e.g. in the
    client's preferences).

login
-----

both ways

Sent by the client when the user logs in. The server responds with this.

client arguments:

*   the authentication method used for this session. This MUST be one that is
    supported by the server, as specified through the auth command.
*   the username. If the host name is omitted (e.g. "jdoe" instead of
    "jdoe@example.org"), the user is assumed to be registered at the server
    receiving this command.
*   depending on the authentication method, optionally additional arguments
    *   when using the "pwd" method, the password (any of Unicode except U+0,
        U+A and U+20). If the user is registered at another host, the server
        MUST verify the password by attempting to log into the host with the
        username and password specified by the client, before responding to the
        command.

server arguments:

*   the string "OK" if the login succeeded, or the error code if it didn't.
    Error codes vary depending on the authentication method.

error codes

*   (pwd) "REG": user not registered.
*   (pwd) "PWD": wrong password.
