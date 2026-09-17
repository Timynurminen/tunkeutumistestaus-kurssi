# h4 Täysin Laillinen Sertifikaatti

## x) Lue/katso ja tiivistä.

### OWASP 2021: OWASP Top 10:2021

- Broken Access Control tarkoittaa tilannetta, jossa käyttäjä pystyy tekemään asioita tai näkemään tietoja, joihin hänellä ei pitäisi olla oikeuksia.
- Haavoittuvuus voi mahdollistaa esimerkiksi toisen käyttäjän tietojen katselun tai muokkaamisen, ylläpitäjän toimintojen käyttämisen tai suojatutihin sivuihin pääsyn ilman oikeita käyttöoikeuksia.
- Yleisiä esimerkkejä ovat IDOR, URL-osoitteidein tai parametrien muokkaaminen, forece browsing sekä virheellisesti toteutetut API-käyttäoikeudet.
- Myös path traversal kuuluu tähän kategoriaan, koska sen avulla voidaan päästä käsiksi tiedostoihin tai hakemistoihin, joiden ei pitäisi olla käyttäjän saatavilla.
- OWASPin datassa Broken Access Control oli erittäin yleinen, sitä testattiin 94%:ssa sovelluksista ja havaintoja oli yli 318 tuhatta.
- Suojatumisessa tärkeää on tehdä käyttöoikeustarkistukset palvelinpuolella, käyttää oletuksena estävää mallia ja varmistaa, että käyttäjä pääsee käsiksi vain omiin tai hänelle sallittuihin resursseihin.
- Käyttöoikeusvirheitä kannattaa myös lokittaa, valvoa ja testata osana sovelluksen testausta.

**Oma huomio:** Minusta Broken Access Control vaikuttaa erityisen vaaralliselta siksi, että hyökkääjänä ei aina tarvitse löytää moniumutkaista teknistä haavoittuvuutta. Jos käyttöoikeudet on toteutettu huonosti, joskus pelkkä URL:n tai tunnisteen muuttaminen voi riittää pääsemään tietoihin, joihin käyttäjällä ei pitäisi olla pääsyä.

### Portswigger Academy:

#### Insecure Direct Object References (IDOR)

- IDOR tarkoittaa tilannetta, jossa sovellus käyttää käyttäjän syöttämää tunnistetta (esim. ID, tiedostonimi) suoraan objektin hakemiseen ilman riittäviä käyttöoikeustarkistuksia.
- Haavoittuvuus johtaa usein horisontaalisen etuoikeuksien laajentamiseen, mutta voi myös mahdollistaa vertikaalisen laajentamisen.
- Tyypillinen esimerkki: URL-parametrin muuttaminen, kuten *customer_number=132355* -> hyökkäjä vaihtaa arvon ja näkee muiden asiakkaiden tiedot.
- IDOR voi esiintyä myös tiedostojen suorissa viittauksissa, kuten *static/12144.txt*, jolloin hyökkääjä voi lukea muiden käyttäjien tallenteita.
- Ydinongelma on se, että sovellus luottaa liikaa käyttäjän syötteeseen eikä tarkista, onko käyttäjällä oikeus kyseiseen resurssiin.

**Oma huomio:** IDOR on pelottavan yksinkertainen, joskus hyökkääjän ei tarvitse tehdä mitään teknisesti vaikeaa, vaan pelkkä URL-parametrin muuttaminen riittää.

#### Path Traversal

- Path traversal tarkoittaa haavoittuvuutta, jossa hyökkääjä voi lukea tai joskus kirjoittaa mihin tahansa tiedostoon palvelimella manipuloimalla tiedostopolkuja.
- Tyypillinen hyökkäys: URL-parametrin muuttaminen, esim. *filename=../../../etc/passwd*, jolloin sovellus lukee järjestelmän tiedostoja kuvatiedoston sijaan.
- Haavoittuvuus syntyy, kun sovellus liittää käyttäjän syötteen suoraan tiedostopolkuun ilman tarkistuksia.
- Mahdollistaa pääsyn sovelluksen lähdekoodiin, salasanoihin, konfiguraatioihin ja käyttöjärjestelmän tiedostoihin.
- Yleisiä kiertotapoja: URL-enkoodaus, netsed traversal, null byte -bypass tai absolute path -viittaukset

