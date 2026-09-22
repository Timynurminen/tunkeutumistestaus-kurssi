# h5 Elokuu2026!

## x) Lue/Kkatso ja tiivistä

### Karvinen 2022: Cracking Passwords with Hashcat

- Järjestelmät eivät tallenna salasanoja sellaisenaan, vaan niiden tiivisteitä (hash). Tiivistys on yksisuuntainen funktio, joten sitä ei voi purkaa suoraan takaisin salasanaksi.
- Hashcat kokeilee sanakirjan jokaista sanaa ja vertaa sen tiivistettä annettuun hashiin, kunnes löytyy täsmäys.
- Ennen murtamista pitää tunnistaa hashin tyyppi (esim. MD5, SHA1) `hashid`-työkalulla, koska hashcat tarvitsee tämän `-m` -parametrina.
- Yleisimpiä sanakirjoja on RockYou, joka sisältää yli 14 miljoonaa oikeasti vuodettua salasanaa.
- Komento `hashcat -m 0 '<hash>' rockyou.txt -o solved` tallentaa löytyneen salasanan `solved`-tiedostoon. Jos kaikki sanat on kokeiltu eikä osumaa löydy, tila on "Exhausted" kun taas onnistuneen osuman jälkeen se on "Cracked".
- Hashcat on huomattavasti nopeampi ajettuna oikealla laitteistolla (host-käyttöjärjestelmässä GPU:n kanssa) kuin virtuaalikoneessa, koska se hyödyntää näytönohjaimen laskentatehoa.

**Oma huomio:** On mielenkiintoista kuinka nopeasti voidaan murtaa yksinkertainen salasana. Korostaa suuresti pitkien ja satunnaisten salasanojen tehokkuutta.


### Karvinen 2023: Crack File Password With John

- John the Ripper pystyy murtamaan monien tiedostomuotojen (esim. ZIP, Office, PDF) salasanoja sanakirjahyökkäyksellä.
- Jakelupaketeissa oleva John ei tue kaikkia formaatteja, joten artikkelissa käännetään lähdekoodista uusin "Jumbo" -versio, joka tukee huomattavasti enemmän tiedostotyyppejä.
- Salasanan murtaminen on kaksivaiheinen: ensin erotetaan tiedoston hash omaksi tiedostoksi työkalulla kuten `zip2john`, sitten ajetaan `john`-komento tätä hash-tiedostoa vastaan.
- Jumbo-versiossa on erillinen "2john"-skripti lähes jokaiselle tuetulle formaatille, joka osaa purkaa juuri kyseisen tiedostomuodon salauksesta hashin.
- John käy oletuksena ensin läpi yksinkertaisia sääntöjä, sitten oman sanakirjatiedostonsa (`password.lst`), ja tulostaa löytyneen salasanan suoraan komentorivin, minkä lisäksi `--show`-parametrilla voi näyttää kaikki jo murretut salasanat uudelleen.
- Sanakirjahyökkäys ei toimi hyvin generoituja, satunnaisia salasanoja vastaan (esim. `pwgen`-työkalulla luotuja). Tämä korostaa hyvien salasanojen ja salasananhallintaohjelmien käytön tärkeyttä.

**Oma huomio:** Mielenkiintoista on se, että lähes minkä tahansa tunnetun tiedostomuodon salaus voidaan murtaa samalla periaattella (poimi hash -> aja sanakirjahyökkäys) kunhan oikea "2john"-skripti löytyy.

## a) Asenna Hashcat ja testaa sen toiminta murtamalla esimerkkisalasana
*Seurasin Karvisen artikkelin ohjeita (ks. x-kohdan tiivistelmä).*
### Hashcatin asennus

```bash
sudo apt-get update
sudo apt-get -y install hashid hashcat wget
```

<img width="762" height="704" alt="image" src="https://github.com/user-attachments/assets/bceb28dc-8da1-416e-95d0-1a9a5544ff05" />

### Sanakirjan lataus

