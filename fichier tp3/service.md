#!/bin/sh

while [ -s /srv/yt/vdotodownload ]
do
if [ -d "/srv/yt/downloads" ];then
url=$(head -n 1 "/srv/yt/vdotodownload")
title=$(sudo youtube-dl --get-title $url)
sudo mkdir "/srv/yt/downloads/$title" >> "/dev/null"
sudo youtube-dl -f mp4 -o "/srv/yt/downloads/$title/%(title)s" $url >> "/dev/null"
sudo touch "/srv/yt/downloads/$title/description" && sudo chmod 777 "/srv/yt/downloads/$title/description" >> "/dev/null"
youtube-dl --get-description $url >> "/srv/yt/downloads/$title/description"
echo Video $url was downloaded
echo File path : /srv/yt/downloads/"$title"/"$title".mp4
sudo sed -i '1d' /srv/yt/vdotodownload
sudo sed '$d' /srv/yt/vdotodownload
if [ -d "/var/log/yt" ];then
echo [$(date "+%Y/%m/%d %T")] Video $url was downloaded. File path : /srv/yt/downloads/"$title"/"$title".mp4 >> /var/log/yt/download.log
else
        exit
fi
else
        exit
fi


done