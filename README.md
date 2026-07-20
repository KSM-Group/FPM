# FPM — generator okładek magazynów

Jedna strona HTML, która robi z Twojego zdjęcia pierwszą stronę magazynu: winieta, nazwisko bohatera, hasła, kod kreskowy. **53 szablony w 10 grupach.** Wszystko renderuje się na `<canvas>` w przeglądarce — zdjęcia nie są nigdzie wysyłane, serwer podaje tylko plik statyczny.

## Co potrafi

- **Zdjęcie główne + dwa kadry poboczne** — przeciągnij plik albo kliknij pole.
- **Kadrowanie i obróbka lokalnie**: powiększenie, przesunięcie w obu osiach, jasność, kontrast, nasycenie, ziarno druku.
- **Pole na tytuł magazynu** plus dopisek, numer, data i cena.
- **Dowolna liczba haseł** — dodajesz, usuwasz i przestawiasz je przyciskami. Szablon bierze tyle, ile mieści się bez ścisku, zaczynając od pierwszego.
- **Opis → gotowa okładka**: wpisujesz kilka zdań o osobie, dostajesz komplet tytułu, nazwiska, podpisu i dziesięciu haseł. Wbudowany kreator działa bez internetu; opcjonalnie można podłączyć Claude API własnym kluczem.
- **53 szablony** w grupach: Glossy, Gazeta, Tabloid, Moda, Biznes, Plakat, Ramka, Pas, Siatka, Zin. Wyszukiwarka, filtry i przewijanie strzałkami ← →. Dziesięć kolorów przewodnich plus duoton.
- **Eksport**: PNG, JPG oraz PNG 2× (2480 × 3496 px, czyli A4 przy ~300 dpi). Skrót `Ctrl/Cmd + S`.

## Uruchomienie

Otwórz `index.html` w przeglądarce. Nie ma buildu, nie ma zależności poza fontami z Google Fonts.

Lokalnie z serwerem (przydatne przy testach):

```bash
python3 -m http.server 8000
```

## Publikacja na GitHub Pages

```bash
git init
git add .
git commit -m "FPM: generator okładek"
git branch -M main
git remote add origin https://github.com/UZYTKOWNIK/fpm-cover-studio.git
git push -u origin main
```

Potem w repozytorium: **Settings → Pages → Source: Deploy from a branch → main / (root)**. Strona pojawi się pod `https://UZYTKOWNIK.github.io/fpm-cover-studio/`. Plik `.nojekyll` jest w repo, żeby Pages nie przetwarzało niczego po drodze.

## Klucz API — uwaga

Tryb „Przez Claude API" wywołuje `api.anthropic.com` prosto z przeglądarki, więc klucz jest widoczny dla osoby, która go wpisuje. Trzyma się tylko w pamięci karty i znika po odświeżeniu. Na publicznej stronie nigdy nie wpisuj do kodu własnego klucza — albo zostaw wbudowany kreator, albo postaw mały serwer pośredniczący i podmień adres w funkcji `generateApi`.

## Fonty i wzorce

| Rola | Font | Gdzie |
|---|---|---|
| Winieta glossy i gazetowa | Playfair Display | Splendor, Dziennik, Kapitał |
| Winieta tabloidowa, nazwiska | Anton | Impakt, Kapitał |
| Hasła, podpisy | Oswald | wszystkie |
| Winieta modowa, nazwiska | Cormorant Garamond | Kontur |
| Tekst pomocniczy | Inter | wszystkie |

## Jak to jest zbudowane

Szablony nie są osobnymi plikami. Jest **10 archetypów układu** — funkcji rysujących — a każdy szablon to zestaw parametrów podanych temu archetypowi:

| Archetyp | Co robi |
|---|---|
| `fullbleed` | zdjęcie na całą stronę, hasła w dwóch kolumnach |
| `press` | pierwsza strona gazety: winieta, szpalta, zdjęcie w kolumnie |
| `tabloid` | pas u góry, hasła na kolorowych podkładach, okrągła plakietka |
| `minimal` | papier, wklejone zdjęcie, cienka winieta, pionowe podpisy |
| `split` | zdjęcie na jednej połowie, tekst i duża liczba na drugiej |
| `poster` | gigantyczna winieta na zdjęciu, pas haseł u dołu |
| `frame` | zdjęcie w passe-partout, nazwisko na belce |
| `band` | pionowy pas z tytułem obróconym o 90° |
| `grid` | zdjęcie u góry, kolorowy blok z trzema szpaltami niżej |
| `zine` | kolaż: przekrzywione zdjęcie i naklejki z hasłami |

Nowy szablon to jedna linijka w tablicy `TEMPLATES`:

```js
T('moj-01','Mój szablon','Glossy','fullbleed',{
  mf:'Abril Fatface', mw:400, mls:0,   // winieta: krój, grubość, światło
  lf:'Oswald', lw:500, lu:true,        // hasła
  nf:'Abril Fatface', nw:400,          // nazwisko
  thumbs:true, barcode:true, pal:6     // dodatki i domyślny kolor
})
```

Własny archetyp: napisz `function archMoj(sp){…}` (płótno 1240 × 1748) i dopisz go do mapy `ARCH`. Do dyspozycji masz `text()`, `fitSize()`, `fitBlock()`, `column()`, `rule()`, `photo()`, `mainPhoto()`, `scrimTop()`, `scrimBottom()`, `barcode()`, `makeQueue()` oraz `PALETTES[S.pal]`.

`makeQueue()` jest kluczowe dla dowolnej liczby haseł: archetyp pobiera z kolejki tyle, ile zmieści, a `column()` sam pilnuje, żeby nic nie wyszło poza wyznaczony obszar.

## Licencja

MIT. Zdjęcia i teksty, które wgrywasz, pozostają Twoje.
