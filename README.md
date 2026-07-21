# FPM — generator okładek magazynów

Jedna strona HTML, która robi z Twojego zdjęcia pierwszą stronę magazynu: winieta, nazwisko bohatera, hasła, kod kreskowy. **53 szablony w 10 grupach.** Wszystko renderuje się na `<canvas>` w przeglądarce — zdjęcia nie są nigdzie wysyłane, serwer podaje tylko plik statyczny.

## Co potrafi

- **Zdjęcie główne + dwa kadry poboczne** — przeciągnij plik albo kliknij pole.
- **Kadrowanie i obróbka lokalnie**: skala od 30% do 320% (a więc i pomniejszanie, nie tylko przybliżanie), przesunięcie w obu osiach, jasność, kontrast, nasycenie, ziarno druku. Przycisk „Zmieść całe zdjęcie" liczy potrzebną skalę z proporcji zdjęcia i kadru w danym szablonie. Margines, który zostaje wokół pomniejszonego zdjęcia, wypełnia rozmyte powiększenie tego samego kadru albo jednolity kolor.
- **Pole na tytuł magazynu** plus dopisek, numer, data i cena.
- **Do 10 haseł** — dodajesz, usuwasz, przestawiasz i edytujesz. Licznik pilnuje limitu, a szablon bierze tyle, ile mieści się bez ścisku, zaczynając od pierwszego.
- **Edycja wprost na okładce** — kliknij dowolny napis albo kadr, żeby go zaznaczyć. Przeciąganie przesuwa, kwadrat w rogu skaluje, kółko myszy też skaluje, strzałki przesuwają co 2 px (z Shiftem co 10). Dla tekstu dochodzi wybór kroju i koloru. Kliknięcie w puste miejsce odznacza i przechodzi w tryb przesuwania zdjęcia głównego.
- **Wycinanie tła osoby (AI, lokalnie)** — jedno kliknięcie oddziela postać od tła. Model MediaPipe Selfie Segmenter działa w przeglądarce przez WASM/GPU, zdjęcie nigdzie nie jest wysyłane; z sieci pobiera się tylko sam model (~2 MB, raz). Tło można zostawić rozmyte, zamienić na kolor przewodni, na jasne albo usunąć zupełnie. Suwak wygładza krawędzie. PNG z gotową przezroczystością jest rozpoznawany od razu, bez modelu i bez internetu.
- **Tytuł za osobą** — po wycięciu winieta ląduje między tłem a postacią, tak jak na prawdziwych okładkach.
- **Żartobliwe teksty domyślne** — startowa okładka to „Legenda Osiedla" z redakcyjnym śledztwem w sprawie drugiej skarpetki. Wbudowany kreator też pisze z przymrużeniem oka.
- **Opis → gotowa okładka**: wpisujesz kilka zdań o osobie, dostajesz komplet tytułu, nazwiska, podpisu i dziesięciu haseł. Wbudowany kreator działa bez internetu; opcjonalnie można podłączyć Claude API własnym kluczem.
- **69 szablonów** w grupach: Wysoka moda, Glossy, Gazeta, Tabloid, Moda, Biznes, Plakat, Ramka, Pas, Siatka, Zin, Kino.
- **10 motywów zdobniczych** nakładanych na dowolny układ: konfetti, art déco, raster drukarski, ukośne paski, taśma klejąca, pieczęć z datą, wieniec laurowy, gwiazdki, siatka techniczna. Do tego suwak siły motywu. Dziesięć kolorów przewodnich plus duoton. Przewijanie strzałkami ← →.
- **Filtr grup zamiast wyszukiwarki** — dziesięć kafelków grup wystarcza, żeby ogarnąć 53 pozycje. Każdy szablon ma tagi z okazjami (`GROUP_TAGS` + `EXTRA_TAGS`), które podpowiadają w dymku kafelka i w opisie pod galerią, na co się nadaje.
- **Cofanie i ponawianie** — `Ctrl/Cmd + Z` oraz `Ctrl/Cmd + Shift + Z`, plus przyciski ↺ ↻ w belce dla dotyku. Historia trzyma 60 kroków: teksty, ustawienia, wybrany szablon i wszystkie przesunięcia elementów. Zdjęcia zostają poza migawką, więc cofanie nie kasuje wgranego pliku.
- **Podgląd w oprawie** (przycisk „Podgląd w ramce" w górnej belce) — ta sama okładka pokazana tak, jak trafi do obdarowanego: w ramce na ścianie, w ramce na półce albo jako magazyn leżący na blacie. Cztery kolory ramy, passe-partout, odbicie w szkle i cień. Osobny plik PNG do wysłania komuś przed drukiem.
- **Eksport**: PNG, JPG oraz PNG 2× (2480 × 3496 px, czyli A4 przy ~300 dpi). Ramka zaznaczenia nigdy nie trafia do pliku. Skrót `Ctrl/Cmd + S`.

## Uruchomienie

Otwórz `index.html` w przeglądarce. Nie ma buildu, nie ma zależności poza fontami z Google Fonts.

Lokalnie z serwerem (przydatne przy testach):

```bash
python3 -m http.server 8000
```

## Publikacja na GitHub Pages — gdzie jest adres strony

GitHub sam z siebie nie hostuje strony. Trzeba jednorazowo włączyć Pages, a adres pojawia się dopiero potem.

**1. Wrzuć pliki do repozytorium.** Najprościej przez przeglądarkę: na stronie repozytorium **Add file → Upload files**, przeciągasz `index.html`, `README.md`, `LICENSE` i `.nojekyll`, na dole **Commit changes**.

**2. Włącz Pages.** W repozytorium: zakładka **Settings** (na górze, nie w profilu) → w lewym menu **Pages** → sekcja *Build and deployment* → *Source*: **Deploy from a branch** → *Branch*: **main**, folder **/ (root)** → **Save**.

**3. Poczekaj minutę i odśwież stronę Settings → Pages.** Nad ustawieniami pojawi się zielona ramka z adresem:

```
https://TWOJA-NAZWA.github.io/NAZWA-REPOZYTORIUM/
```

Postęp wdrożenia widać w zakładce **Actions** — zielony znaczek oznacza, że strona już działa. Adres jest też potem widoczny na stronie głównej repozytorium, po prawej stronie w sekcji *About* (trzeba raz kliknąć kółko zębate i zaznaczyć *Use your GitHub Pages website*).

### Najczęstsze powody, że adresu nie ma albo strona pokazuje 404

| Objaw | Przyczyna | Co zrobić |
|---|---|---|
| W Settings nie ma pozycji **Pages** | repozytorium jest prywatne na darmowym koncie | ustaw je jako publiczne: Settings → General → na samym dole *Change visibility* |
| Adres jest, ale wyświetla się 404 | `index.html` leży w podfolderze, np. `fpm-cover-studio/index.html` | albo przenieś plik do korzenia repozytorium, albo wejdź na `https://user.github.io/repo/fpm-cover-studio/` |
| Adres jest, ale widać treść README | Pages działa, tylko brakuje `index.html` w korzeniu | dodaj `index.html` do korzenia |
| Strona się nie odświeża po zmianie | pamięć podręczna przeglądarki | odśwież z pominięciem cache: Ctrl+F5 (Windows) albo Cmd+Shift+R (Mac) |
| Wdrożenie wisi kilka minut | pierwsze uruchomienie potrafi trwać 2–3 minuty | sprawdź zakładkę Actions |

Uwaga: adres nie zaczyna się od `www`. GitHub Pages zawsze daje `nazwa-uzytkownika.github.io/nazwa-repozytorium/`. Własną domenę z `www` podpina się osobno w Settings → Pages → *Custom domain*.

## Klucz API — uwaga

Tryb „Przez Claude API" wywołuje `api.anthropic.com` prosto z przeglądarki, więc klucz jest widoczny dla osoby, która go wpisuje. Trzyma się tylko w pamięci karty i znika po odświeżeniu. Na publicznej stronie nigdy nie wpisuj do kodu własnego klucza — albo zostaw wbudowany kreator, albo postaw mały serwer pośredniczący i podmień adres w funkcji `generateApi`.

## Polskie znaki w krojach

Aplikacja nie ufa deklaracjom dostawcy fontów. Przy starcie, już po załadowaniu krojów, uruchamia test: mierzy szerokość napisu `ĄĆĘŁŃÓŚŹŻąćęłńóśźż` dwa razy — raz z fallbackiem `monospace`, raz z `serif`. Jeśli krój ma komplet znaków, obie szerokości są identyczne. Jeśli choć jednego brakuje, przeglądarka podstawia znak z fontu zastępczego i szerokości się rozjeżdżają.

Krój, który nie przejdzie testu, znika z listy wyboru, a szablony, które go używały, dostają automatycznie zamiennik z tej samej rodziny stylistycznej (`FONT_SWAP`). Wynik testu widać w pasku edycji pod płótnem.

Dodając nowy krój, dopisz go do `ALL_FONTS`, do linku Google Fonts i do `FONT_SWAP` — reszta zadzieje się sama.

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
| `credits` | plakat filmowy z blokiem twórców złożonym ze zwężonych wersalików |
| `moda` | okładka modowa: didone przez całą szerokość, hasła w dwóch łamach, temat okładkowy kursywą |
| `editorial` | numer kolekcjonerski: winieta, dużo powietrza, jedno albo dwa hasła |
| `runway` | zdjęcie w białej ramie, winieta wchodząca na jego krawędź, hasła w pasie pod spodem |

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

`makeQueue()` obsługuje zmienną liczbę haseł: archetyp pobiera z kolejki tyle, ile zmieści, a `column()` pilnuje, żeby nic nie wyszło poza wyznaczony obszar.

## Jak działa „tytuł za osobą"

Po wycięciu tła zdjęcie główne rozpada się na dwie warstwy. `mainPhoto()` rysuje tylko tło i odkłada pierwszy plan do `PENDING_FG`. Funkcja `E()`, która opakowuje każdy ruchomy element, po narysowaniu elementu o identyfikatorze `tytuł` wywołuje `drawPendingFg()` — postać ląduje na winiecie, reszta tekstów rysuje się już po niej, czyli na wierzchu. Gdyby jakiś układ nie rysował winiety, `render()` domyka warstwę na końcu.

Odkładanie działa tylko wtedy, gdy winieta faktycznie ma się dopiero narysować. W układach gazetowych tytuł idzie na samej górze, przed zdjęciem — flaga `MAST_DONE` wykrywa ten przypadek i rysuje postać od razu, zamiast wypychać ją na sam wierzch okładki, na podpisy pod zdjęciem.

Wyjątek to kolaż w grupie Zin: zdjęcie jest tam obrócone we własnym układzie współrzędnych, więc pierwszy plan rysuje się od razu (`mainPhoto(..., false)`), a tytuł zostaje na wierzchu.

### Biegunowość maski

Model zwraca maskę klas, ale numeracja bywa różna w zależności od wersji — raz tło to `0`, raz `255`. Zamiast przyjmować jedną konwencję, `classifyMask()` porównuje dwa punkty odniesienia: ramkę kadru (górna krawędź i boki, gdzie prawie zawsze jest tło) oraz środkowy słupek (gdzie prawie zawsze stoi człowiek). Klasa z ramki to tło, wszystko inne to pierwszy plan.

Gdy obie strony wskazują tę samą klasę, model nic sensownego nie znalazł — wtedy aplikacja odmawia i mówi to wprost, zamiast wyciąć losową połowę zdjęcia.

## Typografia okładek modowych

Grupa „Wysoka moda" odtwarza konwencje, nie konkretne tytuły — winiety znanych magazynów są znakami towarowymi i nie ma ich w projekcie.

Trzy rzeczy budują ten charakter. **Didone** o wysokim kontraście grubości kresek: Bodoni Moda, Prata i Marcellus, dobrane tak, by miały komplet polskich znaków. **Szerokie rozstrzelenie wersalików** w podpisach i dopiskach, często 6–18 px. Oraz **rytm haseł**: `splitLead()` dzieli każde hasło na wejście i resztę — po dwukropku, myślniku, a gdy ich nie ma, po drugim słowie. Wejście idzie grubo, reszta lekko. To ta jedna rzecz najmocniej odróżnia okładkę modową od zwykłej listy tematów.

Do tego geometryczny grotesk Jost jako przeciwwaga dla szeryfów, ustawiany osobno w polu `sans` każdego szablonu.

## Motywy

Motyw to warstwa zdobnicza rysowana już na gotowym układzie, w `drawMotif()`, tuż przed ziarnem druku. Dzięki temu nie musi nic wiedzieć o archetypie i działa z każdym szablonem — 69 układów razy 10 motywów daje znacznie więcej kombinacji niż sama lista szablonów.

Wszystkie motywy rysują się kolorami z aktywnej palety (`P.a`, `P.b`), więc nigdy nie kłócą się z resztą okładki, a suwak siły steruje przezroczystością. Szablon może mieć motyw domyślny — pole `motif` w jego konfiguracji.

Nowy motyw to jedna gałąź w `drawMotif()` plus wpis w tablicy `MOTIFS`.

## Historia zmian

Migawka to zwykły JSON ze stanem: teksty, ustawienia obrazu, wybrany szablon, paleta i mapa `OVR` z przesunięciami elementów. Zdjęcia są poza nią celowo — cofnięcie ma odkręcić literówkę albo nieudane przeciągnięcie, a nie wyrzucić wgrany plik.

Zapisy trafiają na stos w dwóch trybach: natychmiast po zakończonej czynności (zmiana szablonu, puszczenie przeciąganego elementu, dodanie hasła) i z opóźnieniem 450 ms przy pisaniu oraz ruchu suwaków, żeby jedno zdanie nie zamieniło się w czterdzieści kroków historii. Zmiana po cofnięciu odcina gałąź „do przodu", tak jak w każdym edytorze.

## Warstwa edycji bezpośredniej

Każdy ruchomy element jest opakowany w `E(id, kotwicaX, kotwicaY, fn)`. Ta funkcja:

1. nakłada zapisane przesunięcie, skalę, krój i kolor (`OVR[id]`),
2. przechwytuje wszystko, co `fn()` narysuje, i liczy z tego obrys w układzie ekranu — dzięki temu trafienie kursorem działa też dla tekstu obróconego o 90° albo przekrzywionej naklejki,
3. dopisuje obrys do tablicy `HIT`, po której idzie test trafienia od wierzchu w dół.

Nowy element robisz przez opakowanie rysowania:

```js
E('mój-napis', x, y, () => text('Cokolwiek', {x, y, size:40, family:'Anton', color:'#fff'}));
```

Przesunięcia kasują się przy zmianie szablonu — inaczej element ustawiony pod jeden układ lądowałby w losowym miejscu następnego. Przycisk „Cofnij przesunięcia" czyści wszystko ręcznie.

## Licencja

MIT. Zdjęcia i teksty, które wgrywasz, pozostają Twoje.
