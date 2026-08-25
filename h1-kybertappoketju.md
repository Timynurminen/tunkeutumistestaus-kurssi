# h1 Kybertappoketju

## Lue/katso/kuuntele ja tiivistä

### Herrasmieshakkerit: Valokuvien vainukoira, vieraana Teemu M. Nieminen | 0x41

- Teemu Nieminen on avointen lähteiden tutkija, hän on väitöskirjatutkija Helsingin yliopistossa ja hänen aiheenaan on ulkoparlamentaarisen äärioikeiston rahoitus 2000-luvulla Euroopassa.
- Bellingcat on avointen lähteiden tutkijoiden kollektiivi. Heidän perusperiaatteensa on olla läpinäkyviä ja he yrittävät levittää osaamistaan sekä avointen lähteiden tutkimisen menetelmiä ja tekniikoita kaikkien saataville.
- Teemu tekee myös geopaikantamista ja on onnistunut selvittämään esimerkiksi ääriliikkeiden salaisia kokoontumispaikkoja.
- Käänteinen kuvahaku tarkoittaa sitä, että hakukoneet yrittävät löytää samanlaisia kuvia tai jopa samoja kuvia eri nettisivuilta, ja on todella kätevä työkalu geopaikantamista varten.
- Jos haluat estää geopaikantamista itsestäsi Teemun ensimmäinen neuvo on olla ottamatta kuvaa ollenkaan. Jos kuva on pakko ottaa, kannattaa kuvassa välttää tunnistettavia taustoja ja heijastuksia, joiden avulla kuva voitaisiin geopaikantaa.
- Teemu kuuli Bellingcatin tutkimusvetäjältä, että jos haluaa tehdä geopaikantamista siten, että joku jopa maksaa siitä, täytyy sinun löytää joku markkinarako tietystä aiheesta.
- Teemu tämän jälkeen alkoi tutkimaan suomalaisia uusnatseja sekä suomalaisia äärioikeistoja, joten hän liittyi vitsinä uusnatsien Telegram-ryhmiin tutkimaan mitä he tekevät. Tämän jälkeen hän geopaikansi yhden heidän kuvistaan ja kertoi siitä Bellingcatin tutkijoille ja tästä tapauksesta tehtiin artikkeli Bellingcatin sivuille.
- Lopuksi Teemu alkoi kirjoittamaan väitöskirjaa aiheesta johon oli yhtäkkiä upottanut paljon aikaa.

**Oma huomio:** Oli todella mielenkiintoista kuunnella geopaikantamisesta ja yllätyin kuinka paljon tietoa voidaan saada kuvan tietyistä asioista kuten heijastuksista tai auringon ja varjojen perusteella. Teemun lyhyt elämänkerta oli myös mielenkiintoinen, aihe vaikuttaa vielä hieman vaaralliselta jossa täytyy itsekin olla aika anonyymi jos haluaa äärioikeistoja tutkia.

### Hutchins et al. 2011: Abstract

- Perinteiset tietoturvatyökalut kuten virustorjunta ja tunkeutumisen havaitsemisjärjestelmät eivät yksin riitä kehittyneitä hyökkäyksiä vastaan
- "Advanced Persistent Threat" (APT) -hyökkääjät ovat hyvin varusteltuja ja voivat toteuttaa hyökkäyskampanjoita pitkään
- Hyökkääjät käyttävät kehittyneitä työkaluja ja tekniikoita päästäkseen perinteisten suojausten ohi
- Kill Chain -mallilla hyökkäys voidaan jakaa eri vaiheisiin ja suunnitella puolustusta niiden perusteella

#### 3.2 Intrusion Kill Chain

