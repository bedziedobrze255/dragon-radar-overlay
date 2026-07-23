# Smoczy Radar + Pasek Donate — nakładka na stream (wersja mobilna)

Dwa niezależne widgety w klimacie Dragon Balla:

- **`dragon-radar.html`** — "Smoczy Radar": pomarańczowo-biała obudowa z kolcami (jak w anime), czarny bezel, zielony ekran z **prawdziwą mapą z GPS telefonu** (OpenStreetMap) w skanerowym stylu, siatka, gwiazdki, czerwone znaczniki, żółta igła (podąża za kompasem telefonu, jeśli jest dostępny), oraz licznik **dystansu w km liczony naprawdę z ruchu GPS**, plus data i godzina.
- **`donate-bar.html`** — pasek z 7 **prawdziwymi smoczymi kulami** (SVG z properami układami gwiazdek 1–7, nie generyczne gwiazdki unicode), cel **możliwy do ustawienia** (domyślnie Laptop — 500 zł), plakietka `kick.com/krakowiakv2`, napis "ZBIERAM SMOCZE KULE, WESPRZYJ!", specjalny efekt po osiągnięciu celu.
- **`preview.html`** — tylko do podglądu obu widgetów naraz w przeglądarce na komputerze (nieużywane w telefonie/IRL PRO).

## WAŻNE: dlaczego to nie jest link z claude.ai

Strony publikowane bezpośrednio przeze mnie (Artifacts) mają zablokowany dostęp do internetu (nie mogą pobrać mapy, ani danych z Tipply). Żeby radar naprawdę pokazywał mapę i GPS, oraz żeby dało się to dodać jako URL w IRL PRO/Streamlabs, pliki muszą wisieć pod zwykłym adresem WWW. Najprostszy sposób **z samego telefonu, bez komputera** — GitHub Pages (darmowe, stabilne, działa wszędzie):

### Wystawienie plików online (tylko telefon, ~5 minut)

1. Wejdź w przeglądarce telefonu na **github.com**, załóż darmowe konto (jeśli nie masz).
2. Kliknij **+ → New repository**. Nazwa np. `dragon-radar-overlay`, zaznacz **Public**, kliknij **Create repository**.
3. W nowym repo kliknij **Add file → Upload files** i wgraj z telefonu pliki: `dragon-radar.html`, `donate-bar.html` (plik README/preview nie są potrzebne na streamie).
4. Kliknij **Commit changes**.
5. Wejdź w **Settings → Pages**. Przy "Build and deployment" wybierz **Deploy from a branch**, branch: **main**, folder: **/ (root)** → **Save**.
6. Po ok. 1 minucie adres widgetów będzie dostępny pod:
   - `https://TWOJANAZWA.github.io/dragon-radar-overlay/dragon-radar.html`
   - `https://TWOJANAZWA.github.io/dragon-radar-overlay/donate-bar.html`

(Jeśli kiedyś będziesz chciał coś zmienić w pliku — w repo na GitHubie otwórz plik, kliknij ✏️ (edytuj), zmień, **Commit changes**. Strona zaktualizuje się automatycznie.)

## Dodanie w IRL PRO / Streamlabs (telefon)

**IRL PRO:** Overlays → Web Overlays → New web overlay → wklej adres URL widgetu → ustaw pozycję i rozmiar (radar np. w prawym dolnym rogu, pasek donate na dole na środku).

**Streamlabs (mobile):** dodaj źródło typu Widget/Browser i wklej ten sam URL.

Tło obu stron jest przezroczyste, więc nie trzeba nic więcej ustawiać (bez chroma key).

## Smoczy Radar — jak działa GPS

Przy pierwszym otwarciu strona poprosi o **zgodę na dostęp do lokalizacji** — trzeba ją zaakceptować, żeby mapa i licznik km działały naprawdę.

