#!/bin/sh

id_card()
{
name=$(hostname)
echo "Machine name: $name"

ip=$(ip a | grep inet | grep 192. | cut -d' ' -f6)
echo "IP: $ip"

kversion=$(uname -r)
os=$(cat /etc/issue | cut -d' ' -f1)
echo "OS $os and kernel version is $kversion"

ramtotal=$(free -mh | grep Mem: | cut -c 15-20)
ramfree=$(free *mh | grep Mem: | cut -c 38-44)
echo "Ram :$ramfree RAM restante sur $ramtotal RAM totale "

disc=$(df -h | grep sda2 | cut -c 29-32)
echo "Disque : $disc space left"

process=$(ps -eo pid,ppid,cmd,%mem,%cpu --sort=-%mem | head -n 6 | tail -n 5)
echo -e "Top 5 processes by RAM usage :\n$process"

port=$(sudo ss -latnp | grep LISTEN)
echo -e "Listening ports :\n$port"

chat=$(curl https://cdn2.thecatapi.com/v1/images/search | jq -r '.[].url')
echo -e "\nHere's your random cat : $chat"