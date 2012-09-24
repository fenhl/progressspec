Progress URIs
=============

Progress uses URIs of the *prog* or *gres* schemes interghangeably. Although they don't use the *urn* scheme and aren't currently registered with IANA, they function much like URNs: the URI converter on the Progress website can resolve them into *http* scheme URIs that point to a webpage of the host/user/task in question, provided that the host has a web interface and supports the URI converter. Progress clients should thell the OS that they can understand this URI scheme, and when presented with such a URI, make appropriate actions, such as:

*   If the URI refers to a host, ask if they want to create an account with that
    host or log in
*   If the URI refers to a user, display their profile
*   If the URI refers to a task, display it
    *   If the task has been delegated to the user, ask if they accept the
        delegation
    *   If the task is not theirs but is public or shared with them, allow the
        user to watch it
    *   They can also use it as a dependency in the above case

URI syntax
----------

This is the syntax for Progress URIs in Augmented Backus-Naur Form:

    uri = scheme ":" [user "@"] host ["/" taskid]
    ; the user bit is optional for servers which have a default user (or only
    ; one), or when referring to the server itself. URIs that refer to a user
    ; omit the task ID
    scheme = "prog" / "gres"
    ; "task" may also be used in the future, if Progress becomes a standard of
    ; some sort
    user = 1*20(ALPHA / DIGIT) ; the username of an account
    host + 1*(CHAR) ; any IP address or domain name
    taskid = 1*(ALPHA / DIGIT / "-" / "_")
    ; the host may randomize these, use a date-and-time-based system, or simply
    ; iterate through them. They just have to be unique
