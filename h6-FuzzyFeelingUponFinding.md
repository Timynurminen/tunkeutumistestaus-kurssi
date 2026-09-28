# h6 Fuzzy Feeling Upon Finding

## x) Lue/katso/kuuntele

### Hoikkala 2026: Fuzzing with Fuff

- Ffuf lähetää suuren määrän HTTP-pyyntöjä ja tunnistaa poikkeamia (status-koodi, koko, vasteaika, regex).
- Sanakirjan avainsana (oletus `FUZZ`) voidaan sijoittaa mihin tahansa pyynnön osaan (URL, header, data), ja useita sanakirjoja voi yhdistää samassa ajossa.
- Nopeusrajoitus (`-rate`) on tärkeä, ettei kohdetta kuormiteta liikaa.
- Kertakäyttöisen CRSF-tokenin vaativat lomakkeet eivät toimi suoralla fuzzauksella. Ratkaisu on `-preflight`, joka lähettää esipyynnön ennen jokaista fuzzuspyyntöä ja poimii siitä (`-preflight-var`) tuoteen tokenin, joka sijoitetaan varsinaiseen pyyntöön.
