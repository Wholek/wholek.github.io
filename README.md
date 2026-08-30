# Strona kamilgacek — projekt Quarto

Statyczna strona-wizytówka zbudowana w Quarto. Jeden wąski blok tekstu, górna belka
nawigacji, paleta oliwkowo-złota przeniesiona z Twojego CV.

## 1. Uruchomienie w RStudio

> **Uwaga o pliku `.Rproj`.** Paczka celowo go nie zawiera. RStudio rozpoznaje projekt
> Quarto po obecności `_quarto.yml` i samo pokazuje zakładkę Build. Jeśli w Twoim
> `.Rproj` znajdzie się kiedykolwiek linia `BuildType: Website`, przycisk Build będzie
> wywoływał `rmarkdown::render_site()` zamiast Quarto i skończy się błędem
> „No site generator found". Wtedy wystarczy tę jedną linię skasować.


1. Rozpakuj folder w dowolnym miejscu na dysku.
2. RStudio → *File* → *New Project* → *Existing Directory* → wskaż ten folder.
   (RStudio utworzy własny plik `.Rproj` — to ten właściwy, nie nadpisuję go.)
3. W zakładce **Build** kliknij **Render Website**, albo w terminalu:

```bash
quarto preview      # podgląd na żywo, odświeża się przy zapisie pliku
quarto render       # pełne zbudowanie strony do katalogu docs/
```

Gotowy serwis ląduje w `docs/`. Tego katalogu nie edytujesz ręcznie — jest generowany.

## 2. Co gdzie leży

| Plik | Zawartość |
|---|---|
| `_quarto.yml` | konfiguracja: nazwa strony, menu, stopka, motyw |
| `index.qmd` | About — hero, bio, doświadczenie akademickie, wykształcenie |
| `research.qmd` | agenda badawcza i projekty |
| `publications.qmd` | publikacje z abstraktem, słowami kluczowymi i DOI |
| `conferences.qmd` | konferencje, warsztaty, szkoły letnie, wyjazdy badawcze + mapa |
| `teaching.qmd` | prowadzone przedmioty i informacje dla studentów |
| `grants.qmd` | granty i projekty badawcze |
| `other.qmd` | stypendia, członkostwa, działalność akademicka |
| `industry.qmd` | ścieżka poza uczelnią (Jacobs, Aptiv) |
| `cv.qmd` | pobranie CV |
| `contact.qmd` | kontakt i profile |
| `styles.scss` | cała warstwa wizualna: kolory, fonty, układ |
| `header.html` | fonty Google + miejsce na kod analityki |
| `files/` | CV w PDF i inne pliki do pobrania |
| `images/` | zdjęcie profilowe i favikona |

## 3. Do uzupełnienia (`TODO` w plikach)

Wyszukaj w projekcie frazę `TODO` — znajdziesz wszystkie miejsca do wypełnienia:

- **zdjęcie** — podmień `images/profile.jpg` (proporcja ok. 4:5, min. 600 px szerokości);
- **linki do profili** — ORCID, Google Scholar, ResearchGate, LinkedIn w `index.qmd` i `contact.qmd`
  (obecnie prowadzą do `#`);
- **abstrakty** — w `publications.qmd` są robocze streszczenia; wklej oficjalne abstrakty;
- **industry.qmd** — punkty przy każdej roli;
- **adres wydziału** w `contact.qmd` — zweryfikuj.

## 4. Zmiana wyglądu

Wszystko jest w `styles.scss`, na samej górze pliku:

- **kolor akcentu** — podmień `$gold` i `$gold-deep`;
- **fonty** — trzy gotowe warianty w bloku „TYPOGRAFIA". Aktywny jest wariant A
  (nagłówki szeryfowe + tekst bezszeryfowy). Żeby przełączyć, zakomentuj blok A
  i odkomentuj B albo C. Fonty są już wczytane, nie trzeba nic dogrywać;