Latasin RockYou-sanakirjan, joka sisältää yli 14 miljoonaa oikeasti vuodettua salasanaa.

```bash
wget https://github.com/danielmiessler/SecLists/raw/master/Passwords/Leaked-Databases/rockyou.txt.tar.gz
```

<img width="227" height="61" alt="image" src="https://github.com/user-attachments/assets/b903b249-7aca-4171-a182-99e591237977" />

### Hash-tyypin tunnistaminen

Tein oman hashin sanasta: "kissa123"
`echo -n 'kissa123' | md5sum`

<img width="302" height="74" alt="image" src="https://github.com/user-attachments/assets/f9790d97-3839-4839-b6bd-6acaa6c5c2ef" />

Tarkistin, että se löytyy rockyou.txt listasta:

<img width="306" height="77" alt="image" src="https://github.com/user-attachments/assets/c9e134b3-5b5d-4367-b3e1-db44006fc50b" />

Tunnistin hash-tyypin:

<img width="432" height="345" alt="image" src="https://github.com/user-attachments/assets/fddb82ad-bb92-41bd-8f98-1a9ae6ac26e7" />

### Murtaminen

Ajoin hashcatin sanakirjahyökkäyksenä:

```bash
hashcat -m 0 '13c3a117d0013ab22417c8edca354b76' rockyou.txt -o solved
```

**Huom:** Ensimmäinen ajo epäonnistui virheeseen "No OpenCL, HIP or CUDA compatible platform found", koska virtuaalikoneella ei ollut käytettävissä näytönohjainta. Jouduin asentamaan CPU:lle OpenCL-tuen, jotta hashcat toimisi.

```bash
sudo apt-get -y install pocl-opencl-icd
```

Tämän jälkeen ajo onnistui ja hashcat käytti tietokoneen suoritinta laskentaan näytönohjaimen sijaan.

<img width="630" height="470" alt="image" src="https://github.com/user-attachments/assets/cd74a86b-6c01-4fca-9cd6-1e16e3391604" />

Tarkistin löytyneen salasanan: `cat solved`

<img width="339" height="60" alt="image" src="https://github.com/user-attachments/assets/f7929660-aadf-4e4d-ae13-4ba03f201334" />

