# 6. praktikum
**Ül. 3** Käsk: ps -axu | grep daemon | tr -s "" | cut -68-
**Ül. 4** Käsk: ip a | grep inet | sed '3!d' | cut -d '/' -f 1 | tr -d inet > ipaddress.txt
