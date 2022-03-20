server {
    listen 443 ssl http2;
    listen [::]:443 ssl http2;

    server_name vault.local.drokdev.pro vault.drokdev.pro;
    root /usr/share/nginx/html;
    
    # SSL
    ssl_certificate /etc/letsencrypt/live/vault.drokdev.pro/fullchain.pem; # managed by Certbot
    ssl_certificate_key /etc/letsencrypt/live/vault.drokdev.pro/privkey.pem; # managed by Certbot
    ssl_prefer_server_ciphers on;
    ssl_protocols TLSv1 TLSv1.1 TLSv1.2; 
    ssl_ciphers TLS-CHACHA20-POLY1305-SHA256:TLS-AES-256-GCM-SHA384:TLS-AES-128-GCM-SHA256:HIGH:!aNULL:!MD5;

    client_max_body_size 128M;
    client_body_buffer_size 128k; 

    # security
    include security.conf;

    # reverse proxy
    location / {
        proxy_pass http://172.16.100.10:8080;
        include proxy.conf;

    }

    location /notifications/hub {
        proxy_pass http://172.16.100.10:3012;

        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection "upgrade";
    }

    location /notifications/hub/negotiate {
        proxy_pass http://172.16.100.10:8080;
    }

    location /admin {
        proxy_pass http://172.16.100.10:8080;

        # See: https://docs.nginx.com/nginx/admin-guide/security-controls/configuring-http-basic-authentication/
        auth_basic "Private";
        auth_basic_user_file /etc/nginx/.htpasswd;

        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;

    }

    # additional config
    include general.conf;

}

# subdomains redirect
server {
    listen 443 ssl http2;
    listen [::]:443 ssl http2;

    server_name *.vault.local.drokdev.pro *.vault.drokdev.pro;

    # SSL
    ssl_certificate /etc/nginx/ssl/bitwarden/cert.pem;
    ssl_certificate_key /etc/nginx/ssl/bitwarden/key.pem;

    return 301 https://vault.local.drokdev.pro$request_uri;
}

# HTTP redirect
server {
    if ($host = vault.drokdev.pro) {
        return 301 https://$host$request_uri;
    } # managed by Certbot


    listen 80;
    listen [::]:80;

    server_name vault.local.drokdev.pro vault.drokdev.pro;

    return 301 https://vault.local.drokdev.pro$request_uri;


}