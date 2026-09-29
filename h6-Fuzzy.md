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

**Lähteet:** 
- [Hoikkala 2023: ffuf README.md](https://github.com/ffuf/ffuf/blob/master/README.md)

## a) Fuzzzz. Ratkaise dirfuz-1

*Seurasin Karvisen artikkelin ohjeita*

### Työkalun ja sanakirjan asennus

Asensin ffufin ja latasin SecListsin `common.txt`-sanakirjan:

```bash
sudo apt-get update
sudo apt-get install ffuf
wget https://raw.githubusercontent.com/danielmiessler/SecLists/master/Discovery/Web-Content/common.txt
```

<img width="947" height="692" alt="image" src="https://github.com/user-attachments/assets/4da0f2f2-a885-40c0-9779-93cd54a8b64d" />

### Kohteen lataus ja käynnistys

Latasin harjoituskohteen dirfuzt-1, annoin sille suoritusoikeuden ja käynnistin sen: 

```bash
wget https://terokarvinen.com/2023/fuzz-urls-find-hidden-directories/dirfuzt-1
chmod u+x dirfuzt-1
./dirfuzt-1
```

Palvelin käynnistyi osoitteeseen `http://127.0.0.2:8000`. Jätin sen pyörimään omaan terminaaliin ja ajoin seuraavat komennot toisessa ikkunassa.


<img width="933" height="364" alt="image" src="https://github.com/user-attachments/assets/e4cb4260-685b-4f03-96bd-44b537df8190" />


### Perusfuzzaus ilman suodattimia

Ajoin ffufin `common.txt`-sanakirjalla kohdetta vastaan ilman suodattimia:

```bash
ffuf -w common.txt -u http://127.0.0.2:8000/FUZZ
```

Ajo kävi läpi kaikki 4752 sanaa ilman virheitä. Sanakirjassa oli 4752 riviä. Lähes kaikki polut palauttivat statuksen 200, joten status-koodin perusteella oikeita osumia ei pystynyt erottamaan.

Tulosteesta huomasin, että lähes jokaisella rivillä toistui sama koko: `Size: 154`, `Words: 9`, `Lines: 10`. Palvelin siis vastaa samalla tavalla olemattomiin polkuihin, ja nämä ovat vääriä osumia.

<img width="927" height="283" alt="image" src="https://github.com/user-attachments/assets/588005cd-2612-4718-bb46-35508f18ae54" />


### Väärien osumien suodatus

Suodatin pois vääriä osumia tuottaneen koon (154 tavua) `-fs`-lipulla:

```bash
ffuf -w common.txt -u http://127.0.0.2:8000/FUZZ -fs 154
```

Jäljelle jäi seitsemän osumaa: 

<img width="734" height="534" alt="image" src="https://github.com/user-attachments/assets/a65911f5-6669-4b8d-99fa-e76b565f60fd" />

Selkeät ehdokkaat: `wp-admin` ja `.git` (admin-sivu sekä versionhallintaan liittyvä sivu)

### Löydösten vahvistus selaimella

Ffuf kertoo vain vastauksen tunnusluvut, joten avasin löydökset selaimessa.

**Admin-sivu:** `http://127.0.0.2:8000/wp-admin`

<img width="561" height="259" alt="image" src="https://github.com/user-attachments/assets/3b31886c-05e7-46d2-8e94-9c2c768d0544" />

**Versionhallintasivu**: `http://127.0.0.2:8000/.git/`

<img width="517" height="262" alt="image" src="https://github.com/user-attachments/assets/3e36ebd6-2ae2-4763-a81d-59681e8a5a55" />


**Lähteet:** 

- [Karvinen 2023: Find Hidden Web Directories - Fuzz URLs with ffuf](https://terokarvinen.com/2023/fuzz-urls-find-hidden-directories/)
- [Hoikkala 2023: ffuf README.md](https://github.com/ffuf/ffuf/blob/master/README.md)

## b) Fuff me. 

*Seurasin Karvisen artikkelin ohjeita (Fuffme - Install Web Fuzzing Target on Debian. ks. lähteet).*

### Asennus

Asensin tarvittavat paketit: 

```bash
sudo apt-get install docker.io git
```

<img width="327" height="58" alt="image" src="https://github.com/user-attachments/assets/b3c329ad-e700-4843-a58e-b63be4ac6293" />

Kloonasin seuraavaksi FuffMe-repositorion, rakensin Docker-imagen ja käynnistin kontin:

```bash
git clone https://github.com/adamtlangley/ffufme
cd ffufme/
sudo docker build -t ffufme .
sudo docker run -d -p 80:80 ffufme
```

<img width="936" height="724" alt="image" src="https://github.com/user-attachments/assets/5ee5609c-0be7-40e7-8ee2-d85f03e0c93b" />

Testasin että kohde vastaa: `curl -si localhost | grep title`

<img width="317" height="71" alt="image" src="https://github.com/user-attachments/assets/ce2c08ca-a389-4259-b68f-64d2e8c26a2e" />


### Sanakirjojen asennus

Latasin FuffMen omat sanakirjat:

```bash
mkdir ~/wordlists
cd ~/wordlists
wget http://ffuf.me/wordlist/common.txt
wget http://ffuf.me/wordlist/parameters.txt
wget http://ffuf.me/wordlist/subdomains.txt
```

<img width="923" height="668" alt="image" src="https://github.com/user-attachments/assets/0ef286e8-c635-4958-88a5-55a1100a3e92" />

### Testifuzzaus

Varmistin ympäristön toimivuuden ajamalla artikkelin esimerkkiharjoituksen:

`ffuf -w ~/wordlists/common.txt -u http://localhost/cd/basic/FUZZ`

Löysin samat kaksi osumaa kuin artikkelissa luvattiin: `class` ja `development.log`. Ympäristö on siis valmis seuraavia tehtäviä varten.

<img width="756" height="447" alt="image" src="https://github.com/user-attachments/assets/b699fad7-103f-4f64-9a3e-2a4ea520169a" />



**Lähteet:**

- [Karvinen 2023: Fuffme - Install Web Fuzzing Target on Debian](https://terokarvinen.com/2023/fuffme-web-fuzzing-target-debian/)

## c) Basic Content Discovery

<img width="814" height="236" alt="image" src="https://github.com/user-attachments/assets/ed483a09-938f-48b6-8c7c-87636bf952ba" />


Ajoin FuffMen ensimmäisen harjoituksen:

`ffuf -w ~/wordlists/common.txt -u http://localhost/cd/basic/FUZZ`

Sama komento tuli jo ajettua b)-kohdan testifuzzauksessa. Tulos oli sama: löysin tiedostot `class` ja `development.log`, kuten tehtävänanto lupasi.

<img width="766" height="432" alt="image" src="https://github.com/user-attachments/assets/b4a12f25-a4d0-4521-bc66-1c88b9ba238b" />

## d) Content Discovery With Recursion

<img width="827" height="247" alt="image" src="https://github.com/user-attachments/assets/98040cc1-fcac-45b9-849f-7b0d976c9052" />


Ajoin FuffMen rekursiivisen harjoituksen:

`ffuf -w ~/wordlists/common.txt -recursion -u http://localhost/cd/recursion/FUZZ`

`-recursion`-lippu sai ffufin jatkamaan automaattisesti löydettyihin alihakemistoihin. Ffuf löysi ensin hakemiston `/admin`, käynnisti sille uuden fuzzausjonon, löysi sieltä  `/admin/users`, käynnisti senkin sisään uuden jonon, ja löysi lopulta tiedoston `/admin/users/96`. Kaikki kolme askelta näkyvät tulosteessa. Tulos täsmää tehtävänannon kanssa.

<img width="757" height="585" alt="image" src="https://github.com/user-attachments/assets/c4adb53f-bd0e-45be-82a4-74e9c941e60d" />

## e) Content Discovery With File Extensions

<img width="814" height="304" alt="image" src="https://github.com/user-attachments/assets/332e3e77-3163-4ab9-a414-7c5768e545a1" />


Ajoin FuffMen harjoituksen, jossa hakemiston `/logs` sisällä olevien tiedostojen oletetaan olevan `.log`-päätteisiä:

`ffuf -w ~/wordlists/common.txt -e .log -u http://localhost/cd/ext/logs/FUZZ`

`-e`-lippu lisää määritetyn päätteen jokaisen sanakirjan sanan perään. Ffuf kokeili yhteensä 9372 sanaa ja löysi tiedoston `users.log`, kuten tehtävänannossa luvattiin.

<img width="743" height="426" alt="image" src="https://github.com/user-attachments/assets/125ddf81-4834-4585-b1f3-a9e048be4fb0" />


## f) No 404 Status

<img width="807" height="458" alt="image" src="https://github.com/user-attachments/assets/7d8daf2a-1400-4ac8-9fcc-0884aa5a0adc" />

Ajoin ensin FuffMen harjoituksen ilman suodattimia: 

`ffuf -w ~/wordlists/common.txt -u http://localhost/cd/no404/FUZZ`

Lähes jokainen pyyntö palautti statuksen 200 ja saman koon, 669 tavua. Palvelin ei siis anna oikeaa 404-virhettä olemattomille sivuille, vaan näyttää "Page Cannot Be Found" -sivun statuksella 200. Tämä tekisi tuloksista harhaanjohtavia, jos luottaisi pelkkään status-koodiin.

<img width="755" height="680" alt="image" src="https://github.com/user-attachments/assets/0abce478-06e2-4836-8e88-58e39755df49" />

Suodatin pois nämä vääränlaiset osumat niiden yhteisen koon perusteella:

`ffuf -w ~/wordlists/common.txt -u http://localhost/cd/no404/FUZZ -fs 669`

Jäljelle jäi yksi oikea osuma: tiedosto `secret`, kuten tehtävänannossa luvattiin.

<img width="749" height="435" alt="image" src="https://github.com/user-attachments/assets/44887314-2f1a-4691-9a63-c7bad5f0e519" />


## g) Param Mining

<img width="828" height="283" alt="image" src="https://github.com/user-attachments/assets/14c090e8-234c-47d0-a525-120d8b2d557b" />

Ajoin FuffMen harjoituksen, jossa fuzzattiin puuttuvaa URL-parametria polun sijaan:

`ffuf -w ~/wordlists/parameters.txt -u http://localhost/cd/param/data?FUZZ=1`

Sivu `/cd/param/data` palautti ilman parametria statuksen 400 ("Required Parameter Missing"). Ffuf löysi puuttuvan parametrin nimen: `debug`, kuten tehtävänannossa luvattiin.

<img width="748" height="410" alt="image" src="https://github.com/user-attachments/assets/722aaebd-83bc-4ee9-8f60-2a0d24829563" />

## h) Rate Limited

<img width="806" height="459" alt="image" src="https://github.com/user-attachments/assets/b423564c-0a7b-489c-b5c1-b52dae704f8d" />

Ajoin ensin perusfuzzauksen rajoitettua hakemistoa vastaan:

`ffuf -w ~/wordlists/common.txt -u http://localhost/cd/rate/FUZZ -mc 200,429`

Hakemisto on rajoitettu 50 pyyntöön sekunnissa. Koska Ffuf lähetti pyyntöjä nopeammin, lähes kaikki pyynnöt palauttivat statuksen 429 (liikaa pyyntöjä, tilapäisesti estetty).

<img width="764" height="734" alt="image" src="https://github.com/user-attachments/assets/7c42d65f-556a-41b2-9ef2-3487862a6465" />

Hidastin sitten ajoa säikeiden määrää ja viivettä säätämällä:

`ffuf -w ~/wordlists/common.txt -t 5 -p 0.1 -u http://localhost/cd/rate/FUZZ -mc 200,429`

`-t 5` rajoitti samanaikaisten säikeiden määrän viiteen, `-p 0.1` lisäsi 0,1 sekunnin viiveen jokaisen pyynnön jälkeen. Ffuf laski nopeudeksi 47 pyyntöä sekunnissa, mikä pysyi rajoituksen alla. 429-virheitä ei enää tullut, ja löysin tiedoston `oracle`, kuten tehtävänannossa luvattiin.

<img width="744" height="436" alt="image" src="https://github.com/user-attachments/assets/dffc4efc-6baa-4870-b3d6-bd79bda4d59f" />

## i) Subdomains - Virtual Host Enumeration

<img width="807" height="422" alt="image" src="https://github.com/user-attachments/assets/99d44b67-2394-44cc-9ecd-e774a3771f20" />

Ajoin ensimmäisen FuffMen virtuaalihosti-harjoituksen fuzzaten `Host`-otsaketta polun sijaan:

`ffuf -w ~/wordlists/subdomains.txt -H "Host: FUZZ.ffuf.me" -u http://localhost`

Kaikki 1907 tulosta palauttivat saman koon, 1495 tavua, kuten tehtävänannossa sanottiin. Tämä tarkoittaa, että palvelin vastaa samalla tavalla riippumatta siitä, mikä aliverkkotunnus Host-otsakkeessa on. Pelkkä status-koodi ei siis erottele oikeita osumia.

<img width="757" height="761" alt="image" src="https://github.com/user-attachments/assets/8025867c-aa74-4efa-a211-664bdb76485c" />

Suodatin pois vääränlaiset osumat niiden yhteisen koon perusteella:

`ffuf -w ~/wordlists/subdomains.txt -H "Host: FUZZ.ffuf.me" -u http://localhost -fs 1495`

Jäljelle jäi yksi osuma `redhat`, kuten tehtävänannossa luvattiin.

<img width="754" height="452" alt="image" src="https://github.com/user-attachments/assets/77207019-fe80-424b-9046-194bcf28aba3" />

## Lähteet

- [Karvinen 2023: Find Hidden Web Directories - Fuzz URLs with ffuf](https://terokarvinen.com/2023/fuzz-urls-find-hidden-directories/)
- [Hoikkala 2023: ffuf README.md](https://github.com/ffuf/ffuf/blob/master/README.md)
- [Karvinen 2023: Fuffme - Install Web Fuzzing Target on Debian](https://terokarvinen.com/2023/fuffme-web-fuzzing-target-debian/)
- [Langley: FFUF Me - Target Practice For FFUF (Github-repositorio)](https://github.com/BuildHackSecure/ffufme)
