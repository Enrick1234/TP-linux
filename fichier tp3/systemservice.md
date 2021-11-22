[Unit]
Description=dl video yt

[Service]
ExecStart= sudo bash /usr/local/bin/yt.service

[Install]
WantedBy=multi-user.target