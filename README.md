# Kummisetä - verkkokaupan ulkoasu

Divari & Antikvariaatti Kummisetä Oy:n verkkokaupan asiakasnäkymän ulkoasu.
Suunnittelu JH Analytics, 10/2026.

## Tiedostot

| Tiedosto | Sisältö |
|---|---|
| `etusivu.html` | Etusivu: yläpalkki, header, navigaatio, kampanjahero, uusimmat, Kummisetä suosittelee, liike-esittely, footer |
| `tuotesivu.html` | Yksittäisen kirjan sivu: kuvat, ostolaatikko, kunto ja huomiot, muut kappaleet, tiedot, kuvaus, samasta hyllystä |
| `tuotelistaus.html` | Kategoriasivu: sivupalkin suodattimet, järjestys, tuoteruudukko, sivutus |
| `hakutulos.html` | Hakutulossivu: sama rakenne kuin listaus + hakuvinkki |
| `tyyliopas.html` | Värit, typografia, painikkeet, kuntomerkinnät ja logo yhdellä sivulla |
| `css/kummiseta.css` | Kaikki tyylit, osioittain kommentoituna |
| `logo/` | Logo SVG:nä, musta ja valkoinen versio |
| `img/` | Esimerkkikuvat (kirjojen kannet ovat vain esimerkkejä, ei tuotantokäyttöön) |
| `favicon.svg`, `favicon.ico`, `apple-touch-icon.png` | Favicon-setti |

## Miten CSS on rakennettu

Kaikki värit, fontit ja pyöristykset ovat `:root`-muuttujina tiedoston alussa.
Muualla niihin viitataan `var(--brand)` -tyyliin, joten esimerkiksi päävärin
vaihto on yhden rivin muutos.

Tärkeimmät muuttujat:

| Muuttuja | Käyttö |
|---|---|
| `--brand` | Pääväri (viininpunainen): painikkeet, yläpalkki, hero, footer |
| `--accent` | Korostus (keltainen): kampanjapainikkeet, korin lukumäärä |
| `--paper` / `--surface` | Sivun tausta / valkoiset pinnat |
| `--ink` / `--muted` | Teksti / toissijainen teksti |
| `--line` / `--tint` | Viivat / vaaleat sävypinnat |
| `--f-head` / `--f-body` | Otsikkofontti (Crimson Pro) / leipäteksti (Source Sans 3) |

Tiedosto etenee järjestyksessä muuttujat -> perus -> header ja navigaatio ->
etusivu -> komponentit (painikkeet, tuotekortti, kuntoluokitus) -> tuotesivu ->
footer -> responsiivisuus -> tarkennukset -> tuotelistaus. Tarkennukset-osio on
lisätty kerroksena perussääntöjen päälle, joten sama valitsin voi esiintyä
kahdesti; jälkimmäinen voittaa. Jos haluat yhdistää ne, se on turvallista tehdä
osio kerrallaan.

Leikkauspisteet: 1320 px (yläpalkin kolmas USP piiloon), 1100 px (tabletti),
760 px (mobiili: hampurilaisvalikko, haku omalle riville, kategoriaruudukko).

## Toistuvat komponentit

- **Yläpalkki** `.utility`: vasemmalla USP:t, oikealla Yhteystiedot, Ota yhteyttä
  ja korostettu `Tarjoa kirjoja myytäväksi`. Mobiilissa vain painike näkyy;
  yhteystietolinkit kuuluvat hampurilaisvalikkoon.
- **Tuotekortti** `.card`: kansi, kuntopallerot `.pips` (1-5 täytettyä),
  tekijä, nimi, sidosasu ja kunto, hinta ja ostoskorikuvake. Alennushinta
  `.old` yliviivattuna ja `-25 %` -merkki kannen kulmassa.
- **Kuntoluokitus**: viisi palleroa, täytettyjen määrä = kuntoluokka K1-K5.
- **Painikkeet** `.btn`: `.primary` (pääväri tai hero-näkymässä keltainen),
  `.secondary` (valkoinen pohja, pääväri teksti), `.ghost` (reunaviiva).
- **Suodatinpalkki** `.applied`: listauksen yläpuolella näkyvä palkki, joka
  kertoo käytössä olevat suodattimet ja hakusanan. Jokainen suodatin on oma
  poistettava merkki, lisäksi "Poista kaikki suodattimet". Näkyy aina kun
  vähintään yksi suodatin on päällä.
- **Suodattimet** `.filters`: jokaisen vaihtoehdon perässä tuotemäärä.
  Pitkä lista (alakategoriat) näyttää kuusi ensimmäistä, loput
  `.more-item`-luokalla piilossa ja "Näytä kaikki" -linkin takana; listan
  alussa hakukenttä `.facet-search`. Hinta ja painovuosi: liukusäädin `.slider`
  (staattinen malli, toteutus esim. kahdella `input type=range`) ja sen alla
  numerokentät, jotka toimivat myös ilman säädintä.
- **Muut kappaleet** `.copies` (tuotesivu): taulukko saman teoksen muista
  kappaleista, avaimena ISBN. Sarakkeet: kansi, painos ja vuosi, sidosasu,
  kunto, huomiot, hinta ja Katso-painike. Halvin ensin, katsottava kappale
  `tr.this`-luokalla korostettuna. Mobiilissa rivit muuttuvat korteiksi.
  Ostolaatikon "3 kappaletta · vertaa" -linkki vie ankkuriin `#kappaleet`.
- **Kummisetä suosittelee** `.picks` / `.pick` (etusivu, hakusivu kun osumia
  on alle neljä): käsin valitut nostot, kansi, tekijä, nimi, kauppiaan saate
  `.say` ja hinta. Suositukset tulevat Ecwidin omasta kategoriasta, jota
  kauppias ylläpitää.
- **Samasta hyllystä** `.reco` (tuotesivu): automaattinen nosto samasta
  alakategoriasta, tavallinen korttiruudukko.
- **Kappalemerkintä kortissa** `.copies-n`: listauksessa ja haussa yksi kortti
  per teos, merkintä "4 kappaletta · 7–24 €"; kortti vie halvimman kappaleen
  sivulle, jossa kappaletaulukko on.
- **Pudotusvalikko** `.toolbar select`: selaimen oma nuoli korvattu
  CSS-taustakuvalla, toimii ilman JavaScriptiä.

## Mitä paketti ei sisällä

- Ostoskori ja kassa ovat Ecwidin omia näkymiä. Niihin vaikutetaan vain CSS:llä;
  ne eivät kuulu tähän pakettiin.
- Hampurilaisvalikon avautuva sisältö on toteuttajan vastuulla. Mobiilin
  kategorialinkit löytyvät etusivulta `.mob-cats`-lohkosta mallina.
- Lomakkeet (uutiskirje, Tarjoa kirjoja myytäväksi) ovat ulkoasun osalta
  mukana vain linkkeinä.

## Fontit

Crimson Pro ja Source Sans 3 ladataan Google Fontsista (linkki sivujen
`<head>`-osassa). Molemmat ovat ilmaisia (SIL Open Font License) ja voi myös
hostata itse.
