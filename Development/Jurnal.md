28.3.24.
I need to wirite out a auto-cert package for go. one prob exists but it is most likely too complex for my use case. The idea is to check if a TLS cert exists, if not, check the underline package manager, install cert-bot and set it to auto renew. Make it repeatable.

For now it will only be Debian, I cant bother with anything other then apt for the moment.