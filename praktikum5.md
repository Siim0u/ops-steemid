# 5. praktikum
**Ül. 5-1:** **a)** Omanikul on vaja vaid luba kataloogi sisenemiseks (x) ja luba faili lugemiseks (r)
**b)** Omanikul on vaja vaid luba kataloogi sisenemiseks (x), sinna kirjutamiseks/kustutamiseks (w) ning luba faili kirjutamiseks (w)

**Ül. 5-2:** See ei ole piisav õigus shelli skriptifaili käivitamiseks, sest shell ei saa lugeda faili (ei tea, mis sisu on, mida käivitada). Tuleks lisada ka lugemisõigus "read".

**Ül. 5-3:** Useradd command lisab igale kasutajale tema nimega grupi. Tiimitöö puhul on tihti hea kasutada näiteks umask 002-te, mis lisab "group read/write" õigused, aga sellel juhul saavad erinevad töölised tiimis muuta teiste privaatfaile, mistõttu on hea, kui iga kasutaja on alguses oma umask 022 grupis, millel on vaid "group read-only" õigused.
