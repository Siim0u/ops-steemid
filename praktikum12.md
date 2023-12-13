# 12. praktikum
12. praktikumis õppisin, kuidas Linuxis skriptida. Sain teada, kuidas töötavad põhifunktsioonid, näiteks if-else, for, while. Lõin ka ise funktsiooni ning tegelesin ka failide haldusega.
# ÜL 1
<code>#!/bin/sh
echo "Sisestage nimi:"
read nimi
echo "Sisestage eriala:"
read eriala
echo "Sisestage matriklinumber:"
read matriklinumber
echo $nimi
echo $eriala
echo $matriklinumber</code>
# ÜL 2
<code>#!/bin/bash
read A
read B
for i in $(ls); do
    if [ ${i##*.} = $A ]; then
        mv $i ${i/$A/$B}
    fi
done</code>
# ÜL 3
<code>#!/bin/bash
echo "Sisestage protsess:"
read protsess
ps -A | awk -v protsess="$protsess" '$0 ~ protsess {print "Protsess:", $4, "PID:", $1}'</code>
# ÜL 4
<code>#!/bin/bash
astenda() {
  arv=$1
  eksponent=$2
  if [ $eksponent -eq 0 ]; then
    echo 1
  else
    result=$((arv * $(astenda $arv $((eksponent - 1)))))
    echo $result
  fi
}
echo $(astenda 9 5)</code><img width="842" alt="Kuvatõmmis 2023-12-13 202014" src="https://github.com/Siim0u/ops-steemid/assets/112852891/c91cb07e-a0db-4aff-bde5-79ae23cba471">
<img width="953" alt="Kuvatõmmis 2023-12-13 213252" src="https://github.com/Siim0u/ops-steemid/assets/112852891/c16aae75-8ae1-4e56-a32c-032636626d1e">
<img width="865" alt="Kuvatõmmis 2023-12-13 212709" src="https://github.com/Siim0u/ops-steemid/assets/112852891/4397193b-9e69-4628-928c-da9086a84570">
<img width="887" alt="Kuvatõmmis 2023-12-13 212125" src="https://github.com/Siim0u/ops-steemid/assets/112852891/ca887971-1198-4bfe-a680-8001f0292cb8">
oading Kuvatõmmis 2023-12-13 202014.png…]()
<img width="794" alt="1" src="https://github.com/Siim0u/ops-steemid/assets/112852891/cabc0ecc-3878-42e1-8c50-645bd657072b">
