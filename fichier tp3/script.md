#!/bin/sh

title=$(sudo youtube-dl --get-title $1)
#description=$(youtube -dl --get-description https://www.youtube.com/watch?v=UO_QuXr521I >> /srv/yt/downloads/$title/de>#file=$(sudo touch /srv/yt/downloads/$title/description && sudo chmod 777 /srv/yt/downloads/$title/description)
date=$(date)

if [ -d "/srv/yt/downloads" ];then
sudo mkdir "/srv/yt/downloads/$title" >> "/dev/null"
sudo youtube-dl -f mp4 -o "/srv/yt/downloads/$title/%(title)s" $1 >> "/dev/null"
sudo touch "/srv/yt/downloads/$title/description" && sudo chmod 777 "/srv/yt/downloads/$title/description" >> "/dev/nul>youtube-dl --get-description $1 >> "/srv/yt/downloads/$title/description"
echo Video $1 was downloaded
echo File path : /srv/yt/downloads/"$title"/"$title".mp4
if [ -d "/var/log/yt" ];then
echo File path : [$(date "+%Y/%m/%d %T")] Video $1 was downloaded. File path : /srv/yt/downloads/"$title"/"$title".mp4 >else
        exit
fi
else
        exit
fi