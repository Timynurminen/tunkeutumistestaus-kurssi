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
Nmapin dokumentaatio on luotetta lähde, koska se on Nmap-projektin virallinen dokumentaatio osoitteessa nmap.org ja kuvaa suoraan ohjelman omien komentojen toimintaa.'

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


Hostit:

<img width="749" height="119" alt="image" src="https://github.com/user-attachments/assets/6d91713c-ce18-4790-a2ed-e928737d9139" />

Palvelut:

<img width="910" height="478" alt="image" src="https://github.com/user-attachments/assets/86627335-e912-4d8f-94d5-6fc743bf4d9c" />

Tutkin komentoja ja kokeilin:

<img width="935" height="466" alt="image" src="https://github.com/user-attachments/assets/9e130141-5d59-42f7-a14d-681046c17fb1" />

<img width="928" height="771" alt="image" src="https://github.com/user-attachments/assets/5a0982b5-c857-4ab9-aae1-8d5c0845559a" />

## d) Internet Famous

Etsin netistä erilaisia kuuluisia exploitteja liittyen Metasploittiin

Törmäsin sellaiseen kuin post/linux/gather/hashdump

<img width="932" height="199" alt="image" src="https://github.com/user-attachments/assets/3f5a2eac-275d-44e0-ae56-f3294ce1c57a" />

- Hashien dumppaaminen on yksi tunnetuimmista post-exploitation-tekniikoista
- Ollut esillä monissa tietoturvakonferensseissa
- Tekniikka on klassinen osa Metasploitin työkalupakkia ja mainitaan lähes kaikissa Metasploit-oppaissa
- Ollut julkisuudessa jo vuosia, koska se liittyy käyttäjätunnus ja salasana säilöntä järjestelmiin.

Moduuli toimii Linux-järjestelmissä, joihin on saatu shell tai meterprete-sessio. Sen avulla voidaan kerätä järjestelmän käyttäjien hash-tietoja jatkoanalyyisä varten.

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
