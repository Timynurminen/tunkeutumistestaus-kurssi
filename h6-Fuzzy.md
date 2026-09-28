# h6 Fuzzy

## x) Lue/katso/kuuntele

### Karvinen 2023: Find Hidden Web Directories - Fuzz URLs with ffuf

- Ffuf on gobusterin/dirbusterin tapainen työkalu piilotettujen web-hakemistojen löytämiseen. (Kokeilee sanakirjan jokaista riviä osana URL:ia FUZZ-avainsanan paikalla)
- Käytännön ongelma: jos palvelin vastaa kaikkeen HTTP 200:lla, pelkkä status-koodi ei riitä suodattamiseen. Silloin pitää katsoa muita yhteisiä piirteitä väärillä osumilla ja suodattaa ne pois.(`-fs`, `-fw`, `-fl`, `-fc`, `-ft`)
- Artikkelissa käytetään SecListsin `common.txt`-sanakirjaa, joka sisältää lähes 5000 yleistä web-polkua.
- Vasteajalla (`-ft`) suodattaminen mainitaan epäluotettavimmaksi vaihtoehdoksi, koska se voi vaihdella sattumanvaraisesti.

**Oma huomio:** En olisi uskonut että pelkkä "200 OK" voi olla harhaanjohtava. Luulisi että se tarkoittaa aina "sivu löytyi oikeasti". Pitää siis muistaa aina tarkistaa muutama satunnainen osuma käsin ennen kuin luottaa tuloksiin sellaisenaan.

**Lähteet:**

- [Karvinen 2023: Find Hidden Web Directories - Fuzz URLs with ffuf](https://terokarvinen.com/2023/fuzz-urls-find-hidden-directories/)

### Hoikkala 2023: ffuf README.md

- Perus-fuzzauskomento on aina samanmuotoinen: `FUZZ`-avainsana korvataan URL:ssa, headerissa(`-H`) tai POST-datassa(`-d`) sanakirjan riviarvoilla.
- Ffuf tukee virtuaalihostien (vhost) löytämistä ilman DNS-tietueita: fuzzataan `Host`-headeria ja suodatetaan pois oletusvastauksen kokoinen vastaus(`-fs`)
- `-recursion`-lippu antaa ffufin jatkaa automaattisesti syvemmälle löydettyihin alihakemistoihin, ja `-maxtime-job` rajoittaa yhden rekursiivisen työn kestoa niin ettei koko ajo jää jumiin yhteen hakemistoon.
- Interaktiivinen tila (paina ENTER kesken ajon) mahdollistaa suodattimien säätämisen lennossa ilman ajon keskeyttämistä. Uudet suodattimet vaikuttavat takautuvasti muistissa oleviin tuloksiin, mutta jo hylättyjä ("negative") osumia ei saa takaisin ilman `restart`-komentoa.
- README mainitsee myös `ffuf.me`-harjoitusympäristön (Adam Langley), jota voi ajaa paikallisesti Dockerilla tai käyttää live-versiona.

**Oma huomio:** Yllättävää oli ylipäätänsä se, että suodattimia voi muokata kesken ajon. Olisin olettanut että fuzzaus pitää aina keskeyttää ja käynnistää uudelleen jos huomaa suodattimen olevan väärä.

**Lähteet:** [Hoikkala 2023: ffuf README.md](https://github.com/ffuf/ffuf/blob/master/README.md)

