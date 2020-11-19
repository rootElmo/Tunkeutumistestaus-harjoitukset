# Harjoitus 3

Harjoituksessa 3 opetellaan käyttämään **nmap**-porttiskanneria, tulkitsemaan sen toimintaa **snifferin** avulla ja tarkastelemaan sen moninaisia toiminnalisuuksia. [Linkki tehtävänantoon](http://terokarvinen.com/2020/tunkeutumistestaus-kurssi-pentest-course-ict4tn027-3006-autumn-2020/#h3). Tehtävän tekoon käytän jo aiemmin asentamaan **Linux Kali**-virtuaalikonetta.

## Lukuläksyt (kohta z)

_Lue artikkelit ja katso videot, tee kustakin muistiinpanot (muutama ranskalainen viiva per artikkeli/video). Tässä z-kohdassa ei tarvitse tehdä mitään kokeita koneella._

### Santos et al: The Art of Hacking - Active Reconnaissance

#### 4.1 ja 4.2

  * Aktiivinen tiedustelu eroaa passiivisesta tiedustelusta sillä, että hyökkääjä/tunkeutuja/testaaja pyrkii aktiivisesti eri keinoin ja työkaluin hankkimaan tietoa uhrista/kohteesta ja/tai tämän tietoverkoista.
  * Kohteiden tiedustelussa on tärkeää laittaa muistiin tiedot tiedustelluista kohteista
  * Aktiivisen tiedustelun vaiheita ovat mm. porttiskannaus, webbipalveluiden silmäily, sekä haavoittuvuuksien kartoitus.

#### 4.3

**Porttiskannaus**

  * Työkaluista tärkeimmät porttiskannauksessa ovat **nmap** yleistä porttiskannausta ja version nuuhkintaa varten, **masscan** suuria porttimääriä skannatessa, sekä **udpprotoscanner** UDP-skannauksia varten.
  * **nmap**ia käytettäessä voidaan parilla parametrilla saada ohjelma ajettua niin, että kohdejärjestelmästä saadaan esiin mm. avoimet, suljetut, sekä suodatetut portit, ja yleistä tietoa järjestelmästä (käyttöjärjestelmä, palveluiden versiot).
  * **masscan**illä voidaan saavuttaa huimia skannausnopeuksia, mm. koko internetin skannaus yhtä porttia varten kestää n. 3 minuuttia _(ainakin masscanin manuaalin mukaan)_
  * **udpprotoscanner**illa tietojärjestelmiä voidaan skannata UDP-palveluiden varalta.

**Webbipalveluiden silmäily**

  * **EyeWitness** soveltuu hyvin mahdollisten webbipalveluiden silmäilyyn, sekä tarkasteluun teknisestä näkökulmasta.
  * Ajettaessa ohjelma käy läpi kohdejärjestelmästä mahdollisesti löytyvät webbipalvelut, ottaa niistä kuvankaappaukset, listaa yleistä tietoa, sekä tarjoaa käyttäjälle mahdollisia haavoittuvuuksia, mitä hyödyntää jälkeenpäin.

**Haavoittuvuuksien kartoitus**

  * Voidaan etsiä verkon haavoittuvuuksia, sekä webbipalveluiden haavoittuvuuksia.
  * Tietoverkon haavoittuvuuksien kartoituksiin käy mm. ohjelmat **OpenVAS**, **Nessus**, sekä **nmap**, mutta vain rajoitetusti.
  * Webbipalveluiden haavoittuvuuksien kartoituksiin käy mm. **Nikto**, **WPScan**, sekä **SQLMap**
  * Esimerkiksi **nmap**illa haavoittuvuuksien kartoitus tapahtuu ohjelmaan sisäänrakennetun _script engine_n avulla.


### Nmap Network Scanning -  Chapter 1. Getting Started with Nmap



## Lähteet

1. [Tero Karvinen](http://terokarvinen.com/2020/tunkeutumistestaus-kurssi-pentest-course-ict4tn027-3006-autumn-2020/#h3)
2. [O'Reilly - Santos et al](https://learning.oreilly.com/videos/the-art-of/9780135767849/9780135767849-SPTT_04_00)
3. [nmap - Gordon Lyon](https://nmap.org/book/nmap-overview-and-demos.html)