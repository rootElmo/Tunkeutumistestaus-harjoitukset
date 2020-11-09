# Harjoitus 1

### a) Hanki kutsu HackTheBoxiin

Suurin osa tämän tehtävän suorituksesta tehtiin yhdessä tiistaisen luennon loppupuolella. Kutsun saaminen ei ollut hirveän monimutkainen prosessi näin jälkeenpäin katsottuna, mutta kun menetelmät olivat uusia niin niiden sisäistämisessä meni hetki.

Tehtävän ideana oli saada kutsu HackTheBox alustalle saamalla kutsukoodi jotenkin selvitettyä sivun [hackthebox.eu/invite](www.hackthebox.eu/invite) syövereistä.

![kuva1](./Kuvat/hackthebox001.png)

Tehtävän alkuun pääsi avaamalla selaimen __Developer Tools__it ja löytämällä vinkin konsolin logista. Jokin mielenkiintoinen JavaScript-tiedosto latautuu sivun latauksen yhteydessä. Kyse on **inviteapi.min.js**-nimisestä tiedostosta.

Tiedosto piti "deobfuskoida", jotta siitä sai jotain selvää. Tämän jälkeen saimme tietoomme linkin, josta löytyisi oletettavasti vinkkejä eteenpäin. Konsolissa voi myös ajaa komennon **makeInviteCode()**, joka antaa salakirjoitetun vinkin. 

Saimme tietoomme, että lähettämällä POST-pyynnön tiettyyn osoitteeseen, saisimme vastineeksi **jotain**. Muistaakseni se, mitä saimme oli BASE64 salakirjoitettu pääsykoodi hacktheboxiin.
