# Croatia CityGuessr

Mala, proširiva web-igra u hrvatskom jeziku: pet nasumičnih gradova, interaktivna karta, oznaka odgovora, Haversineova udaljenost, bodovanje i završni pregled.

## Pokretanje

Za ES module i JSON import pokreni lokalni HTTP poslužitelj iz direktorija projekta (nemoj otvarati `index.html` direktno kao `file://`):

```bash
python -m http.server 8000
```

Zatim otvori http://localhost:8000. Alternativa: `npx serve .`.

## Tehnologije i mrežni zahtjevi

- Vanilla JavaScript (ES modules), HTML i CSS; nema build koraka ni API ključa.
- Leaflet 1.9.4 učitava se s unpkg CDN-a.
- Karta koristi OpenStreetMap pločice i automatski prelazi na CARTO Voyager ako OSM pločice učestalo prijavljuju pogreške. Oba izvora zahtijevaju internet i podliježu vlastitim pravilima korištenja, limitima i atribuciji. Za javni prometni ili komercijalni servis provjeri uvjete pružatelja i po potrebi koristi ugovorenog pružatelja pločica.
- Fontovi se dohvaćaju s Google Fonts; ako nisu dostupni, koriste se sistemski fontovi.
- Nema API ključa. Karta se ne može koristiti potpuno offline.

## Struktura

```text
croatia-cityguessr/
├── index.html
├── README.md
├── LICENSE
└── src/
    ├── app.js
    ├── cities.json
    └── style.css
```

## Gradovi i koordinate

`src/cities.json` sadrži početni skup gradova, približno središnjih koordinata i regija. Koordinate su praktične referentne točke za igru, a ne službene administrativne granice ili geodetski certificirani podaci. Prije produkcijske objave preporučuje se provjera i dopuna iz Wikidata/OSM izvora, uz bilježenje izvora i datuma uvoza.

Dodavanje novih lokacija: dodaj objekt oblika `{"name":"Naziv","lat":45.0,"lng":16.0,"region":"Regija"}` u JSON. Nasumični odabir uzima pet različitih zapisa, bez ponavljanja unutar igre.

## Pravila bodovanja

Udaljenost računa se Haversineovom formulom na srednjem polumjeru Zemlje od 6371 km. Bodovi su `round(5000 * exp(-distanceKm / 200))`, ograničeni na 0–5000 po rundi. Ukupan maksimum je 25.000.

## Objavljivanje

Može se objaviti kao statična stranica na GitHub Pages, Netlify ili Cloudflare Pages. Objavi sve datoteke iz direktorija projekta. Provjeri CDN i pružatelja pločica u ciljnom okruženju.

## Ograničenja i provjere

- Aplikacija nema backend ni online ljestvicu; najbolji rezultat pohranjuje se lokalno u pregledniku (trenutačno bez zasebnog prikaza rekorda).
- Provjera koda: potvrđena je konzistentnost strukture podataka, broj zapisa, jedinstvenost naziva, valjanost koordinata i izračun bodova kroz testove u `tests/test_data.py`. Ručno vizualno testiranje u stvarnom pregledniku i potvrda dostupnosti vanjskih tile/CDN poslužitelja nisu provedeni u ovom paketu; stvarno učitavanje ovisi o mreži i pravilima pružatelja.
