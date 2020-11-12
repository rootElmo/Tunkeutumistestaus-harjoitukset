# !!!! TYÖN ALLA !!!!


# XSS iso teht:

Aloitin avaamalla dev toolit F12. Ajoin lomakkeen kerran läpi jo annetuilla arvoilla ja katsoin, millaisen viestin sain.

Heti hälytti, sillä selain tulostaa käyttäjälle syötteen, jossa on kentästä "Enter your creddit card number" käyttäjän syöttämä luottokortin tunnus. Tämän oltiin sanottu olevan yksi mahdollinen hyökkäyksen mahdollistavista paikoista.

Seuraavaksi olisi aika etsiä sopivaa hyökkäyspintaa.

Katsoin Devtoolsien Network välilehdestä, jos lähettämäni pyyntö olisi antanut jotain jännää. Ei ainakaan mikään osunut silmään. Arvelen, että tulisi taas muokata itse HTML:ää, jotta saisin jonkin Javascript pätkän ajettua. (Try again. We do want to see this specific JavaScript (in case you are trying to do something more fancy).

Ajoin seuraavaksi pari komentoa, ihan vruteforce mielessä, ilman kummempaa taka-ajatusta.

    alert(/WebGoat/CrossSiteScripting/attack5a);
    alert(document./WebGoat/CrossSiteScripting/attack5a);

Ajattelin tämän antavan jotain vänkää ulos, mutta eipä tullut.

Katsoin pari vinkkiä sivun yläkulmasta. Vinkit antoivat jo pari sellaista mitä olin oivaltanut itse (mikä tieto "echotaan" takaisin käyttäjälle), mutta sitten tajusin, että minun pitäisi syöttää haluamani JS johonkin tekstikenttään sen ajamiseksi. Katsoin takaperin ohjeita ja vinkkejä harjoituksen muista kohdista ja mietin, voisinko laitaa `<script>`-tägien sisään jotain, jolla saisin "alertoitua" itselleni tuon luottokortti tiedon.

    4128 3214 0002 1999 <script>alert(document.field1)</script> // ei toiminut, mutta alert näkyi! (xss_alert001)

    <script>alert(document.field1.value)</script> // tehtävä meni läpi, mutta mitään ei näkynyt

alert(document.getElementsByClassName("field1").value); ajettu consolessa, palautti undefined

TAJUSIN ETTÄ:
    - hain väärällä tapaa field1:n arvoa. Olisi pitänyt hakea getElementsByName:lla, sillä getElementsByClassName hakee classin nimen mukaan. getElementsByName palauttaa myös kaikki instanssit listana (???) joten olisi tarvinnut määritellä, mikä instanssi noista field1:stä palautetaan (getElementsByName('foobar')[0].value).

loppuun xss_alert002.png

LISÄYSTÄ:

Kokeiltu, ei feedbäkkiä

    <script>
        function function123(){
            var x = document.getElementsByName("field1")[0].value;
            alert(x);
        }
    </script>

    <script>
        function function123(){
            var parent = document.getElementById("lessonContent");
            var x = document.getElementsByName("field1")[0].value;
            alert(x);
        }
    </script>

    <script>function function123(){var x = document.getElementsByName("field1")[0].value; alert(x);}</script>


LÄHTEET: https://developer.mozilla.org/en-US/docs/Web/API/Document/getElementsByClassName Tsekkasin JS,
https://www.w3schools.com/jsref/met_doc_getelementsbyname.asp kokeiluja


# XS REQUEST FORGERY:

Basic Get CSRF:

Avasin myös webwolfin taustalle

    $java -jar webwolf-se-ja-se
    localhost:9090/webwolf
    incoming requests

painettuani get query, avasi webgoat uuden välilehden jossa ilmoitettiin, etten ollut onnistunut, sillä pyyntö tuli samalta hostilta (KUVA seasurf001). Minun pitäisi jostain saada siis "painettua" query selaimen ulkopuolelta tai toisen sivun kautta, jotta onnistuisin.

Löysin Inspectorista formista kohdan "target". Tämä vaikuttaisi suuntaavaan queryn tiettyyn paikkaan. https://www.w3schools.com/tags/att_form_target.asp

(KUVA seasurf002)

targetin muuttaminen http://webwolf/landing/* muotoon ei tarjonnut vastausta. Tajusin, että eihän tässä pyydetä lomakkeen tietojen lähetystä ulos, vaan lomakkeen "submittausta" ulkopuolelta.

Loin nopeasti webbisivun attack.html johon sijoitin:

/////// LAITA TÄHÄN attack001.html //////////////

Pistin seuraavaksi helpon serverin pystyyn, jos sillä vaikka tapahtuisi.

    $ python3 -m http.server

ja navigoin tiedostoon attack001.html (attack001.html oli alkuperäinen)

kuitenkin sivusto vinkui virhettä syötteessä (seasurf003).

LISÄSIN attack001.html:ään formiin "accept-charset='unknown'", sekä otin action:stä pois localhostin ja portin. Sain erilaisen virheilmoituksen. Sanoo, ettei POST toimi. Taidampa kokeilla GETiä. (seasurf004)

HUOMASIN TÄSSÄ VAIHEESSA, että olin tuijottanut Confirm Flag formin koodia inspectorissa ja käyttäytynyt sen mukaan. Ei ihme, ettei mikään pelitä. Tässä oli parin voimasanan mittainen huokaustuokio.

Katsoin parit vinkit sivun yläpalkista ja siellä sanottiin, että formissa on piilotettuja inputteja. Toden totta siellä olikin ja lisäsin tuon inputin omaan webbisivuuni. Formin targetiksi laitoin "_blank", sekä ohjasin pyynnön "localhost:8080/WebGoat.....", että POST menisi suoraan oikealle palvelimelle.

Onnistuin saamaan vastauksen! (seasurf005)

Laitoin vastauksessa tulleen flagin alempaan lomakkeeseen ja sain onnistuneesti tehtävän läpi.
