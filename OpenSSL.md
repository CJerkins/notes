One liner self signed cert
`openssl req -new -newkey rsa:4096 -days 365 -nodes -x509 -keyout /etc/ssl/private/snipe_key.pem -out /etc/ssl/certs/snipe_cert.pem`

`openssl req -x509 -newkey rsa:4096 -keyout pi.wall.key -out pi.wall.crt -days 365`

openssl req -new -newkey rsa:4096 -days 365 -nodes -x509 -keyout /etc/pki/tls/private/key.pem -out /etc/pki/tls/certs/cert.pem