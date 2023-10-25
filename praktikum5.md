# 5. praktikum
**Ül. 5-1:** **a)** Omanikul on vaja vaid luba kataloogi sisenemiseks (x) ja luba faili lugemiseks (r)
**b)** Omanikul on vaja vaid luba kataloogi sisenemiseks (x), sinna kirjutamiseks/kustutamiseks (w) ning luba faili kirjutamiseks (w)

**Ül. 5-2:** See ei ole piisav õigus shelli skriptifaili käivitamiseks, sest shell ei saa lugeda faili (ei tea, mis sisu on, mida käivitada). Tuleks lisada ka lugemisõigus "read".

**Ül. 5-3:** Useradd command lisab igale kasutajale tema nimega grupi. Tiimitöö puhul on tihti hea kasutada näiteks umask 002-te, mis lisab "group read/write" õigused, aga sellel juhul saavad erinevad töölised tiimis muuta teiste privaatfaile, mistõttu on hea, kui iga kasutaja on alguses oma umask 022 grupis, millel on vaid "group read-only" õigused.

**Ül. 5-4:**
<img width="959" alt="Screenshot 2023-10-25 185502" src="https://github.com/Siim0u/ops-steemid/assets/112852891/101abb12-e345-4b6f-b52a-8e6c72303c17">

**Ül. 5-5:** Setuid õigust on vaja, et teised kasutajad (peale omaniku) saaksid faili jooksutada omaniku lubadega, siin et jukuisa, kes muidu ei pääseks faili "hinded.txt"-le ligi, saaks faili korralikult jooksutada.
<img width="960" alt="Screenshot 2023-10-25 194319" src="https://github.com/Siim0u/ops-steemid/assets/112852891/1d87f71a-da14-4dcd-93d1-0b0586610cf5">

**Ül. 5-6:** See võib vähendada süsteemi turvalisust, sest kasutajad, kellel muidu poleks piisavalt õigusi võivad skriptide kaudu pääseda ligi privaatsele infole.

**Ül. 5-7:** Kustutada saavad kasutajad: root, peeter, opetaja
**Ül. 5-8:** # file: hinded.txt

#owner: opetaja

#group: opetaja

user::rw-

group::---

group:direktor:rw-

mask::rw-

other::---
**Ül. 5-9:** Mitte keegi ei saa selle faili sisu modifitseerida. Saate eemaldada atribuudi faililt käsuga: "sudo chattr -i testfail-2" ning peale seda teha "rm testfail-2", et fail ikkagi kustutada.



