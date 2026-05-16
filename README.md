# GeoHackathon.
## EKIPA SOKCI: Urbani Prevajalec: Ljubljana 4D
Tukaj je samo naš index file(ogrodje) in ne tudi file-i s podatki o stavbah in objektih, saj so preveliki za github.
V index3.html manjkajo tudi API ključi za MapBox in Gemini, ki se uporabljajo v našem projektu. Manjkajo, ker jih git zaradi varnosti ne pusti dati na github.


# Urbani Prevajalec: Ljubljana 4D

**Urbani Prevajalec: Ljubljana 4D** je interaktivna spletna aplikacija, ki prikazuje 3D/4D maketo Ljubljane s pomočjo Mapboxa in GeoJSON podatkov. Projekt omogoča raziskovanje stavb skozi čas, filtriranje objektov po izbranih parametrih, iskanje konkretnih stavb ter AI razlago surovih GIS podatkov v uporabniku prijazen jezik.

Namen projekta je približati kompleksne prostorske podatke širši javnosti. Namesto da uporabnik bere surove GeoJSON atribute ali strokovne GIS sloje, lahko klikne na stavbo in dobi razumljiv povzetek njenih podatkov.

---

## Glavna ideja

Veliko prostorskih podatkov o mestih že obstaja, vendar so pogosto:

- razpršeni po različnih virih,
- zapisani v tehničnih formatih,
- težko razumljivi za navadne uporabnike,
- zahtevni za hitro vizualno primerjavo.

Naš projekt rešuje ta problem tako, da združi:

```text
GeoJSON podatke + Mapbox 3D karto + časovni prikaz + filtre + iskanje + AI razlago
```

Rezultat je interaktivna 4D maketa Ljubljane, kjer lahko uporabnik razume, kako se mesto spreminja skozi čas in kaj pomenijo podatki o posamezni stavbi.

---

## Funkcionalnosti

### 1. 3D prikaz Ljubljane

Aplikacija uporablja **Mapbox GL JS** za prikaz interaktivnega zemljevida Ljubljane. Stavbe iz GeoJSON datotek so prikazane kot objekti na karti, z možnostjo 3D vizualizacije.

---

### 2. 4D časovni slider

Dodali smo časovni slider, ki omogoča prikaz sprememb stavb skozi čas.

Uporabnik lahko premika letnico in vidi, katere stavbe so bile oziroma bodo prikazane glede na časovne podatke v GeoJSON-u.

Primer uporabe:

```text
Premik sliderja iz leta 2024 proti 2030 prikaže razvoj stavb skozi čas.
```

---

### 3. Filter meni

Aplikacija vsebuje meni s filtri, kjer lahko uporabnik izbere določene parametre. Na zemljevidu se nato prikažejo samo stavbe, ki ustrezajo izbranim pogojem.

Možni primeri filtrov:

- tip stavbe,
- leto izgradnje,
- višina stavbe,
- namenska raba,
- poplavna ogroženost,
- kulturna dediščina,
- območja z določenimi urbanističnimi pogoji.

Filtri omogočajo hitrejše raziskovanje velikih količin prostorskih podatkov.

---

### 4. Search bar

Dodali smo iskalnik, ki omogoča hitro iskanje stavb po podatkih iz GeoJSON datotek.

Uporabnik lahko išče po:

- imenu stavbe,
- ID-ju,
- naslovu,
- tipu objekta,
- drugih atributih, ki obstajajo v GeoJSON properties.

Ko uporabnik izbere rezultat, se aplikacija premakne na ustrezno stavbo in jo označi na zemljevidu.

---

### 5. Klik na stavbo

Ob kliku na posamezno stavbo se v stranskem panelu prikažejo njeni surovi GIS podatki iz GeoJSON datoteke.

To omogoča neposreden vpogled v podatke, ki jih aplikacija uporablja za vizualizacijo in AI razlago.

---

### 6. AI razlaga stavbe

Poleg surovih podatkov ima uporabnik možnost klikniti gumb za AI razlago.

AI prebere podatke iz izbrane stavbe in jih pretvori v bolj razumljiv opis.

Primer:

```text
Namesto da uporabnik vidi samo tehnične atribute, kot so višina, leto izgradnje, namenska raba ali poplavno območje, AI pripravi kratek povzetek, ki pojasni pomen teh podatkov.
```

Cilj je, da aplikacija deluje kot **urbani prevajalec** med tehničnimi GIS podatki in navadnim uporabnikom.

---

## Tehnologije

Projekt uporablja:

- **HTML**
- **CSS**
- **JavaScript**
- **Mapbox GL JS**
- **GeoJSON**
- **Gemini AI API**
- **Font Awesome**
- **Google Fonts**

---

## Struktura projekta

Primer strukture projekta:

```text
project-root/
│
├── index.html
├── README.md
├── logo-sokci.png
│
└── public/
    ├── geojson/
    │   ├── stavbe.geojson
    │   ├── parcele.geojson
    │   └── drugi-sloji.geojson
    │
    └── geojson-manifest.json
```