Lähteet:
- [Karvinen 2022: Cracking Passwords with Hashcat](https://terokarvinen.com/2022/cracking-passwords-with-hashcat/)
- [pocl-opencl-icd: Debian Package](https://packages.debian.org/bookworm/pocl-opencl-icd)
- [hashcat Forum](https://hashcat.net/forum/archive/index.php?thread-10695.html=)

## c) Asenna John the Ripper ja testaa sen toiminta murtamalla jonkin esimerkkitiedoston salasana.
*Seurasin Karvisen artikkelin ohjeita (ks. x-kohdan tiivistelmä).*
### Riippuvuuksien asennus

Asensin kääntämiseen tarvittavat paketit:

```bash
sudo apt-get update
sudo apt-get -y install micro bash-completion git build-essential libssl-dev zlib1g zlib1g-dev libbz2-1.0 libbz2-dev atool zip wget
```
**Huom:** Alkuperäisen artikkelin pakettilistassa mainittu `zlib.gst` ei löytynyt nykyisestä Kali-repositoriasta. Jätin paketin pois listalta.
<img width="931" height="122" alt="image" src="https://github.com/user-attachments/assets/b213be89-2204-4577-985d-bd3db3242f82" />


<img width="934" height="236" alt="image" src="https://github.com/user-attachments/assets/87ec6467-add8-491c-8ad0-65184262019f" />
*lyhennetty kuva*

### John the Ripper Jumbo -version kääntäminen

Kloonasin ja käänsin John the Ripperin Jumbo-version lähdekoodista:

```bash
git clone --depth=1 https://github.com/openwall/john.git
cd john/src/
./configure
make -s clean && make -sj4
```

Käännetyt suoritettavat tiedostot ja skriptit löytyvät `run/`-kansiosta:

```bash
cd ../run/
```

Testasin että käännös onnistui: `./john`

<img width="919" height="164" alt="image" src="https://github.com/user-attachments/assets/95aaad83-f72b-4d22-93b5-db149ecdefec" />

### Esimerkkitiedoston murto (ZIP)

Latasin artikkelin esimerkkitiedoston, salasanasuojatun ZIP-arkiston:

```bash
wget https://Terokarvinen.com/2023/crack-file-password-with-john/tero.zip
```

Poimin ZIP-tiedoston salasanan hashin:

```bash
./zip2john tero.zip > tero.zip.hash
```

<img width="941" height="106" alt="image" src="https://github.com/user-attachments/assets/bedc274d-7e31-427b-a4c2-f5033e95d9bf" />

Ajoin Johnin hash-tiedostoa vastaan:

```bash
./john tero.zip.hash
```

John löysi sanakirjahyökkäyksellä salasanan muutamassa sekunnissa **butterfly**.

<img width="786" height="266" alt="image" src="https://github.com/user-attachments/assets/4f374fae-2598-492d-94e7-094f93a8213c" />

Vahvistin tuloksen: `./john --show tero.zip.hash`

Purin ZIP:n löydetelly salasanalla ja tarkistin sisällön:

```bash
unzip tero.zip
cat secretFiles/SECRET.md
```

Tiedosto avautui onnistuneesti.

<img width="673" height="393" alt="image" src="https://github.com/user-attachments/assets/c3e2ce7d-d9c6-42e5-818c-164075b43b29" />

Lähteet:
- [Karvinen 2023: Crack File Password With John](https://terokarvinen.com/2023/crack-file-password-with-john/)

## e) Tiedosto. Tee itse tai etsi verkosta jokin salakirjoitettu tiedosto, jonka saat auki.

### Salatun tiedoston luonti

Tein LibreOfficella lyhyen tekstitiedoston ja exporttasin sen PDF:ksi salasanalla suojattuna.
(File -> Export As -> Export as PDF -> Security -> Set Passwords)

<img width="667" height="637" alt="image" src="https://github.com/user-attachments/assets/fa5a8b4c-4e62-45e4-b52e-bd9b751ef553" />

### Hashin poiminta

Poimin PDF:n salauksen hashin `pdf2john.pl`-skriptillä:

```bash
./pdf2john.pl /home/kali/hashed/salainen.pdf > /home/kali/hashed/pdf.hash
```
<img width="642" height="48" alt="image" src="https://github.com/user-attachments/assets/b36423d3-2b8a-4d1d-b2c2-fb7f49bb4e6b" />

<img width="936" height="81" alt="image" src="https://github.com/user-attachments/assets/059bfff2-3459-4a43-9199-bc7bdd9fa513" />

### Murto

Ajoin Johnin hash-tiedostoa vastaan:

```bash
./john /home/kali/hashed/pdf.hash
```
John tunnisti hashin PDF-salaukseksi (MD5-RC4 / SHA2-AES) ja löysi salasanan sanakirjahyökkäyksellä: **password123**

<img width="882" height="289" alt="image" src="https://github.com/user-attachments/assets/67ab84e4-1924-4a82-8430-7f3f667b10c7" />

Vahvistin tuloksen:

```bash
./john --show /home/kali/hashed/pdf.hash
```

<img width="365" height="95" alt="image" src="https://github.com/user-attachments/assets/dfdf55a7-13d5-4b6e-8ad0-b90962db99f9" />

<img width="943" height="329" alt="image" src="https://github.com/user-attachments/assets/ecf2fe10-e566-401d-ae3d-acff5fb83115" />


Lähteet:
- [Openwall: John the Ripper documentation](https://www.openwall.com/john/doc/)
- [Karvinen 2023: Crack File Password With John](https://terokarvinen.com/2023/crack-file-password-with-john/)


## f) Tiiviste. Tee itse tai etsi verkosta jokin salakirjoitettu tiedosto, jonka saat auki. Murra sen salaus.

### Ensimmäinen yritys: yescrypt (/etc/shadow)

Loin uuden käyttäjän järjestelmään ja tarkistin hänen tiivisteen /etc/shadow-tiedostosta.

```bash
sudo adduser testi
sudo grep testi /etc/shadow
```

Tiiviste alkoi `$y$`, mikä tarkoittaa yescrypt-algoritmia. Yritin murtaa tämän hashcatilla (`-m 70200`), mutta se antoi virheilmoituksen "No hashes loaded". En ole varma tarkasta syystä, mutta veikkaan että se liittyy yescryptin tuoreuteen.

<img width="936" height="411" alt="image" src="https://github.com/user-attachments/assets/8a06bd25-d06f-4865-9393-5cb98e1fa5ff" />

### Toinen yritys: SHA-512 crypt (onnistui)

Loin sen sijaan oman SHA-512 crypt -tiivisteen opensslilla, jolloin sain valita algoritmin itse eikä tarvinnut luottaa järjestelmän oletusarvoon:

```bash
openssl passwd -6 -salt "testi" kissa123
```

`-6` valitsee SHA-512-algoritmin, `-salt "testi"` on itse valitsemani suola ja `kissa123` on salasana jonka tiivistin.

Tallensin tuloksen tiedostoon:

<img width="941" height="188" alt="image" src="https://github.com/user-attachments/assets/d207216f-fc3c-4cc1-a08d-2da5ff00c29e" />


### Murto

Ajoin Johnin hash-tiedostoa vastaan:

```bash
./john ~/hashed/sha512_hash.txt
```

John tunnisti algoritmin automaattisesti tunnisteen `$6$`-perusteella: "Loaded 1 password hash (sha512crypt, crypt(3) $6$)". Sanakirjahyökkäys löysi salasanan lähes välittömästi: **kissa123**

<img width="863" height="302" alt="image" src="https://github.com/user-attachments/assets/e87c786e-eacf-4755-86c1-7cb4038ff4b3" />

Vahvistin tuloksen:

<img width="379" height="62" alt="image" src="https://github.com/user-attachments/assets/5020d396-4fda-4608-845d-097ee6b1b014" />

Lähteet:
- [Openwall: John the Ripper documentation](https://www.openwall.com/john/doc/)
- [OpenSSL Manual: openssl-passwd Password Hash Generator](https://docs.openssl.org/master/man1/openssl-passwd/)


## g) Sanakirja

Oman sanakirjan teko parantaa murron onnistumismahdollisuuksia erityisesti silloin, kun salasana perustuu johonkin ennustettavaan malliin, jota yleisest sanakirjat kuten RockYou eivät kata.

### Oman sanakirjan luonti

Tein sanakirjan, joka sisältää kaikki kuukaudet yhdistettynä muutamaan vuoteen ja huutomerkkiin. Tämä on yleinen salasanamalli yrityksissä, joissa salasana vaihdetaan säännöllisesti (esim. "Elokuu2026!").

```bash
for kk in Tammikuu Helmikuu Maaliskuu Huhtikuu Toukokuu Kesakuu Heinakuu Elokuu Syyskuu Lokakuu Marraskuu Joulukuu; do for vuosi in 2024 2025 2026; do echo ${kk}${vuosi}!; done; done > oma_sanakirja.txt
```

Tämä loi 36 riviä sisältävän sanakirjan (12 kk x 3 vuotta).

<img width="217" height="624" alt="image" src="https://github.com/user-attachments/assets/54c1c4f9-e713-400e-9bec-2bba8f079f60" />

### Demonstraatio.

Loin hashin salasanasta "Elokuu2026!":

```bash
echo -n 'Elokuu2026!' | md5sum
```

Kokeilin ensin murtaa sen RockYou-sanakirjalla

<img width="599" height="63" alt="image" src="https://github.com/user-attachments/assets/8293ea19-43a4-4a94-a949-7ca3bc7d2758" />

Ajo päättyi tilaan **"Exhausted"**. Kaikki yli 14 miljoonaa sanaa käytiin läpi, mutta salasanaa ei löytynyt, koska "Elokuu2026!" ei ole tavallinen, yleisesti vuodettu salasana.

<img width="247" height="32" alt="image" src="https://github.com/user-attachments/assets/4d75ab2e-d7c8-4cd4-8f1e-3ce4a46bd15a" />

Kokeilin sitten samaa hashia omalla sanakirjallani:

<img width="654" height="63" alt="image" src="https://github.com/user-attachments/assets/dca7ab68-f433-47be-8ea4-70e259ad358a" />

<img width="253" height="40" alt="image" src="https://github.com/user-attachments/assets/101341b6-2825-483f-b0f7-dca147957e97" />

Salasana löytyi välittömästi: **Elokuu2026!**

<img width="383" height="62" alt="image" src="https://github.com/user-attachments/assets/ceed7ece-dffc-437d-a530-edd98aba386f" />

Lähteet:
- Vinkit: [Terokarvinen.com](https://terokarvinen.com/tunkeutumistestaus/)


## h) Hash rules

HashCatin säännöt muuntavat sanakirjan sanoja automaattisesti eri tavoin ilman että jokaista muunnelmaa tarvitsee kirjoittaa käsin sanakirjaan. Esimerkiksi: numeron lisäys perään, ison alkukirjaimen lisäys.

### Sääntötiedoston tarkistus

Tarkistin mitä valmis best66.rule-sääntötiedosto sisältää:

```bash
cat /usr/share/hashcat/rules/best66.rule
```

Listasta löytyi mm. "simple number append" -osio, joka sisältää säännöt `$0` - `$9`, eli yhden numeron lisäämisen sanan loppuun.


<img width="326" height="285" alt="image" src="https://github.com/user-attachments/assets/370e4647-67eb-411d-8f46-539c8c390f40" />
*enemmän tietoa alapuolella*

### Testisanakirja

Loin pienen testisanakirjan, jossa on vain yksi sana:

`echo 'kissa' > rules_testi.txt`

### Demonstraatio

Loin hashin muunnellusta versiosta "kissa1"

`echo -n 'kissa1' | md5sum`

Kokeilin ensin murtaa sen ilman sääntöjä:

<img width="568" height="66" alt="image" src="https://github.com/user-attachments/assets/3c4b2dfe-f750-4ef5-95c9-085717bb1cfe" />

<img width="254" height="40" alt="image" src="https://github.com/user-attachments/assets/9383d4ec-f9db-439b-b544-99ea9b9f765d" />

Ajo epäonnistui, koska sanalistassa oli vain "kissa", ei "kissa1".

Kokeilin sitten samaa hashia rules-parametrin kanssa:

<img width="929" height="59" alt="image" src="https://github.com/user-attachments/assets/a50f032a-96c7-4bfb-b6a3-182abdeb1d6a" />

Tulos: **kissa1** löytyi. Hashcat kokeili sanaa "kissa" sekä sellaisenaan että kaikilla best66.rule-tiedoston sisältämillä muunnoksilla, ja yksi näistä muunnoksista tuotti oikean salasanan.

lähteet:
- Vinkit: [terokarvinen.com](https://terokarvinen.com/tunkeutumistestaus/)

## Lähteet:
- [Karvinen 2022: Cracking Passwords with Hashcat](https://terokarvinen.com/2022/cracking-passwords-with-hashcat/)
- [Karvinen 2023: Crack File Password With John](https://terokarvinen.com/2023/crack-file-password-with-john/)
- [pocl-opencl-icd: Debian Package](https://packages.debian.org/bookworm/pocl-opencl-icd)
- [hashcat Forum](https://hashcat.net/forum/archive/index.php?thread-10695.html=)
