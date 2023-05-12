upstream ws {
 server web-app.abchospitaleg.com:8000;
}

server {
    listen 8001 ssl;  
    
    server_name web-app.abchospitaleg.com;
    
    root /var/www/html/web-app.abchospitaleg.com;     
    
    ssl_certificate /home/webserver-ubuntu/root/ticket-back/ssl/webapp.crt;
    ssl_certificate_key /home/webserver-ubuntu/root/ticket-back/ssl/webapp.key;
    

    location = /favicon.ico { access_log off; log_not_found off; }
    
    location /staticfiles/ {
        root /home/webserver-ubuntu/root/ticket-back;
    }
    
    location /static/ {
        root /home/webserver-ubuntu/root/ticket-back;
    }

    location /media/ {
       root /home/webserver-ubuntu/root/ticket-back;
    }

    location / {
        include proxy_params;
        proxy_set_header X-Forwarded-Host $host;
        proxy_set_header X-Forwarded-Server $host;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
	proxy_headers_hash_max_size 512;
	proxy_headers_hash_bucket_size 128; 
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-Proto $scheme;
        proxy_pass http://unix:/run/daphne.sock; 
        return 301 https://web-app.abchospitaleg.com;       
   }
    location /ws/ {
        proxy_pass https://ws/;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection $connection_upgrade;
        proxy_read_timeout 86400;
        proxy_send_timeout 86400;
    }   
    
   error_log /home/webserver-ubuntu/root/ticket-back/logs/error.log error;

}
