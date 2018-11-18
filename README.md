# dreamteam

<h1>Changelog</h1>
18.Nov.2018 : Fixed scrape.js

# Run Reatjs with PM2
$ pm2 start npm -- start

# Nginx config SSL for Nodejs


	limit_conn_zone $binary_remote_addr zone=conn_limit_per_ip:10m;
	limit_req_zone $binary_remote_addr zone=req_limit_per_ip:10m rate=5r/s;
	limit_req_status 444;
	limit_conn_status 503;

	proxy_cache_key "$scheme$request_method$host$request_uri$is_args$args";
	proxy_cache_valid 404 1m;

    upstream dreamteam.hlvbongda.com {
        server localhost:3000;
   		 }

	server {
    listen 80;
    server_name example.com;

    location / {
        proxy_pass http://localhost:3000;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
   	 }
	}   

	server {
  	  listen 443 ssl http2;
   	 server_name example.com;

      ssl on;
        ssl_stapling on;
        ssl_stapling_verify on;
  	# SSL
	ssl_certificate /etc/letsencrypt/live/example.com/fullchain.pem;
	ssl_certificate_key /etc/letsencrypt/live/example.com/privkey.pem;
	ssl_protocols TLSv1 TLSv1.1 TLSv1.2; 
	ssl_prefer_server_ciphers on; 
	ssl_ciphers EECDH+CHACHA20:EECDH+AES128:RSA+AES128:EECDH+AES256:RSA+AES256:EECDH+3DES:RSA+3DES:!MD5;

	root /opt/app/public;

        ##
        # `gzip` Settings
        #
        #
        gzip on;
        gzip_disable "MSIE [1-6]\.";

        gzip_vary on;
        gzip_proxied any;
        gzip_comp_level 7;
        gzip_buffers 16 8k;
        gzip_http_version 1.1;
        gzip_min_length 256;
        gzip_types text/plain text/css application/json application/x-javascript text/xml application/xml application/xml+rss text/javascript application/vnd.ms-fontobject application/x-font-ttf font/opentype image/svg+xml image/x-icon;

      location / {
                try_files $uri @proxy;
        }
 	location @proxy {
                proxy_pass http://localhost:3000;
                proxy_http_version 1.1;
                proxy_set_header Upgrade $http_upgrade;
                proxy_set_header Connection 'upgrade';
                proxy_set_header Host $host;
                proxy_cache_bypass $http_upgrade;
       	 }

	}
