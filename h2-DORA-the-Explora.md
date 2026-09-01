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
- Jos Red Team ei muuten pääse etenemään, sillve voidaan antaa esimerkiksi pääsy sisäverkkoon eli leg-up, jotta testissä voidaan jatkaa seuraavaan tavoitteeseen.

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
