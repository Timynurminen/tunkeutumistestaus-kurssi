# h2 - DORA the Explora

## x) Lue/katso/kuuntele ja tiivistä

### Buuri 2026: DORA and TLPT testing

- **DORA** on EU:n finanssialan digitaalista toimintavarmuutta koskeva sääntely, jota alettiin soveltamaan tammikussa 2025.
- DORA sisältää tavallisen tietoturvatestauksen lisäksi Threat-Led Penetration Testing (TLPT)-testauksen merkittäville finanssialan toimijoille.
- TIBER tarkoittaa *Threat Intelligence-Based Ethical Red Teaming.* TIBER-EU toimii yhteisenä mallina red team -testaukselle ja TIBER-FI on sen suomalainen toteutus.
- Testauksessa keskeisiä osapuolia ovat Control Team, Threat Intelligence Provider, Red Team Testers ja Blue Team. Blue Team ei tiedä testistä etukäteen.
  - Red team: toimii hyökkääjän roolissa ja yrittää murtautua sovittuihin kohteisiin.
  - Blue team: puolustaa organisaation järjestelmiä ja pyrkii havaitsemaan sekä estämään hyökkäykset.
  - Purple team: Red ja Blue team työskentelevät yhdessä ja käyvät läpi hyökkäyksiä sekä puolustuksen toimintaa oppimisen ja tietoturvan parantamiseksi

**Oma huomio:** Oli kiinnostavaa huomata, kuinka tarkasti TLPT-testaukset on organisoitu.

### DORA article 26 - Advanced testing based on TLPT

- Määrättyjen finanssialan toimijoiden täytyy tehdä TLPT-testausta vähintään kolmen vuoden välein.
- Testi kohdistuu kriittisiin tai tärkeisiin toimintoihin ja suoritetaan niitä tukevissa tuotantojärjestelmissä.
- Testauksessa pitää hallita riskejä, jotta esimerkiksi tietoja, järjestelmiä tai kriittisiä palveluita ei vahingoiteta.
- Testin jälkeen havainnot ja korjaussuunnitelmat raportoidaan viranomaiselle.

**Oma huomio:** Mielenkiintoisin asia oli se, että testaus tehdään oikeissa tuotantojärjestelmissä. Tämän takia riskienhallinta on tärkeä osa testausta.

### DORA article 27 - Requirements for testers

- TLPT-testaajien pitää olla luotettavia ja heillä täytyy olla osaamista threat intelligencestä, penetration testingistä ja red team testingistä.
- Testaajien pitää olla sertifioituja tai noudattaa virallista eettistä toimintamallia.
- Testaajien täytyy huolehtia riskien ja luottamuksellisten tietojen asianmukaisesta käsittelystä.
- Sisäisiä testaajia voidaan käyttää tietyillä ehdoilla, mutta Threat Intelligence Providerin täytyy olla ulkopuolinen.

**Oma huomio:** Testaajille asetetut vaatimukset ovat mielestäni ihan ymmärrettäviä, koska testin aikana he voivat päästä käsiksi erittäin kriittisiin järjestelmiin ja tietoihin.

### TIBER-FI 5.4 - Testing phase: Red team testing

- RTT (Red Team Testers): testaajat, jotka toimivat hyökkääjän roolissa ja toteuttavat sovitut hyökkäysskenaariot kohdejärjestelmiä vastaan.
- Red team -vaiheessa RTT suunnittelee ja toteuttaa valittuihin uhkaskenaarioihin perustuvan testin.
- Vaiheessa tehdään ensin **Red Team Test Plan**, jonka hyväksymisen jälkeen alkaa aktiivinen testaus.
- Testauksessa voidaan käyttää vaiheita kuten reconnaissance, weaponisation, delivery, exploitation, control and movement sekä actions on target.
- Jos Red Team ei muuten pääse etenemään, sille voidaan antaa esimerkiksi pääsy sisäverkkoon eli leg-up, jotta testissä voidaan jatkaa seuraavaan tavoitteeseen.

