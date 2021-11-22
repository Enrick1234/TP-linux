#!/bin/sh

nbrline=$(wc -l /srv/yt/vdotodownload | cut -d ' ' -f1)
turn=1
for ((i=1; i<=$nbrline; i++))
do
line=$(sed -n '$ip' /srv/yt/vdotodownload)
title=$(sudo youtube-dl --get-title $line)
#description=$(youtube -dl --get-description https://www.youtube.com/watch?v=UO_QuXr521I >> /srv/yt/downloads/$title/de>#file=$(sudo touch /srv/yt/downloads/$title/description && sudo chmod 777 /srv/yt/downloads/$title/description)
date=$(date)

if [ -d "/srv/yt/downloads" ];then
sudo mkdir "/srv/yt/downloads/$title" >> "/dev/null"
sudo youtube-dl -o "/srv/yt/downloads/$title/%(title)s" $line >> "/dev/null"
sudo touch "/srv/yt/downloads/$title/description" && sudo chmod 777 "/srv/yt/downloads/$title/description" >> "/dev/nul>youtube-dl --get-description $line >> "/srv/yt/downloads/$title/description"
echo "Video $line was downloaded"
if [ -d "/var/log/yt" ];then
echo "File path : "$(date "+%Y/%m/%d %T") >> "/var/log/yt/download.log"
else
        exit
fi
else
        exit
fi