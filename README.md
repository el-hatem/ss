[Unit]
Description=daphne daemon
Requires=daphne.socket
After=network.target

[Service]
Type=simple
User=webserver-ubuntu
WorkingDirectory=/home/webserver-ubuntu/root/ticket-back
ExecStart=/home/webserver-ubuntu/root/ticket-back/venv/bin/daphne -e ssl:8000:privateKey=/home/webserver-ubuntu/root/ticket-back/ssl/webapp.key:certKey=/home/webserver-ubuntu/root/ticket-back/ssl/webapp.crt -b 0.0.0.0 -p 2504 TecketingABC.asgi:application
Restart=on-failure
[Install]
WantedBy=multi-user.target
