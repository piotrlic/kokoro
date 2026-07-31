# Przestrzeń Kokoro — strona wizytówka

Statyczna, jednostronicowa witryna psychoterapeutki Kamili Mik. Czysty HTML,
style inline + jeden mały skrypt animacji wejścia. Zero zależności, zero builda.

## Uruchomienie

Otwórz `index.html` w przeglądarce. Lokalnie wygodniej przez serwer:

```bash
python3 -m http.server 8000   # → http://localhost:8000
```

## Deploy na GitHub Pages

1. Wrzuć zawartość tego folderu do repo (`index.html` w katalogu głównym lub `/docs`).
2. Settings → Pages → Source: `main` / `/root` (lub `/docs`).

## Struktura

```
index.html        cała strona — jeden plik
assets/
  logo-mark.png        logo (głowa z sercem), PNG z alfą
  naglowek.jpg         grafika hero z pełnym lockupem logo
  pierwsza-fotka.jpg   portret w sekcji „Przed spotkaniem”
  przed-spotkaniem.jpg portret z książką w sekcji „O mnie”
  psychoterapia.jpg    zdjęcie psychoterapii (sekcja + kafelek)
  mentoring.jpg        zdjęcie kafelka mentoringu
  kregi.jpg            zdjęcie Kobiecych Kręgów (też og:image)
```

## Sekcje (kolejność w pliku)

| id | Sekcja |
|---|---|
| — | `nav` — sticky, logo + linki + CTA „Kontakt” |
| `hero` | grafika nagłówka + dwa CTA |
| `przed-spotkaniem` | list do klientki + portret |
| `dlaczego` | Dlaczego istnieje Kokoro |
| `obszary` | trzy kafelki: Psychoterapia / Mentoring / Kręgi |
| `o-mnie` | O mnie — zdjęcie po lewej, tekst po prawej |
| `psychoterapia` | Psychoterapia Gestalt + panel „Jak pracuję” |
| `kregi` | Kobiece Kręgi „Cała Ja” — opis, zdjęcie, cykl (7 spotkań), „Dla kogo” |
| `mentoring` | Mentoring — lista wsparcia + publikacje |
| `kontakt` | Kontakt — e-mail, telefon (szałwiowe tło) |
| — | `footer` — copyright + nawigacja + LinkedIn/Facebook |

## Design tokens

Kierunek: **płaskie, jednolite tła, zero zaokrągleń, zero gradientów i faktur.**
Paleta chłodnego kamienia i szałwii.

| Rola | Hex |
|---|---|
| tło bazowe (krem) | `#FAF6EF` |
| tło hero | `#F9F0EB` (dopasowane do tła grafiki nagłówka) |
| tło sekcji przemiennych | `#EFE9DD` |
| tło Kręgów i Kontaktu (szałwia) | `#E4E8DB` |
| tekst główny | `#33322D` |
| tekst akapitów | `#5C564C` |
| akcent główny (CTA, linki) | `#7E8C6A`, hover `#657351` |
| akcent piaskowy (nadtytuły) | `#8A7B5F` |
| obramowania / linie | `#C7BEAC`, `#E2DACB`, hover-tło `#E6DECF` |

Typografia (Google Fonts):
- nagłówki i cytaty — **Cormorant Garamond** 500 / italic 400
- treść i UI — **Work Sans** 300 / 400 / 500
- h2 `clamp(32px, 4vw, 44px)`, h3 30 / 26px, akapity 16–17px, `line-height 1.75–1.8`
- akapity i listy justowane (`text-align: justify; hyphens: auto`, `lang="pl"`)
  — na telefonach justowanie wyłączone, bo w wąskiej kolumnie robi rzeki

Pozostałe:
- `border-radius: 0` w całym projekcie — świadoma decyzja, nie dodawaj zaokrągleń
- padding sekcji: `100px 6vw` (kontakt / obszary `110px 6vw`), na telefonach `64px 5vw`
- easing animacji: `cubic-bezier(.22,.61,.36,1)`, czas `.85s`, stagger `90ms`

## Responsywność

Style są inline, więc media queries w `<head>` nadpisują je przez `!important`.
Trzy progi:

| Próg | Co się zmienia |
|---|---|
| `≤ 900px` | nawigacja łamie się na dwa rzędy: logo + CTA „Kontakt”, linki wyśrodkowane pod spodem (`.nav-logo` / `.nav-cta` / `.nav-links` + `order`) |
| `≤ 760px` | sekcje `64px 5vw`, siatki do jednej kolumny, koniec justowania, stopka wyśrodkowana, w nawigacji znika „ Gestalt” (`.nav-long`) |
| `≤ 380px` | mniejsze logo, CTA i linki, żeby logo i „Kontakt” zmieściły się w jednym rzędzie |

Uwagi:
- Każda siatka wielokolumnowa ma `class="cols"` — to jej punkt zaczepienia
  do składania w jedną kolumnę. Dodajesz nową siatkę → dodaj tę klasę.
- `#przed-spotkaniem` ma sztywne dwie kolumny (`minmax(300px,560px) minmax(260px,380px)`),
  potrzebuje ~590px + marginesy — bez składania wychodzi poza ekran telefonu.
- `scroll-padding-top` (88px / 104px) trzyma nagłówki sekcji spod sticky nawigacji.
- Sprawdzone od 320px do 1440px — bez poziomego przewijania.

## Animacja wejścia

Skrypt na końcu `index.html`: elementy poniżej pierwszego ekranu startują z
`opacity: 0; translateY(28px)` i odsłaniają się przy przewijaniu (kontenery
`display: grid` odsłaniają dzieci ze staggerem). Respektuje
`prefers-reduced-motion`. Failsafe po 20 s odsłania wszystko.

## Hover states

Stany hover są w `<style>` w `<head>` jako klasy `.h1`–`.h17` (generowane,
deklaracje z `!important`). Element ma `class="hN"`. Przy większych zmianach
warto przepisać na nazwy semantyczne (`.btn-primary:hover` itd.).

## Dane kontaktowe

E-mail `przestrzenkokoro@gmail.com`, telefon `+48 609 767 667` — na sztywno w
sekcji `kontakt` (linki `mailto:` / `tel:`) oraz w CTA mentoringu (`mailto:`).

## Do zrobienia / uwagi

- Google Ads / Analytics: w `<head>` jest zakomentowany blok `gtag` — wklej
  identyfikator (`AW-…` / `G-…`) i odkomentuj.
- Brak polityki prywatności i informacji RODO — wymagane, jeśli dojdzie formularz
  kontaktowy lub analityka.
- `og:image` wskazuje na `assets/kregi.jpg`; po wgraniu na domenę zmień na URL absolutny.
- Zdjęcia nie są zoptymalizowane (JPEG z telefonu). Warto skompresować i dodać
  `loading="lazy"` poza pierwszym ekranem.
