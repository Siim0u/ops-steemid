# 12. praktikum

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

<code>#!/bin/bash
echo "Sisestage protsess:"
read protsess
ps -A | awk -v protsess="$protsess" '$0 ~ protsess {print "Protsess:", $4, "PID:", $1}'</code>
