# Przestrzeń Kokoro — strona wizytówka

Statyczna, jednostronicowa witryna psychoterapeutki Kamili Mik. Czysty HTML + CSS
inline + jeden mały skrypt do animacji wejścia. Zero zależności, zero builda.

## Uruchomienie

Otwórz `index.html` w przeglądarce. Do pracy lokalnej wygodniej przez serwer:

```bash
python3 -m http.server 8000   # → http://localhost:8000
```

## Deploy na GitHub Pages

1. Wrzuć zawartość tego folderu do repo (`index.html` musi być w katalogu głównym
   lub w `/docs`).
2. Settings → Pages → Source: `main` / `/root` (lub `/docs`).

## Struktura

```
index.html        cała strona — jeden plik
assets/
  logo-mark.png       logo (głowa z sercem), PNG z alfą
  naglowek.jpg        grafika hero z pełnym lockupem logo
  pierwsza-fotka.jpg  portret w sekcji „Przed spotkaniem”
  przed-spotkaniem.jpg portret z książką w sekcji „O mnie”
  psychoterapia.jpg   zdjęcie sekcji psychoterapii
  kregi.jpg           zdjęcie sekcji Kobiecych Kręgów (też og:image)
  tex-stucco.png      kafelkowa faktura stiuku (tło sekcji)
```

## Sekcje (kolejność w pliku)

| id | Sekcja |
|---|---|
| — | `nav` — sticky, logo + linki + CTA „Kontakt” |
| `hero` | grafika nagłówka + dwa CTA |
| — | „Przed spotkaniem” — list do klientki + portret |
| `dlaczego` | Dlaczego istnieje Kokoro |
| `o-mnie` | O mnie — zdjęcie po lewej, tekst po prawej |
| `psychoterapia` | Psychoterapia Gestalt + panel „Jak pracuję” |
| `kregi` | Kobiece Kręgi „Cała Ja” — opis, zdjęcie, lista cyklu, „Dla kogo” |
| `mentoring` | Mentoring — ciemna sekcja, lista wsparcia + publikacje |
| `kontakt` | Kontakt — e-mail, telefon, CTA |
| — | `footer` — copyright + LinkedIn/Facebook |

## Design tokens

Kolory:

| Rola | Hex |
|---|---|
| tło bazowe (krem) | `#F9F2E7` |
| tło hero | `#F8F0E5` |
| tło ciasteczkowe (sekcje przemienne) | `#F1E6D4` |
| tło Kręgów (szałwia) | `#E9EEE0` |
| tło mentoringu (ciemny brąz) | `#46362A` |
| tekst główny | `#46362A` |
| tekst zwykły / akapity | `#6B5647` |
| tekst na ciemnym | `#D9C6AE` / `#F1E6D4` |
| akcent terakota (CTA, linki) | `#C0754B`, hover `#9F5A34` |
| akcent zieleń | `#7E9370`, obramowania `#9EB08E`, tekst `#6A7F5D` |
| akcent złoty | `#B98A2E` (PRZESTRZEŃ), `#D3AD83` (obramowania na ciemnym) |
| linie / obramowania | `#E9DCC6`, na ciemnym `#57452F` |

Typografia (Google Fonts):
- nagłówki i cytaty — **Cormorant Garamond** 500 / italic 400
- treść i UI — **Work Sans** 300 / 400 / 500
- skala nagłówków: `clamp(32px, 4vw, 44px)` (h2), 30px / 26px (h3), akapity 16–17px, `line-height 1.75–1.8`

Pozostałe:
- promienie: `12px` (karty, zdjęcia), `999px` (przyciski/pigułki), `200px 200px 12px 12px` (portret „Przed spotkaniem”)
- padding sekcji: `100px 6vw` (kontakt `110px 6vw`)
- easing animacji: `cubic-bezier(.22,.61,.36,1)`, czas `.85s`, stagger `90ms`

## Faktura tła

`assets/tex-stucco.png` to bezszwowa (kafelkowa) tekstura wenecko-stiukowa w
neutralnej szarości. Nakładana przez `background-blend-mode`:

- kremowe i ciasteczkowe sekcje: `overlay`, `background-size: 480px`
- zieleń Kręgów: `soft-light`, `760px` (subtelniej)
- ciemny brąz mentoringu: **bez faktury**
- sekcja kontakt: faktura nad gradientem — `background-image: url(...), linear-gradient(...)` + `background-blend-mode: overlay, normal`

Żeby zmienić intensywność, ruszaj tryb blendowania i `background-size` (większa
wartość = łagodniej).

## Animacja wejścia

Skrypt na końcu `index.html`: elementy poniżej pierwszego ekranu startują z
`opacity: 0; translateY(28px)` i odsłaniają się przy przewijaniu (kontenery
`display: grid` odsłaniają dzieci ze staggerem). Respektuje
`prefers-reduced-motion`. Failsafe po 20 s odsłania wszystko.

## Hover states

Stany hover są w `<style>` w `<head>` jako klasy `.h1`–`.h15` (generowane z
prototypu, deklaracje z `!important`). Element ma `class="hN"` i `data-fx="hN"`.
Przy większych zmianach warto to przepisać na nazwane klasy semantyczne
(`.btn-primary:hover` itd.).

## Dane kontaktowe

E-mail `przestrzenkokoro@gmail.com`, telefon `+48 609767667` — wpisane na sztywno
w sekcji `kontakt` (dwa linki `mailto:`/`tel:` + CTA). Zmiana = 3 miejsca.

## Do zrobienia / uwagi

- Google Ads / Analytics: w `<head>` jest zakomentowany blok `gtag` — wklej
  identyfikator (`AW-…` / `G-…`) i odkomentuj.
- Brak polityki prywatności i informacji RODO — wymagane, jeśli dojdzie formularz
  kontaktowy lub analityka.
- `og:image` wskazuje na `assets/kregi.jpg`; po wgraniu na domenę zmień na URL absolutny.
- Sekcja mentoringu w prototypie miała przełącznik widoczności — tu jest zawsze
  widoczna. Żeby ukryć, usuń `<section id="mentoring">`.
- Zdjęcia nie są zoptymalizowane (JPEG z telefonu). Warto przepuścić przez
  kompresję i dodać `loading="lazy"` poza pierwszym ekranem.