- Intrusion Kill Chain kuvaa hyökkäyksen eri vaiheita
- Ketjun ideana on, että hyökkääjän täytyy onnistua vaiheissa päästäkseen tavoitteeseensa
- Intrusion Kill Chain on seitsemänkohtainen.
  - **Reconnaissance** - kohteen tutkiminen ja tiedon kerääminen
  - **Weaponization** - hyökkäyksessä käytettävän haitallisen sisällön valmistelu
  - **Delivery** - haitallisen sisällön toimittaminen kohteelle esimerkiksi sähköpostilla, verkkosivulla tai USB-laitteella
  - **Exploitation** - haavoittuvuuden tai käyttäjän hyödyntäminen
  - **Installation** - haittaohjelman tai takaoven asentaminen kohdekoneelle
  - **Command and Control** - yhteyden muodostaminen hyökkääjän ja murretun koneen välille
  - **Actions on Objectives** - hyökkääjä toteuttaa varsinaisen tavoitteensa, esimerkiksi tietojen varastamisen

**Oma huomio:** Kill Chain auttaa hahmottamaan hyökkäyksen kokonaisuutena. Hyökkäys ei tapahdu vain yhdellä kertaa, vaan hyökkääjän täytyy päästä usean eri vaiheen läpi.

### Santos et al: Active Reconnaissance

- Passive Reconnaissance -vaiheessa kohteeseen ei lähetetä suoraan verkkoliikennettä
- Active Reconnaissance -vaiheessa aloitetaan esimerkiksi porttiskannaukset ja varmistetaan passiivisessa vaiheessa löytyneitä tietoja
- Aktiivinen tiedustelu voidaan havaita kohdejärjestelmässä
- **Port scanning** - selvitetään mitä portteja kohdekoneella on avoinna ja mitä palveluita niiden takana mahdollisesti toimii
- **Web service review** - tutkitaan kohteen web-palveluita ja selvitetään esimerkiksi mitä teknologioita tai palveluita sivusto käyttää
- **Vulnerability scanning** - etsitään kohdejärjestelmästä tunnettuja haavoittuvuuksia ja mahdollisia tietoturvaongelmia
- **Nmap** - monipuolinen ja vakaa porttiskanneri
- **Masscan** - erittäin nopea porttiskanneri, mutta ei yhtä monipuolinen kuin Nmap
- **UDPPortScanner** - nopea työkalu UDP-porttien skannaamiseen
- **EyeWitness** - auttaa käymään läpi web-palveluita ja priorisoimaan kiinnostavimmat kohteet
- **OpenVAS** - ilmainen ja avoimen lähdekoodin haavoittuvuusskanneri
- **Nessus** - maksullinen haavoittuvuusskanneri
- **Nexpose** - maksullinen haavoittuvuuksien hallinta- ja skannaustyökalu
- **Qualys** - maksullinen pilvipohjainen haavoittuvuusskanneri
- **Nmap** - pystyy myös rajalliseen haavoittuvuuksien tarkistamiseen
- **Nikto** - skannaa web-palvelimista yleisiä haavoittuvuuksia ja virheellisiä asetuksia
- **WPScan** - tarkoitettu WordPress-sivustojen haavoittuvuuksien tutkimiseen
- **SQLMap** - etsii ja testaa SQL-injektiohaavoittuvuuksia
- **Burp Suite** - monipuolinen työkalu web-sovellusten tietoturvatestaukseen
- **Zed Attack Proxy** - avoimen lähdekoodin työkalu web-sovellusten haavoittuvuuksien etsimiseen

**Oma huomio:** Nmap vaikuttaa selvästi näistä hyödyllisimmältä työkalulta, koska sillä pystyy tekemään paljon muutakin kuin pelkän porttiskannauksen.

### KKO 2003:36

- Tapauksessa henkilö suoritti porttiskannauksen Osuuspankkikeskuksen tietojärjestelmään
- Tarkoituksena oli löytää verkosta avoimia välityspalvelimia
- Porttiskannaus ei päässyt palomuurin läpi
- Korkein oikeus katsoi toiminnan tietomurron yritykseksi
- Porttiskannaus voi siis olla laitonta, jos sillä yritetään löytää keino päästä oikeudettomasti toisen järjestelmään

**Oma huomio:** Tapaus oli hyvä muistutus siitä, että porttiskannausta ei kannata tehdä mihinkään ulkopuoliseen järjestelmään ilman lupaa. 

## a) Kali virtuaalikone

Asensin Kali Linuxin Oracle Virtualboxiin käyttäen Kalin valmista Virtualbox-virtuaalikonetta

