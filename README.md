[Unit]
Description=daphne daemon
Requires=daphne.socket
After=network.target

[Service]
Type=simple
User=webserver-ubuntu
WorkingDirectory=/home/webserver-ubuntu/root/ticket-back
ExecStart=/home/webserver-ubuntu/root/ticket-back/venv/bin/daphne -e ssl:8000:>
Restart=on-failure
[Install]
WantedBy=multi-user.target



