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
