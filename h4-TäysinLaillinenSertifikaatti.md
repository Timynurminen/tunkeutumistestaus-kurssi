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
- Suojatuminen edellyttää **server-puolen autorisointia
