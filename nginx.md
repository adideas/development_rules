# FORGE CONFIG (DO NOT REMOVE!)
include forge-conf/app.zavgar.online/before/*;

map $http_origin $origin_allowed {
    #default 0;
    https://web.zavgar.online 1;
    https://fleetmanager.petrolplus.ru 1;
    https://fms.stavtrack.ru 1;
    https://soft5plus.ru 1;
    https://eam.toirs.ru 1;
}

map $origin_allowed $origin {
    #default "";
    1 $http_origin;
}

server {
    listen      80 default_server;
    server_name _;
    return      444;
}

server {
    #listen 443 ssl http2 default_server;
    #listen [::]:443 ssl http2 default_server;
    listen 443 ssl http2;
    listen [::]:443 ssl http2;
    
    server_name app.zavgar.online;
    server_tokens off;
    root /home/forge/app.zavgar.online/current/public;
    
    client_max_body_size 25M;
    
    #return 500;
    
    # FORGE SSL (DO NOT REMOVE!)
    ssl_certificate /etc/nginx/ssl/app.zavgar.online/1391979/server.crt;
    ssl_certificate_key /etc/nginx/ssl/app.zavgar.online/1391979/server.key;

    ssl_protocols TLSv1.2 TLSv1.3;
    ssl_ciphers ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-RSA-AES128-GCM-SHA256:ECDHE-ECDSA-AES256-GCM-SHA384:ECDHE-RSA-AES256-GCM-SHA384:ECDHE-ECDSA-CHACHA20-POLY1305:ECDHE-RSA-CHACHA20-POLY1305:DHE-RSA-AES128-GCM-SHA256:DHE-RSA-AES256-GCM-SHA384;
    ssl_prefer_server_ciphers off;
    ssl_dhparam /etc/nginx/dhparams.pem;

    add_header X-Frame-Options "SAMEORIGIN";
    add_header X-XSS-Protection "1; mode=block";
    add_header X-Content-Type-Options "nosniff";
    #add_header Content-Security-Policy "app.zavgar.online 'self';";

    index index.html index.htm index.php;

    charset utf-8;

    # FORGE CONFIG (DO NOT REMOVE!)
    include forge-conf/app.zavgar.online/server/*;
    
    #return 418;
    
    location ~* \.(eot|ttf|woff)$ {
        add_header Access-Control-Allow-Origin *;
    }
    
    location ~* /storage/(.+)\.php$ {
        return 200 "Thank you for checking the vulnerability. Please send me a telegram - https://t.me/no_index";
    }

    location / {
        try_files $uri $uri/ /index.php?$query_string;
    }

    location = /favicon.ico { access_log off; log_not_found off; }
    location = /robots.txt  { access_log off; log_not_found off; }

    access_log off;
    error_log  /var/log/nginx/app.zavgar.online-error.log error;

    error_page 404 /index.php;

    location ~ \.php$ {
        #rewrite ^/(.php)$ /index.php last;
        fastcgi_split_path_info ^(.+\.php)(/.+)$;
        fastcgi_pass unix:/var/run/php/php7.4-fpm.sock;
        fastcgi_index index.php;
        #fastcgi_read_timeout               100m;
        #fastcgi_send_timeout               100m;
        #proxy_read_timeout                 100m;
        include fastcgi_params;
        
        add_header 'Access-Control-Max-Age' 86400;
        add_header 'Access-Control-Allow-Origin' "$origin" always;
        
        #add_header 'Access-Control-Allow-Origin' "$http_origin" always;
        add_header 'Access-Control-Allow-Credentials' 'true' always;
        add_header 'Access-Control-Allow-Methods' 'GET, POST, PUT, DELETE, OPTIONS' always;
        add_header 'Access-Control-Allow-Headers' 'Accept,Authorization,Cache-Control,Content-Type,DNT,If-Modified-Since,Keep-Alive,Origin,User-Agent,X-Requested-With,X-CSRF-Token,X-XSRF-TOKEN,FCM' always;
        add_header 'Access-Control-Expose-Headers' 'Authorization' always;
        add_header 'Access-Control-Expose-Headers' 'Authorization, X-RateLimit-Limit, X-RateLimit-Remaining, X-RateLimit-Reset' always;

        
        if ($request_method = 'OPTIONS') {
            return 204;
        }
        
        ##if ($http_origin ~ '^https://web.zavgar.online') {
        ##    add_header 'Access-Control-Allow-Origin' "$http_origin" always;
        ##    add_header 'Access-Control-Allow-Credentials' 'true' always;
        ##    add_header 'Access-Control-Allow-Methods' 'GET, POST, PUT, DELETE, OPTIONS' always;
        ##    add_header 'Access-Control-Allow-Headers' 'Accept,Authorization,Cache-Control,Content-Type,DNT,If-Modified-Since,Keep-Alive,Origin,User-Agent,X-Requested-With,X-CSRF-Token,X-XSRF-TOKEN,FCM' always;
        ##    add_header 'Access-Control-Expose-Headers' 'Authorization' always;
        ##}
        ##if ($http_origin ~ '^https://ppr.zavgar.online') {
        ##    add_header 'Access-Control-Allow-Origin' "$http_origin" always;
        ##    add_header 'Access-Control-Allow-Credentials' 'true' always;
        ##    add_header 'Access-Control-Allow-Methods' 'GET, POST, PUT, DELETE, OPTIONS' always;
        ##    add_header 'Access-Control-Allow-Headers' 'Accept,Authorization,Cache-Control,Content-Type,DNT,If-Modified-Since,Keep-Alive,Origin,User-Agent,X-Requested-With,X-CSRF-Token,X-XSRF-TOKEN,FCM' always;
        ##    add_header 'Access-Control-Expose-Headers' 'Authorization' always;
        ##}
        ##if ($request_method = 'OPTIONS') {
        ##    add_header 'Access-Control-Max-Age' 86400;
        ##    add_header 'Access-Control-Allow-Origin' "$http_origin" always;
        ##    add_header 'Access-Control-Allow-Credentials' 'true' always;
        ##    add_header 'Access-Control-Allow-Methods' 'GET, POST, PUT, DELETE, OPTIONS' always;
        ##    add_header 'Access-Control-Allow-Headers' 'Accept, Authorization, Cache-Control, Content-Type, Keep-Alive, Origin, User-Agent, X-Requested-With,FCM';
        ##    add_header 'Access-Control-Expose-Headers' 'Authorization, X-RateLimit-Limit, X-RateLimit-Remaining, X-RateLimit-Reset' always;
        ##    return 204;
        ##}
    }

    location ~ /\.(?!well-known).* {
        deny all;
    }
}

# FORGE CONFIG (DO NOT REMOVE!)
include forge-conf/app.zavgar.online/after/*;

