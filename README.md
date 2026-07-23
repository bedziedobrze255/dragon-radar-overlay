# Smoczy Radar + Pasek Donate — nakładka na stream (wersja mobilna)

Dwa niezależne widgety w klimacie Dragon Balla:

- **`dragon-radar.html`** — "Smoczy Radar": prosta czarna obudowa (jak prawdziwa zabawka), żywy zielony ekran z siatką i **prawdziwą mapą z GPS telefonu**, czerwony trójkąt-wskaźnik, pomarańczowe kropki-cele, licznik **dystansu w km liczony naprawdę z ruchu GPS**, data i godzina.
- **`donate-bar.html`** — banerowy pasek z 7 **smoczymi kulami**, płomienie przy tytule, plakietka `kick.com/krakowiakv2`, złoty efekt po osiągnięciu celu. **W pełni automatyczny** — sam pobiera na żywo kwotę/cel/nazwę z Twojego konta Tipply (albo tryb ręczny, patrz niżej).
- **`top-donators.html`** — ranking **"TOP WOJOWNICY"** (do 5 osób) w smoczej ramce. **W pełni automatyczny** — osadza żywy widget Tipply z prawdziwymi wpłatami (albo własna, stylizowana lista złoto/srebro/brąz w trybie ręcznym).
- **`panel.html`** — **panel sterowania** (otwierany w zwykłej przeglądarce, nie na streamie) — wpisujesz nazwę celu, kwoty i listę wspierających, klikasz "Generuj", dostajesz gotowy link do wklejenia w IRL PRO. Wygodniejsze niż ręczna edycja adresu URL.
- **`preview.html`** / **`stages-preview.html`** — tylko do podglądu na komputerze (nieużywane w telefonie/IRL PRO).

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

Tło wszystkich stron jest przezroczyste, więc nie trzeba nic więcej ustawiać (bez chroma key).

⚠️ **Ważne w IRL PRO:** pole **Width/Height** warstwy to rozmiar całego okienka WebView, NIE tylko widgetu. Jeśli zostawisz je na rozmiarze całego kadru (np. 1280×720), "Position" nie zadziała i widget wyląduje na środku/nachodzi na inne warstwy. Zmniejsz Width/Height do rozmiaru samego widgetu (radar: ok. 400×400, pasek/ranking: pełna szerokość × ok. 200–220 wysokości), dopiero wtedy "Position: Bottom right / Bottom" ustawi je poprawnie w rogu/na dole.

## Kick

Wszystkie widgety to zwykłe strony WWW — nie mają żadnej zależności od konkretnej platformy streamingowej. Renderują się jako obraz przechwytywany przez IRL PRO/Streamlabs, więc trafiają na Kick dokładnie tak samo jak reszta obrazu z kamery — nic dodatkowo nie trzeba konfigurować pod Kick.

## Smoczy Radar — jak działa GPS

Przy pierwszym otwarciu strona poprosi o **zgodę na dostęp do lokalizacji** — trzeba ją zaakceptować, żeby mapa i licznik km działały naprawdę.

⚠️ Uwaga: nie każda aplikacja pozwala swojemu wbudowanemu oknu przeglądarki pokazać to okienko z pytaniem o zgodę. Jeśli po dodaniu widgetu mapa stoi w miejscu (Kraków) i status na ekranie pokazuje "BRAK ZGODY NA GPS" albo nic się nie zmienia mimo ruchu — oznacza to, że IRL PRO zablokował dostęp do GPS w tym oknie. W takiej sytuacji zostają dwie opcje:
- używać wbudowanej mapy GPS z samego IRL PRO (do lokalizacji) razem z moim paskiem donate osobno,
- albo ręcznie wpisywać pozycję w adresie URL widgetu, np. dopisując `?lat=50.0647&lng=19.9450` i podmieniając co jakiś czas (niewygodne, ale działa zawsze).