- **szerokość kolumny** — `main.content { max-width: 780px }`.

Nagłówki sekcji (wersaliki, rozstrzelone, złote, z cienką linią pod spodem) celowo
powtarzają układ Twojego CV — strona i dokumenty wyglądają wtedy jak jedna rodzina.

## 5. Dodanie nowej publikacji

W `publications.qmd` skopiuj cały blok `::: {.pub} ... :::` i podmień treść. Kolejność jest
ręczna — nowe wpisy na górę.

## 6. Analityka odwiedzin

W `header.html` są trzy przygotowane opcje. Polecam **GoatCounter**: darmowy dla stron
prywatnych, nie używa ciasteczek, więc nie potrzebujesz baneru zgody. Rejestrujesz się na
goatcounter.com, dostajesz swój kod, wklejasz go do `header.html` i odkomentowujesz blok.

Google Analytics działa, ale wymaga baneru zgody (RODO) — w Quarto włącza się go przez
`cookie-consent` w `_quarto.yml`.

## 7. Publikacja

Model pracy: piszesz i renderujesz lokalnie, a katalog `docs/` wypychasz do GitHuba. Hosting
serwuje po prostu ten katalog.

```bash
git init
git add .
git commit -m "Initial site"
git branch -M main
git remote add origin https://github.com/UZYTKOWNIK/REPOZYTORIUM.git
git push -u origin main
```

Następnie w repozytorium: **Settings → Pages → Source: Deploy from a branch →
`main` / `/docs`**. Po minucie strona jest pod `https://uzytkownik.github.io/repozytorium/`.

### Własna domena

Domenę kupujesz osobno (OVH, nazwa.pl, Cloudflare Registrar — rząd 50–120 zł rocznie za
`.com` lub `.pl`). Sam hosting zostaje darmowy:

1. w repozytorium: **Settings → Pages → Custom domain** → wpisz `kamilgacek.com`;
2. u rejestratora domeny ustaw rekordy DNS:
   - `A` → `185.199.108.153`, `185.199.109.153`, `185.199.110.153`, `185.199.111.153`
   - `CNAME` dla `www` → `uzytkownik.github.io`
3. zaznacz **Enforce HTTPS** (certyfikat GitHub wystawia sam, za darmo);
4. w `_quarto.yml` odkomentuj `site-url` i wpisz adres — potrzebne do mapy strony i
   poprawnych metadanych przy udostępnianiu linku.

Alternatywa: **Netlify** — przeciągasz katalog `docs/` na stronę, dostajesz adres, własną
domenę podpinasz w panelu. Też darmowe w tej skali.

## 8. Aktualizacja treści

Edytujesz `.qmd` → `quarto render` → `git add . && git commit -m "update" && git push`.
Zmiana jest widoczna po chwili.

## 9. Plik `.nojekyll`

W katalogu projektu leży pusty plik `.nojekyll`, kopiowany przy renderowaniu do `docs/`.
GitHub Pages domyślnie przepuszcza strony przez Jekylla, który ignoruje katalogi
zaczynające się od podkreślnika. Ten plik to wyłącza. Bez niego część stylów i skryptów
może się nie załadować po publikacji. Nie kasuj go.

## 10. Plik `CNAME`

W katalogu projektu leży plik `CNAME` z jedną linią: nazwą Twojej domeny. Przy renderowaniu
trafia do `docs/` i to on mówi GitHub Pages, pod jakim adresem ma serwować stronę.

Bez niego wygląda to tak: ustawiasz domenę w Settings → Pages, GitHub tworzy ten plik sam,
a przy najbliższym `quarto render` katalog `docs/` jest odtwarzany i plik znika. Domena
przestaje działać, na pozór bez powodu. Trzymanie `CNAME` w źródłach projektu zamyka
ten problem raz na zawsze.

Zmieniasz domenę? Popraw zawartość tego pliku i `site-url` w `_quarto.yml`.
