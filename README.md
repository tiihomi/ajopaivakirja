# Ajopäiväkirja

Ajopäiväkirja on puhelimen selaimessa toimiva sovellus. Sen voi lisätä kotinäytölle, jolloin se toimii kuin tavallinen sovellus.

- **Aloita ajo:** valitse auto ja paina *Aloita ajo*. GPS hakee lähtöpaikan osoitteen.
- **Välipisteet:** jos ajat useamman kohteen kautta, paina ajon aikana *Lisää välipiste*. Välipisteet tulevat reittiin ja kilometrilaskentaan.
- **Lopeta ajo:** GPS hakee määränpään, ja sovellus laskee ajoreitin pituuden tieverkkoa pitkin.
- **Tarkista ja tallenna:** voit korjata kaikki kentät ennen tallennusta: osoitteet, kilometrit, reitin, tarkoituksen ja mittarilukemat.
- **Lisää ajo käsin:** tätä käytetään, kun GPS ei ollut käytössä tai ajo unohtui kirjata.
- **Vie Exceliin:** ajot ja kuukausiyhteenveto (autoittain ja kuljettajittain) CSV-tiedostona, joka aukeaa suoraan Excelissä.

## Lisenssit ja kustannukset: 0 €

| Osa | Palvelu | Hinta |
| --- | --- | --- |
| Sovellus | HTML ja JavaScript ilman kirjastoja | ilmainen |
| Sijainti | Puhelimen GPS selaimen kautta | ilmainen |
| Osoite koordinaateista | [Nominatim](https://nominatim.org/) (OpenStreetMap) | ilmainen, [käyttöehdot](https://operations.osmfoundation.org/policies/nominatim/) |
| Ajoreitin pituus | [OSRM](https://project-osrm.org/) (OpenStreetMap) | ilmainen demopalvelin |
| Ulkoasu | [Material Design 3](https://m3.material.io/) -tyyli ja Material Icons (Apache 2.0), vaalea ja tumma teema | ilmainen |
| Fontti | Roboto (Google Fonts) | ilmainen |
| Julkaisu | GitHub Pages, Azure Static Web Apps Free tai vastaava | ilmainen |

Nominatim ja OSRM ovat ilmaisia yhteisöpalveluja, joten niiden saatavuutta ei taata. Pienen tiimin käyttö (muutama haku ajoa kohden) on hyvin niiden rajoissa. Jos palvelu ei vastaa, sovellus näyttää koordinaatit ja linnuntien etäisyyden, ja tiedot voi korjata käsin. Palveluille lähtevät vain koordinaatit, ei nimiä eikä rekisteritunnuksia.

## Julkaisu

Puhelimen paikannus toimii vain HTTPS-osoitteessa, joten sovellus pitää julkaista verkkoon. Julkaistavat tiedostot:

```
index.html
manifest.webmanifest
sw.js
icons/
```

Ohjelmointia tai rakennusvaihetta ei tarvita. Tiedostot vain kopioidaan palvelimelle.

**Vaihtoehto A: Azure Static Web Apps (Free).** Suositeltava, jos yrityksen IT ylläpitää Azurea. Pyydä IT:tä luomaan Static Web App ja lataamaan tiedostot. Voit myös rajata pääsyn yrityksen tunnuksiin.

**Vaihtoehto B: GitHub Pages.**
1. Luo GitHub-repositorio ja lataa siihen yllä olevat tiedostot.
2. Valitse *Settings → Pages → Deploy from a branch → main / (root)*.
3. Osoite on muotoa `https://<käyttäjä>.github.io/<repo>/`.

Sovelluksen koodi on julkinen, mutta ajotiedot eivät ole, koska ne pysyvät puhelimissa. Tarkista yrityksen IT-ohjeet ennen julkaisua.

**Paikallinen testaus tietokoneella:**

```bash
python -m http.server 8765
```

Avaa sitten selaimessa http://localhost:8765.

## Asennus puhelimeen

1. Avaa sovelluksen osoite puhelimessa.
2. Lisää sovellus kotinäytölle:
   - **iPhone (Safari):** valitse *Jaa → Lisää Koti-valikkoon*.
   - **Android (Chrome):** valitse *⋮ → Asenna sovellus* tai *Lisää aloitusnäyttöön*.
3. Avaa sovellus kuvakkeesta ja salli sijainti, kun puhelin kysyy.
4. Lisää *Asetuksissa* nimesi ja autojen rekisteritunnukset.

> **Lisää sovellus kotinäytölle, varsinkin iPhonessa.** Safari voi poistaa selaimessa käytetyn sivuston tiedot, jos sivustoa ei ole avattu 7 päivään. Kotinäytölle lisätty sovellus ei menetä tietojaan näin.

## Tietojen säilytys

Ajot tallentuvat **vain kyseisen puhelimen** selaimeen. Mitään ei lähetetä palvelimelle.

- **Varmuuskopio:** valitse *Asetukset → Tallenna varmuuskopio*, ja tallenna tiedosto esim. OneDriveen. Ota varmuuskopio säännöllisesti, esim. kuukausiraportin yhteydessä.
- **Uusi puhelin:** asenna sovellus ja valitse *Asetukset → Palauta varmuuskopiosta*. Palautus yhdistää ajot eikä tuota tuplia.
- **Mittarilukema:** sovellus esitäyttää alkulukemaksi auton edellisen ajon loppulukeman *tässä puhelimessa*. Jos kollega on ajanut autoa välissä, korjaa lukema mittarista.

## Ajopäiväkirjan sisältö (Verohallinto)

Jokaisesta ajosta kirjataan:
- päivämäärä sekä lähtö- ja saapumisaika
- rekisteritunnus ja kuljettaja
- lähtöpaikka, määränpää ja reitti
- ajon tyyppi (työ- tai yksityisajo)
- ajon tarkoitus (pakollinen työajoissa)
- matkamittarin lukemat
- ajetut kilometrit ja tieto siitä, mistä kilometrit tulivat (GPS, matkamittari vai käsin)

## Rajoitukset

- Selainsovellus ei voi seurata reittiä taustalla, kun näyttö on kiinni. Siksi GPS-sijainti haetaan ajon alussa, lopussa ja välipisteissä, ja niiden välinen tiematka lasketaan. Jos ajoit kiertotietä, korjaa kilometrit tai käytä välipisteitä.
- Kaikki ajot yhdessä paikassa vaatisi yhteisen tietokannan, esim. SharePoint-listan tai Supabasen ilmaisen tason. Sen voi lisätä myöhemmin.

## Päivittäminen

Kun muutat tiedostoja, kasvata `sw.js`:n `VERSION`-arvoa (esim. `ajopk-v2`). Puhelimet hakevat uuden version, kun sovellus seuraavan kerran avataan verkkoyhteyden kanssa.
