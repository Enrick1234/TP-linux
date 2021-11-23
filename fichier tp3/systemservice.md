[Unit]
Description=dl video yt

[Service]
ExecStart= sudo bash /usr/local/bin/yt-v2.sh

[Install]
WantedBy=multi-user.target