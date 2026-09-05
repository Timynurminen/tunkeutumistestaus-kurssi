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

