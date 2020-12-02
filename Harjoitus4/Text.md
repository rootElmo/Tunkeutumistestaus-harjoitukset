# Harjoitus 4

[tero karvinen](http://terokarvinen.com/2020/tunkeutumistestaus-kurssi-pentest-course-ict4tn027-3006-autumn-2020/)

## Kohta z

Lue artikkelit ja katso videot, tee kustakin muistiinpanot (muutama ranskalainen viiva per artikkeli/video).

### Writeup - IppSec

[Linkki videoon](https://www.youtube.com/watch?v=GKq4cwBfH24)

  * Helpohko kone, CMSMadeSimplessä haavoittuvuus, **SearchSploitilla** haavoittuvuuden tunnistaminen, valmiin hyökkäyksen hyväksikäyttö ja tulokseksi saadun suolatun MD5-hashin murto **hashcatillä**, jolla saadaan käyttäjätunnukset. 
  * Aktiivisen tiedustelun aikana **nmapista** paljastuu portissa **80** pyörivä webbipalvelu, sekä **robots.txt**-tiedosto, joka kertoo hakukoneille, että sivua **koneen-ip/writeup** ei tulisi indeksoida. **writeup**-sivulta huomataan, että sivu on generoitu **CMSMadeSimplellä**. **SearchSploitilla** **CMSMS**:stä löytyi useita valmiita haavoittuvuuksia.
  * **SQL-injektio** haavoittuvuus käytiin läpi **BurpSuiten** avulla. Injektiolla saatiin eräs sähköposti, salakirjoitettu salasana, sekä salasanan suola. Salasana kräkättiin **hashcatillä**.
  * Kun käyttäjä oli korkattu, kohdekone enumeroitiin **LinEnumilla** sen jälkeen kun **SSH**-yhteys oltiin avattu.

### Heist - IppSec

[Linkki videoon](https://www.youtube.com/watch?v=fmBb6BgLsC8)

  * Aktiivisessa tiedustelussa paljastui, että kohdekoneessa on auki portti **80**, jossa pyörii **Microsoft IIS**. Kohdekone vaikuttaa myös olevan **Windows Server**. Myös portit **135**, sekä **445** olivat auki.
  * Kohdekoneen webbipalvelusta löytyi kirjautumissivu, jonne pystyi kirjautumaan mnyös vieraana. Vieraana kirjauduttaessa näkyi sivulal aikaisemmin käyty keskustelu, sekä liitteenä oleva tiedosto. Nämä sisälsivät jonkin **Cisco**-laitteen konfiguraatiot ja pari salasanaa. Salasanat murtuivat helposti valmiin **Python**-skriptin avulla.
  * **crackmapexec** käyttäjien, sekä salasanojen enumerointiin.
  * **mfsdb**, **procdump64.exe**


[0xdf - HTB Adminer](https://0xdf.gitlab.io/2020/09/26/htb-admirer.html)
[0xdf - HTB Frolic](https://0xdf.gitlab.io/2019/03/23/htb-frolic.html)

## kohta a, Etsi ja kokeile 5 uutta työkalua jostain lukemastasi/katsomastasi läpikävelystä.

### gobuster

Aloitin tiedustelemalla **Doctor**-nimisen koneen **HackTheBoxin** verkosta **nmapilla**.

  $ sudo nmap 10.10.10.209 -sT -sV -sC -oA doctor

Koneesta paljastui **Apache** pyörimässä portissa **80**, **SSH** portissa **22**, sekä jokin **splunkd** niminen demoni portissa **8089**. Samasta portista tulee myös ilmoitus **robots.txt**-tiedoston olemassaolosta, joka viittaisi siihen, että hakukoneiden indeksoinnilta halutaan estää jokin sivusto.

![gobust001.png](./kuvat/gobust001.png)

Lähdin ajamaan **gobusteria** aluksi **rockyou.txt**-sanakirjalla, mutta tajusin sen kestävän ikusuuksia, joten tarkistin sanalistat uudestaan ja ajoin komennon uudestaan **dirbusterin** sanakirjalla. Käytin myös **-r**-lippua, jolloin **gobuster** ei jatkaa crawlausta redirectin vastaan tullessa.

  $ gobuster dir -u 10.10.10.209 -w /usr/share/wordlists/rockyou.txt 

  $ gobuster dir -r -u 10.10.10.209 -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt 

Medium-sanalistakin oli suhteellisen pitkä, joten ajoin uudestaan pienemmällä sanalistalla

  $ gobuster dir -u 10.10.10.209 -w /usr/share/wordlists/dirbuster/directory-list-2.3-small.txt

Tulokseksi en saanut valitettavasti mitään jännittävää. Lähinnä perus html, js, images, sekä fonts kansiot löytyivät.

![gobust002.png](./kuvat/gobust002.png)

### BurpSuite

BurpSuite on yleihyödyllinen skannaus- ja monitoimityökalu webbi-pohjaisten haavoittuvuuksien tarkasteluun ja hyödyntämiseen.

Asetin **Firefoxissa** proxyn localhostiin, porttiin 8080, josta nappaisin pyyntöjä **BurpSuitella**.

![burp001.png](./kuvat/burp001.png)

Kokeilen kaapata pyyntöjä **HackTheBoxin** **Time**-koneen webbisivulta, sekä muokata niitä ja lähettää eteenpäin manuaalisesti. Käytin webbipalvelimen lomaketta lähettääkseni kokeeksi "Test123" ja katsoisin **BurpSuitesta** pyyntöä.

![burp002.png](./kuvat/burp002.png)

Pyyntö, sekä syöttämäni teksti osui haaviin. Nyt voisin muokata pyyntöä kuinka näkisin sopivaksi. **BurpSuitella** pystyisi esimerkiksi kokeilemaan helposti **SQL-injektioita**. Laiton pyyntöni data-parametriksi "foobar123" ja lähetin pyynnön.

![burp003.png](./kuvat/burp003.png)

Sivusto valittaa saaneensa syötteen "foobar123", joten pyynnön kaappaus ja muokkaus onnistui **BurpSuitella**.


### 




## kohta b, Tiedustele ja analysoi 5 htb konetta perusteellisesti

## kohta c, Nimeä 1-3 walktrough:ta, joissa tunkeudutaan samantapaisiin palveluihin, joita käsittelit kohdassa b


# Lähteet

1. [Tero Karvinen](http://terokarvinen.com/2020/tunkeutumistestaus-kurssi-pentest-course-ict4tn027-3006-autumn-2020/)
2. [Youtube - IppSec - Writeup](https://www.youtube.com/watch?v=GKq4cwBfH24)
3. [0xdf - HTB Adminer](https://0xdf.gitlab.io/2020/09/26/htb-admirer.html)
4. [0xdf - HTB Frolic](https://0xdf.gitlab.io/2019/03/23/htb-frolic.html)