**Oma huomio:** Leg-up oli mielenkiintoinen, mutta tarpeellinen testien suorittamisen kannalta. Olisi harmillista jäädä kiinni heti ensi kättelyssä.

## a) Asenna Metasploitable 2 virtuaalikoneeseen.

Latasin metasploitablen:

```bash
https://sourceforge.net/projects/metasploitable/
```

Käytin latauksen mukana tullutta virtuaalilevyä.

<img width="332" height="66" alt="image" src="https://github.com/user-attachments/assets/ffb2c6f9-fd1d-4b62-a686-d60aa3d98a25" />

## b) Kalin ja Metasploitablen virtuaaliverkko

Tässä tehtävässä loin Kalin ja Metasploitablen välille VirtualBoxin Host-only-verkon.

Kali Linuxille määritin kaksi verkkokorttia:

<img width="710" height="375" alt="image" src="https://github.com/user-attachments/assets/6614c772-96e6-4ff1-b9f6-1a991516bd7c" />

<img width="697" height="383" alt="image" src="https://github.com/user-attachments/assets/17d7c4c2-1b8a-4187-a392-dcc92db35b81" />

Metasploitablelle määritin vain Host-only-verkkokortin. Näin Metasploitable ei ole suoraan yhteydessä Internettiin.

<img width="710" height="325" alt="image" src="https://github.com/user-attachments/assets/cd744ba9-06a4-4701-b57d-df69f68c6127" />


#### IP-osoitteiden tarkistus

Tarkistin Kalin verkkoliitännät komennolla:

```bash
ip a
```

Kali sai NAT-verkkokortille **eth0** osoitteen:

```bash
10.0.2.15/24
```

ja Host-only-verkkokortille **eth1** osoitteen:

```bash
192.168.56.101/24
```


<img width="816" height="373" alt="image" src="https://github.com/user-attachments/assets/f5582f76-78ee-4d86-8908-4150ae617bbe" />

Metasploitable:n verkkoliitännät tarkistin samalla komennolla.

<img width="730" height="220" alt="image" src="https://github.com/user-attachments/assets/a8bc5ace-f3d5-498b-b43a-317e401159f7" />

Metasploitable sai Host-only-verkon osoitteeksi:

```bash
192.168.56.102/24
```

Kali ja Metasploitable ovat siis samassa **192.168.56.0/24** Host-only-verkossa.

#### Yhteyden testaus

Kalin ja Metasploitablen välisen yhteyden testasin Kalista komennolla:

```bash
ping -c 4 192.168.56.102
```

<img width="520" height="196" alt="image" src="https://github.com/user-attachments/assets/2d546e04-35b7-4b3b-95cd-47c8e31e03bd" />

## c) Virtuaaliverkko

Kalista otin internetin pois komennolla:

```bash
sudo nmcli device disconnect eth0
```

Jonka jälkeen:

<img width="324" height="73" alt="image" src="https://github.com/user-attachments/assets/3200eeb2-df09-411e-b51b-a82a530e25ac" />

Metasploitable taas on otettu pois internetistä VirtualBox:n avulla jo valmiiksi.

<img width="412" height="34" alt="image" src="https://github.com/user-attachments/assets/f984017d-0118-44bd-b617-2af4154f467a" />


Yhteys testattu Kalilta Metasploitableen:

<img width="530" height="191" alt="image" src="https://github.com/user-attachments/assets/0211a665-6e6e-4118-9539-a71053a99409" />

Sekä toisinpäin:

<img width="576" height="178" alt="image" src="https://github.com/user-attachments/assets/7569a69a-448b-492f-8139-51348a7d3f28" />

## d) Etsi Metasploitable

Etsin Metasploitablen:

<img width="924" height="285" alt="image" src="https://github.com/user-attachments/assets/394d62e8-527c-4d55-8474-f214ac55fc0e" />

Jonka jälkeen tarkistin weppipalvelimen.

<img width="611" height="494" alt="image" src="https://github.com/user-attachments/assets/a5af8ade-d466-4047-8df7-e0f3d1e78335" />

