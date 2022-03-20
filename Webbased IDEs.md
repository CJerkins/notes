REF:
https://github.com/theia-ide/theia-apps#theia-docker

https://github.com/theia-ide/theia-apps/blob/master/theia-https-docker/README.md

## How to use

Run on https://localhost:10443 with the current directory as a workspace (creating a random token):
```
docker run --init -it -p 10443:10443 -v "$(pwd):/home/project:cached" theiaide/theia-https:latest
```
Output
```
generating key file for https
generating cert file for https
root INFO Theia app listening on http://localhost:3000.
redirecting to localhost:3000
access url: https://0.0.0.0:10443?token=0f34fd329f266caca309bd47f8f8cc6f
token: 0f34fd329f266caca309bd47f8f8cc6f
use cookies: true
expiration: 60
```

Run on https://localhost:10443 with the current directory as a workspace and the token "mysecrettoken":
```
docker run --init -it -p 10443:10443 -e token=mysecrettoken -v "$(pwd):/home/project:cached" theiaide/theia-https:latest
```
Output
```
generating key file for https
generating cert file for https
root INFO Theia app listening on http://localhost:3000.
redirecting to localhost:3000
access url: https://0.0.0.0:10443?token=mysecrettoken
token: mysecrettoken
use cookies: true
expiration: 60
```
The repository also contains a script called theiahere that wraps this command to ease the usage. You are invited to copy it to /usr/local/bin to be able to use it from any folder.