GeoJSON datoteke so shranjene v mapi `public`, aplikacija pa jih naloži in prikaže na Mapbox zemljevidu.

---

## Pomembno glede koordinatnega sistema

Mapbox pričakuje GeoJSON koordinate v koordinatnem sistemu:

```text
EPSG:4326 - WGS 84
```

Koordinate morajo biti zapisane kot:

```text
[longitude, latitude]
```

Primer za Ljubljano:

```text
[14.5058, 46.0569]
```

Če so GeoJSON datoteke v slovenskem državnem koordinatnem sistemu:

```text
EPSG:3794
```

jih je treba pred uporabo v Mapboxu pretvoriti v:

```text
EPSG:4326
```

Primer pretvorbe z `ogr2ogr`:

```bash
ogr2ogr -f GeoJSON -s_srs EPSG:3794 -t_srs EPSG:4326 output.geojson input.geojson
```

Ali v QGIS-u:

```text
Right click layer
→ Export
→ Save Features As
→ Format: GeoJSON
→ CRS: EPSG:4326 - WGS 84
```

---

## Kako zagnati projekt lokalno

Ker aplikacija nalaga GeoJSON datoteke z `fetch()`, je priporočljivo projekt zagnati prek lokalnega serverja.

V terminalu pojdi v mapo projekta:

```bash
cd path/to/project
```

Zaženi lokalni server:

```bash
python3 -m http.server 8000
```

Nato odpri v brskalniku:

```text
http://localhost:8000
```

---

## API ključi

Za delovanje Mapbox zemljevida je potreben Mapbox access token.

Za AI razlago je potreben Gemini API ključ.

V kodi je treba zamenjati placeholder vrednosti:

```js
mapboxgl.accessToken = "YOUR_MAPBOX_ACCESS_TOKEN";
```

in:

```js
const GEMINI_API_KEY = "YOUR_GEMINI_API_KEY";
```

API ključev ni priporočljivo javno objavljati v GitHub repozitoriju. Za produkcijsko verzijo bi bilo bolje uporabiti backend ali okoljske spremenljivke.

---

## Podatki

Projekt uporablja GeoJSON datoteke s prostorskimi podatki o stavbah in drugih urbanih slojih.

Podatki lahko vsebujejo različne atribute, na primer:

- ID stavbe,
- ime ali naslov,
- leto izgradnje,
- višino,
- namensko rabo,
- tip objekta,
- poplavno ogroženost,
- urbanistične omejitve,
- druge GIS atribute.

Aplikacija iz teh podatkov ustvari vizualni prikaz, omogoči filtriranje in pripravi AI povzetek izbrane stavbe.

---

## Namen projekta

Projekt je bil razvit kot prototip pametne urbane aplikacije, ki prikazuje, kako lahko združimo:

- odprte prostorske podatke,
- 3D kartografsko vizualizacijo,
- časovno dimenzijo,
- uporabniške filtre,
- umetno inteligenco.

Glavni namen je narediti mestne podatke bolj razumljive, dostopne in uporabne.

---

## Potencial za nadaljnji razvoj

V prihodnje bi lahko projekt nadgradili z dodatnimi podatkovnimi sloji in funkcionalnostmi.

Možne nadgradnje:

### Dodatni podatkovni sloji

- poplavna ogroženost,
- potresna nevarnost,
- hrup,
- kakovost zraka,
- osončenost stavb,
- solarni potencial,
- kulturna dediščina,
- bližina javnega prometa,
- bližina zelenih površin,
- namenska raba prostora,
- zazidljivost,
- prostorski akti.

---

### Naprednejši AI povzetki

AI bi lahko pripravljal različne tipe razlag glede na uporabnika:

- za občane,
- za urbaniste,
- za investitorje,
- za študente,
- za turiste,
- za raziskovalce.

Primeri vprašanj:

```text
Ali je ta stavba na varnem območju?
Kaj je zanimivega pri tej lokaciji?
Kakšna so tveganja?
Kakšen je razvojni potencial tega območja?
Kako se je ta del mesta spreminjal skozi čas?
```

---

### Boljša zmogljivost

Za večje količine podatkov bi bilo smiselno preiti iz direktnega nalaganja GeoJSON datotek na:

- Mapbox vector tiles,
- Mapbox tilesets,
- PostGIS bazo,
- backend API,
- streženje prostorskih podatkov po ploščicah.

To bi omogočilo hitrejše delovanje tudi pri zelo velikem številu stavb.

---

## Zaključek

**Urbani Prevajalec: Ljubljana 4D** prikazuje, kako lahko kompleksne GIS podatke spremenimo v interaktivno, razumljivo in uporabno orodje.

Projekt ne prikazuje samo stavb na zemljevidu, ampak omogoča uporabniku, da raziskuje mesto skozi čas, filtrira pomembne podatke, poišče konkretne objekte in s pomočjo AI razume, kaj podatki dejansko pomenijo.

Glavna vrednost projekta je v tem, da mesto postane bolj berljivo:

```text
ne samo kot karta, ampak kot razumljiv digitalni prostor.
```