## e) Skannaa Metasploitable

Suoritin skannauksen komennolla:

```bash
sudo nmap -A -T4 -p- 192.168.56.102
```

#### Kolme mielenkiintoista porttia:

##### FTP - portti 21

<img width="448" height="227" alt="image" src="https://github.com/user-attachments/assets/c25778d1-8f83-469d-84de-4d7b9fcefd66" />

- Portissa 21 oli avoinna FTP-palvelu, jonka Nmap tunnisti vsftpd 2.3.4 -palvelimeksi.
- Skripti havaitsi palvelimen sallivan anonyymin FTP-kirjautumisen.
- Palveluun pääsyä ei välttämättä ole rajattu tavalliseen käyttäjätunnukseen ja liikenteen sisältöä ei suojata salauksella

##### Samba - portit 139 ja 445

<img width="610" height="39" alt="image" src="https://github.com/user-attachments/assets/f30ab7e5-a6a2-457e-89a6-7f152a3830a0" />

<img width="796" height="272" alt="image" src="https://github.com/user-attachments/assets/e66caf98-16ef-4f92-a46e-2d566ac305c1" />

Nmap löysi Samba-palvelun TCP-porteista 139 ja 445. Nmap sai SMB-palvelusta myös lisätietoja, kuten tietokoneen nimen *metasploitable*, domainin *localdomain* ja FQDN-nimen *metasploitable.localdomain*

*smb-security-mode*-tarkistus ilmoitti, että *message_signing* oli poistettu käytöstä. Nmapin dokumentaation mukaan SMB message signingilla voidaan varmistaa viestien eheyttä, ja jos allekirjoitusta ei vaadita, yhteys voi olla altis man-in-the-middle-hyökkäyksille.

Samba-palvelusta saatiin siis pelkän avoimen portin lisäksi tietoa järjestelmän nimestä, toimialueesta sekä SMB-palvelun turvallisuusasetuksista.

##### MySQL - portti 3306

<img width="939" height="164" alt="image" src="https://github.com/user-attachments/assets/06d72866-6a82-449b-b182-28f60d7dbb2e" />

- Portissa 3306 oli avoinna MySQL-tietokantapalvelu.
- Nmap suoritti palvelulle myös *mysql-info*-tarkistuksen, joka palautti esimerkiksi käytetyn protokollaversion, palvelimen version, tuettuja ominaisuuksia sekä palvelimen tilan.
- Tuloksesta voidaan päätellä, että MySQL-palvelu hyväksyy verkkoyhteyksiä Metasploitable-koneella.
- Lisäksi palvelusta voidaan saada tietoa jo ennen varsinaista autentikointia.


## Lähteet

- [Buuri 2026: DORA and TLPT testing](https://terokarvinen.com/buuri-2026-dora-and-threat-lead-penetration-testing/buuri-2026-dora-and-threat-lead-penetration-testing--teros-pentest-course.pdf)
- [Eur-Lex 2022: DORA: Articles 26 and 27](https://eur-lex.europa.eu/eli/reg/2022/2554/oj/eng)
- [Suomen pankki 2025: TIBER-FI procedures and Guidelines](https://www.suomenpankki.fi/globalassets/bof/en/money-and-payments/the-bank-of-finland-as-catalyst-payments-council/tiber-fi/tiber-fi-2.0-procedures-and-guidelines.pdf)
- [Nmap: Reference Guide](https://nmap.org/book/man.html)
- [Nmap: Port Scanning Basics](https://nmap.org/book/man-port-scanning-basics.html)
- [Nmap: Service and Version Detection](https://nmap.org/book/man-version-detection.html)
- [Nmap: OS detection](https://nmap.org/book/man-os-detection.html)
- [Nmap: ftp-anon](https://nmap.org/nsedoc/scripts/ftp-anon.html)
- [Nmap: smb-security-mode](https://nmap.org/nsedoc/scripts/smb-security-mode.html)
- [Nmap: mysql-info](https://nmap.org/nsedoc/scripts/mysql-info.html)