#### Versio

```bash
cat /etc/os-release
```

<img width="329" height="264" alt="image" src="https://github.com/user-attachments/assets/f0f6f52d-1069-4f1b-a82b-5716c6bfae61" />

## b) Kalin irrottaminen verkosta

Irrotin Kali-virtuaalikoneen verkkoyhteyden Virtualboxin asetuksista.

#### Testaus

```bash
ping -c 4 8.8.8.8
```

<img width="336" height="118" alt="image" src="https://github.com/user-attachments/assets/818a4ba8-0469-45a9-85da-c092dd9108cb" />

## c) Porttiskannaus

Käytin komentoa:

```bash
nmap -T4 -A localhost
```

#### Komennon parametrit

- nmap on verkkojen ja palveluiden skannaukseen tarkoitettu työkalu
- "-T4" määrittää skannauksen ajoituksen nopeaksi
- "-A" ottaa käyttöön useita tarkempia tunnistustoimintoja, kuten käyttöjärjestelmän tunnistuksen, palveluiden ja niiden versioiden tunnistuksen
- localhost tarkoittaa omaa tietokonetta.

#### Tulosten analysointi

<img width="627" height="223" alt="image" src="https://github.com/user-attachments/assets/5b499802-fc75-4d29-b773-28dfefed9356" />


Tuloksessa näkyi:

```bash
Host is up
```

Tämä tarkoittaa, että localhost vastasi skannaukseen normaalisti.

Nmap skannasi 1000 yleisintä TCP-porttia ja ilmoitti:

```bash
All 1000 scanned ports on localhost (127.0.0.1) are in ignored states.
Not shown: 1000 close tcp ports (reset)
```

Tämä tarkoittaa, että kaikki 1000 skannattua TCP-porttia olivat suljettuja.

Nmap ilmoitti myös:

```bash
Too many fingerprints match this host to give specific OS details.
```

Tämä tarkoittaa, että Nmap ei pystynyt tunnistamaan käyttöjärjestelmää tarkasti.

Tuloksessa näkyi myös: 

```bash
Network Distance: 0 hops
```
Verkkoetäisyys oli 0 hyppyä, koska skannattava kohde oli sama tietokone.

Lisäksi vielä Nmap ilmoitti:

```bash
Other addresses for localhost (not scanned) ::1
```

::1 on localhostin IPv6-osoite. Tässä skannauksessa käytettiin IPv4-osoitetta 127.0.0.1, joten IPv6-osoitetta ei skannattu.

#### Yhteenveto

Skannaus onnistui ja localhost vastasi normaalisti. Kaikki TCP-portit olivat kuitenkin suljettuja.


## d) Kahden demonin asennus ja uusi Nmap-skannaus

Asensin Kaliin kaksi demonia:

- **Openssh-server**
- **Apache2**

Asennuksen jälkeen suoritin saman Nmap-skannauksen uudelleen:

```bash
nmap -T4 -A localhost
```

#### Skannauksen tulos

<img width="652" height="588" alt="image" src="https://github.com/user-attachments/assets/22213b01-5072-45f5-9147-98efbc54cacc" />


Uudessa skannauksessa Nmap löysi kaksi avointa TCP-porttia

```bash
22/tcp open ssh
80/tcp open http
```

Tuloksessa näkyi lisäksi:

```bash
Not shown: 998 closed tcp ports (reset)
```

Eli 998 porttia oli suljettuja vielä.

#### Vertailu aikaisempaan tulokseen

Ennen demojen asennusta kaikki 1000 porttia oli suljettuna.
Demonien asennusten jälkeen suljettu portti määrä muuttui 998.

#### Yhteenveto

Uusi skannaus osoitti selvästi demonien asentamisen vaikutuksen avoimiin portteihin.
Demonien asentamisen jälkeen portit 22 ja 80 olivat avoinna. Nmap kykeni myös tunnistamaan avoimissa porteissa toimivat palvelut ja niiden versiot.

## e) HackTheBox

<img width="1036" height="203" alt="image" src="https://github.com/user-attachments/assets/1cad7c1a-f4c3-49d0-be18-7f9c650fd660" />