**Oma huomio:** Path traversal on klassinen esimerkki siitä, miten pieni kehityksen huolimattomuus voi avata kok palvelimen. (pelkästään *../* voi riittää murtoon)

#### Cross-Site Scripting (XSS)

- XSS mahdollistaa haitallisen JavaScriptin suorittamisen uhrin selaimessa, jolloin hyökkääjä voi esiintyä uhrina ja käyttää sovellusta hänen oikeuksillaan.
- XSS syntyy, kun käyttäjän syöte palautetaan HTML-vastaukseen ilman turvallista käsittelyä.
- Kolme päätyyppiä: Reflected, Stored, DOM-based.
- Hyökkääjä voi varastaa istuntoja, lukea dataa, suorittaa toimintoja, muokata sivua tai lisätä haitallista toiminnallisuutta.
- Suojautuminen
- - Syötteen suodatus (whitelist).
  - Output-enkoodaus (HTML, JS, URL, CSS).
  - Oikeat HTTP-otsakkeet (*Content-Type*, *X-Content-Type-Options*):
  - Content Security Policy viimeisenä puolustuslinjana.
- XSS on yksi yleisimmistä web-haavoittuvuuksista, vaikka todellisia hyökkäyksiä nähdään harvemmin.
- XSS kohdistuu käyttäjiin, kun taas SQL-injektio kohdistuu palvelimen tietokantaan.

**Oma huomio:** XSS on vaarallinen juuri siksi, että se syntyy helposti. Yksi väärin käsitelty syöte voi avata hyökkääjälle koko sovelluksen käyttäjän oikeuksilla.


## a) Totally Legit Sertificate


### Ensimmäisenä päivitettiin kali ja asennettiin ZAP-proxy.

<img width="757" height="583" alt="image" src="https://github.com/user-attachments/assets/4b92c159-e219-47a2-b158-468c504d1927" />

Tämän jälkeen käynnistettiin se komennolla:

```bash
zaproxy
```

### CA-sertifikaatin generointi

Sertfikaatin luonti löytyy Tools -> Options -> Network -> Server Certificates

<img width="772" height="582" alt="image" src="https://github.com/user-attachments/assets/1c81bbeb-3961-4d1d-a76f-d04604d8190f" />

Generoin uuden CA-sertifikaatin ja tellensin sen `.cer` -tiedostoksi levylle.

<img width="683" height="367" alt="image" src="https://github.com/user-attachments/assets/e4b4bd78-e934-4d20-92b5-405c04825e12" />

### CA-sertifikaatin tuonti selaimeen

CA-sertifikaatin luottamus: Firefox -> Settigs -> Privacy & Security -> Certificates -> View Certificates -> Authorities -> Import -> etsi tallennettu `.cer` tiedosto.

<img width="670" height="476" alt="image" src="https://github.com/user-attachments/assets/f93a7ff8-c476-43ca-8666-b68dd810e923" />


<img width="780" height="306" alt="image" src="https://github.com/user-attachments/assets/fd85632d-e36b-4b3a-a248-02b33567f544" />

### ZAP proxyksi selaimeen

Asetin ZAP:n selaimen proxyksi FoxyProxylla. Osoitteeksi asetin `127.0.0.1` ja portiksi `8080` (ZAP:n oletusportti).

<img width="1440" height="478" alt="image" src="https://github.com/user-attachments/assets/6f923db0-9aad-4500-ad06-be21a6608bc1" />

### Kuvien sieppaus
ZAP ei oletuksena käsittele kuvapyyntöjä historiassa.

ZAP-proxystä kuvat näkyviin. 

<img width="746" height="575" alt="image" src="https://github.com/user-attachments/assets/fe31bf6f-09fe-4c9b-99f8-bd222798c970" />

### Todistus:

<img width="1915" height="841" alt="image" src="https://github.com/user-attachments/assets/d5d78a40-b000-483e-93b7-bfa12689edd1" />

<img width="1916" height="871" alt="image" src="https://github.com/user-attachments/assets/f56ce9f8-1981-4008-ac14-c78d7b4acd99" />

Avasin selaimella wikipedia.org:n proxyn ollessa päällä.

#### Lähteet

- Vinkit: [terokarvinen.com](https://terokarvinen.com/tunkeutumistestaus/)
- [OWASP ZAP - Official Documentation](https://www.zaproxy.org/docs/)
- [FoxyProxy Standard - Add-ons for Firefox](https://addons.mozilla.org/en-US/firefox/addon/foxyproxy-standard/)

*Tehty 11.09.2026*

## b) Kettumaista.
*Täydennetty 17.9.2026*

### Asennetaan FoxyProxy Standard

Asensin FoxyProxy Standardin -laajennuksen Firefox Add-ons kaupasta.

<img width="841" height="498" alt="image" src="https://github.com/user-attachments/assets/7485abc0-48ad-4dd3-8e07-d043ba8e4d02" />

### ZAP proxyksi FoxyProxyyn sekä patterns

Käytin samaa ZAP-profiilia kuin a-kohdassa. Lisäsin profiiliin "Proxy by Patterns" -osioon seuraavat include-patternit, jotta vain valitsemani sivustot ohjautuisivat ZAP:n kautta:


<img width="989" height="506" alt="image" src="https://github.com/user-attachments/assets/396cccbc-6505-46b0-819f-54b9a07c089b" />

**Huomio:** Huomasin, että kaikki tuli aluksi läpi ZAP:n työkaluun, ennen kuin huomasin, "proxy by patterns" -kohdan.

<img width="299" height="348" alt="image" src="https://github.com/user-attachments/assets/ffd3b92c-a1e9-4ac8-91e0-a1550613cbd3" />

### Todistus: vain patternit ohjautuvat ZAP:iin


<img width="639" height="179" alt="image" src="https://github.com/user-attachments/assets/4878a6c5-c6fc-4914-8dfe-738448cc2077" />

<img width="937" height="133" alt="image" src="https://github.com/user-attachments/assets/fce2af3a-4208-455c-8abb-79ce8cd24d68" />

<img width="1848" height="539" alt="image" src="https://github.com/user-attachments/assets/0be96945-fd6f-44be-8f64-b622a7417333" />

Tämä osoittaa että wikipedia.org sivusto ei näy ZAP:ssa ja siellä näkyy vain labra alusta *web-security-academy.net*

### Lähteet

- Vinkit: [terokarvinen.com](https://terokarvinen.com/tunkeutumistestaus/)
- [OWASP ZAP - Official Documentation](https://www.zaproxy.org/docs/)
- [FoxyProxy Standard - Add-ons for Firefox](https://addons.mozilla.org/en-US/firefox/addon/foxyproxy-standard/)
- [Portswigger Web Security Academy](https://portswigger.net/web-security)

## Cross Site Scripting (XSS)

### c) Reflected XSS into HTML context with nothing encoded

#### Haavoittuvuuden löytäminen

Sivulla oli hakukenttä "Search the blog...", jonka syöte heijastui takaisin sivulle otsikkoon muodossa "X search results for '[hakusana]'".

<img width="854" height="289" alt="image" src="https://github.com/user-attachments/assets/31db5c73-f1b7-4435-9029-b112b4ce7ae0" />

Testasin ensin sanalla `test` ja tarkistin sivun lähdekoodista (Ctrl + U), että hakusana päätyi suoraan `<h1>` -elementin sisään ilman minkäänlaista HTML-enkoodausta:

<img width="343" height="74" alt="image" src="https://github.com/user-attachments/assets/bd0835fe-3b02-4d4b-9fc6-769fb2972271" />

Koska syöte päätyy suoraan tekstisisältöön sen pystyy korvaamaan suoraan omalla HTML:llä ilman, että tarvitsee ensin katkaista mitään lainausmerkkiä tai attribuuttia.

#### Hyökkäys

Syötin hakukenttään labran ohjeiden mukaan `alert`-funktion 
```bash
<script>alert(1)</script>
```
Selain suoritti scriptin ja avasi alert-ikkunan:

<img width="551" height="190" alt="image" src="https://github.com/user-attachments/assets/acef2656-1989-44db-9e0d-19f3b68a7e9e" />

#### Mitä palvelimella tapahtuu

ZAP:n Historystä nähdään, että pyynnössä hakusana lähetetään URL-enkoodattuna:

`GET /?search=%3Cscript%3Ealert%281%29%3C%2Fscript%3E`

Palvelin purkaa enkoodauksen ja lisää hakusanan suoraan sivun HTML:ään, ilman että se suodattaa tai enkoodaa sitä uudelleen:

`<h1>0 search results for '<script>alert(1)</script>'</h1>`

Kun selain saa tämän vastauksen, se tulkitsee `<script>`-tagin osaksi sivua eikä pelkkänä tekstinä, ja suorittaa sisällä olevan koodin.

<img width="624" height="188" alt="image" src="https://github.com/user-attachments/assets/c83bcdf9-6131-4be3-900b-abd2b503b040" />

<img width="611" height="377" alt="image" src="https://github.com/user-attachments/assets/37d6eee6-b8ed-4bb0-9514-0b11d5792833" />

#### Mistä vika johtuu

Vika johtuu siitä, ettei sovellus enkoodaa käyttäjän syötettä ennen kuin se lisätään HTML-vastaukseen (esim. `<` pitäisi muuttaa muotoon `&lt;`). Koska kyseessä on reflected XSS, haavoittuvuus ei tallennu palvelimelle pysyvästi, vaan se laukeaa vain kun joku itse lähettää pyynnön, jossa payload on mukana, esim. klikkaamalla hyökkääjän lähettämää haitallista linkkiä

<img width="323" height="57" alt="image" src="https://github.com/user-attachments/assets/049fb48a-d361-42c6-9979-de0988db4bf8" />


#### Lähteet:

- [OWASP XSS Preventation Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Cross_Site_Scripting_Prevention_Cheat_Sheet.html)
- [Portswigger Web Security Academy - Reflected XSS](https://portswigger.net/web-security/cross-site-scripting/reflected/lab-html-context-nothing-encoded)


### d) 