⚠️ Uwaga: nie każda aplikacja pozwala swojemu wbudowanemu oknu przeglądarki pokazać to okienko z pytaniem o zgodę. Jeśli po dodaniu widgetu mapa stoi w miejscu (Kraków) i status na ekranie pokazuje "BRAK ZGODY NA GPS" albo nic się nie zmienia mimo ruchu — oznacza to, że IRL PRO zablokował dostęp do GPS w tym oknie. W takiej sytuacji zostają dwie opcje:
- używać wbudowanej mapy GPS z samego IRL PRO (do lokalizacji) razem z moim paskiem donate osobno,
- albo ręcznie wpisywać pozycję w adresie URL widgetu, np. dopisując `?lat=50.0647&lng=19.9450` i podmieniając co jakiś czas (niewygodne, ale działa zawsze).

**Licznik km** liczy się sam na podstawie kolejnych odczytów GPS (odrzuca drgania <6m i nagłe skoki >400m, żeby nie zawyżał wyniku błędnie). Zapisuje się lokalnie w tej przeglądarce/aplikacji, więc przetrwa odświeżenie. Reset: dopisz `?resetkm=1` do adresu raz, potem usuń ten fragment z URL.

**Tryb ręczny/edycji** (tylko do testów w zwykłej przeglądarce telefonu, NIE w IRL PRO): `dragon-radar.html?edit=1` pokazuje panel do ręcznego wpisania/zerowania dystansu.

## Pasek Donate — cel i kwota

Dwa sposoby ustawienia:
- **W adresie URL** — najpewniejsza metoda w IRL PRO (edytuj URL widgetu, gdy chcesz zaktualizować kwotę):
  `donate-bar.html?amount=180&goal=500`
- **Ręcznie w przeglądarce** (do szybkich testów): `donate-bar.html?edit=1` pokazuje pola na kwotę i cel + przycisk Zapisz.

⚠️ Uwaga techniczna: jeśli ustawisz wartość przez `?edit=1` w zwykłej przeglądarce telefonu (np. Chrome), a potem wkleisz **czysty** adres (bez `?edit=1`) do IRL PRO — IRL PRO ma swoje osobne, puste "pudełko" na dane (localStorage), więc zapisana wartość może się NIE przenieść. Najpewniej działa dopisywanie `?amount=` i `?goal=` bezpośrednio do adresu wklejonego w samym IRL PRO.

### Integracja z Tipply (prawdziwe wpłaty na żywo)

Sprawdziłem: Tipply ma **własny, gotowy widget "Cel donate"** (w panelu na tipply.pl), który sam liczy prawdziwe wpłaty i generuje link do wklejenia jako źródło w OBS/Streamlabs — to jest najbardziej niezawodna droga do prawdziwych, żywych danych (ja nie mam dostępu do Twojego konta Tipply, więc nie mogę tego zrobić za Ciebie). W panelu Tipply przy edycji widgetu jest też **konfigurator z opcją wklejenia własnego kodu (HTML/CSS/JS)** — to miejsce, w którym teoretycznie dałoby się "przeskórować" ich pasek na styl smoczych kul.

Nie mam jednak podglądu na strukturę Twojego konkretnego widgetu (wymaga zalogowania na Twoje konto), więc nie chcę zgadywać gotowego kodu CSS, który mógłby po prostu nie zadziałać. Żeby to dograć 1:1:

1. Załóż w Tipply cel "Laptop" na 500 zł (Panel → Widżety → Cel donate).
2. Skopiuj wygenerowany link/otwórz podgląd widgetu.
3. Prześlij mi ten link (albo kod źródłowy strony — w przeglądarce: "Wyświetl źródło strony" / "Zbadaj element") — dopasuję dokładny CSS w stylu smoczych kul do tego, co faktycznie tam jest.

Do tego czasu **`donate-bar.html` działa w pełni samodzielnie** (bez Tipply) — kwotę aktualizujesz ręcznie przez `?amount=` zerknąwszy co jakiś czas na panel Tipply, albo przez `?edit=1`.

## Podgląd lokalny (na komputerze, opcjonalnie)

```
python -m http.server 8000
```
i otwórz `http://localhost:8000/preview.html`.