**Licznik km** liczy się sam na podstawie kolejnych odczytów GPS (odrzuca drgania <6m i nagłe skoki >400m, żeby nie zawyżał wyniku błędnie). Zapisuje się lokalnie w tej przeglądarce/aplikacji, więc przetrwa odświeżenie. Reset: dopisz `?resetkm=1` do adresu raz, potem usuń ten fragment z URL.

**Tryb ręczny/edycji** (tylko do testów w zwykłej przeglądarce telefonu, NIE w IRL PRO): `dragon-radar.html?edit=1` pokazuje panel do ręcznego wpisania/zerowania dystansu.

## Pasek Donate — teraz w pełni automatyczny (na żywo z Tipply)

`donate-bar.html` domyślnie pobiera dane **naprawdę na żywo z Twojego konta Tipply** (Twój "Cel domyślny") — kwota, cel i nazwa aktualizują się same co ~7 sekund, bez dotykania czegokolwiek. Nic nie musisz wpisywać ręcznie. Napis "NA ŻYWO (TIPPLY)" pod paskiem potwierdza, że dane są prawdziwe.

Jak to działa technicznie: Tipply udostępnia publiczny (bez logowania) adres `https://tipply.pl/api/widget/goal/<ID_CELU>` — to ten sam adres, z którego korzysta ich własny widget. Podpiąłem go bezpośrednio pod `donate-bar.html`.

**Jeśli kiedyś zmienisz cel w Tipply** (nowa nazwa/kwota — po prostu edytuj go w Tipply, panel → Cele, dane w moim pasku zaktualizują się same, bo pasek pyta o ten sam cel). **Jeśli założysz zupełnie NOWY, osobny cel** w Tipply (inny wiersz na liście "Twoje cele wpłat"), musisz podać jego ID w adresie:
`donate-bar.html?tipplyGoalId=NOWE_ID_CELU`
(ID celu widać w adresie URL po kliknięciu "Skopiuj URL" przy danym celu w Tipply — to fragment po `/GOAL/`).

**Tryb ręczny / awaryjny** (gdyby Tipply padło, albo chcesz tymczasowo coś innego pokazać): dodaj `&tipply=0` do adresu, wtedy wraca stary tryb reczny opisany niżej.
- W adresie URL: `donate-bar.html?tipply=0&title=Laptop&amount=180&goal=500`
- Ręcznie w przeglądarce: `donate-bar.html?tipply=0&edit=1`
- Albo przez **panel sterowania** (`panel.html`) — generuje link w trybie ręcznym.

## Top Wojownicy — ranking wspierających (na żywo z Tipply)

`top-donators.html` domyślnie **osadza prawdziwy widget Tipply "Ranking wiadomości"** wewnątrz mojej smoczej ramki (płomienie, złota obwódka, plakietka kanału) — lista uzupełnia się sama, w 100% automatycznie, bo to dosłownie ten sam widget co w Tipply. Jedyne czego nie mogłem zrobić: Tipply nie udostępnia publicznego, prostego API z listą wpłat (tak jak dla celu) — dane idą przez ich wewnętrzny system na żywo (WebSocket), więc nie dało się bezpiecznie odtworzyć własnych, stylizowanych wierszy 1:1 bez ryzyka, że coś się zepsuje w trakcie streamu. W środku listy zobaczysz więc styl Tipply (nie moje złote/srebrne/brązowe kulki) — ale ramka dookoła i dane są w 100% żywe i automatyczne.

**Tryb ręczny** (własna, stylizowana lista złoto/srebro/brąz — ale wpisywana ręcznie): dodaj `&tipply=0` i dane osoby:
- **Panel sterowania** (`panel.html`) — wpisz imiona i kwoty, kliknij "Generuj link do rankingu".
- Ręcznie w URL: `top-donators.html?n1=Jan&a1=250&n2=Kasia&a2=180&n3=Michał&a3=90` (do `n5`/`a5`) — obecność `n1` automatycznie wyłącza tryb live.
- Tryb edycji: `top-donators.html?edit=1&tipply=0`.

## Podgląd lokalny (na komputerze, opcjonalnie)

```
python -m http.server 8000
```
i otwórz `http://localhost:8000/preview.html`.
