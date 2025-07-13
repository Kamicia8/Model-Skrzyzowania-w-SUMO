# Model Skrzyżowania w SUMO

Autorzy: Kamila Ćwikła, Klaudia Stodółkiewicz

Projekt prowadzony w ramach przedmiotu **Modelowanie i Symulacja Systemów** na pierwszym semestrze studiów magisterskich **Informatyka - Data Science** na **Wydziale Informatyki AGH w Krakowie**.

Projekt polega na stworzeniu i przeprowadzeniu symulacji rzeczywistego skrzyżowania przy użyciu narzędzia **SUMO (Simulation of Urban Mobility)**. Celem jest odwzorowanie ruchu drogowego, analiza wydajności i porównanie wyników z rzeczywistymi danymi.

## Kluczowe etapy:

1. Modelowanie skrzyżowań – stworzenie modelu krakowskiego skrzyżowania w SUMO.
2. Konfiguracja symulacji – parametryzacja ruchu (natężenie, sygnalizacja, prędkości, typy pojazdów itp.).
3. Zbieranie danych – pozyskanie rzeczywistych danych z wybranych skrzyżowań.
4. Walidacja modelu – porównanie wyników symulacji z rzeczywistymi danymi, ocena dokładności odwzorowania.

## Przebieg projektu:

Początkowym etapem projektu było szerokie zapoznanie się z problemem, przegląd literatury (plik: `literatura.pdf`) i narzędzi, z których należy i można skorzystać. W projekcie zdecydowano się na symulację skrzyżowania w Krakowie, dokładnie skrzyżowanie ulic Buszka - Piastowska - Reymonta. Analizowano wyłącznie ruch samochodów, uzględniono zmianę swiateł, a także różne pory dnia (ranne oraz popołudniowe). Przy tworzeniu symulacji korzystano z danych podanych w artykule `Analiza_funkcjonowania_skrzyżowania.pdf`.

### Import mapy z OSM:

Do pobrania fragmentu rzeczywistej mapy wykorzystano narzędzie **`osmWebWizard.py`** z pakietu SUMO:

#### Instrukcja:

1. Przejdź do katalogu instalacji SUMO, domyślnie:

`C:\Program Files (x86)\Eclipse\Sumo\tools`

2. Kliknij prawym przyciskiem myszy w pustym miejscu w folderze tools i wybierz "Otwórz w terminalu".

3. Uruchom komendę:

```bash
python osmWebWizard.py
```

4. W nowym oknie wybierz interesujący fragment mapy i kliknij **Generate Scenario**.

![alt text](Zdjęcia\image-1.png)

5. Otworzy się SUMO z wygenerowaną częścią mapy:

![alt text](Zdjęcia\image-2.png)

6. W SUMO przy użyciu skrótu klawiszowego `Ctrl + T` otwieramy NetEdit:

![alt text](Zdjęcia\image-3.png)

---

### Przygotowanie skrzyżowania

1. **Oczyszczenie mapy** - usunięto zbędne obiekty (chodniki, budynki), które mogły zakłócać symulację.

![alt text](Zdjęcia\image-4.png)

2. **Nazewnictwo węzłów (`junctions`)** – dostosowane do danych z artykułu `Analiza_funkcjonowania_skrzyżowania.pdf`.

![alt text](Zdjęcia\image-5.png)

3. **Dostosowanie długości ulic** - zmierzono i zweryfikowano przy pomocy Google Maps.

Wyjaśnienie nazwnictwa na przykładzie:

`Do` - oznaczenie D zgodnie z artykułem, druga literka `o` oznacza out, czyli wyjazdowy `line` w symulacji, from J to D oznacza z junction J (skryżowania wszystkich dróg) do junction D:

![alt text](Zdjęcia\image-6.png)

Poniżej prezentujemy wszyskie line'y do junction J:

![alt text](Zdjęcia\image-7.png)

### Konfiguracja świateł

1. **Analiza i synchronizacja sygnalizacji** wykonana została w Excelu:

![alt text](Zdjęcia\image-9.png)

2. **Wprowadzenie do SUMO** - konfiguracja została przeniesiona do NetEdit:

![alt text](Zdjęcia\image-8.png)

### Struktura projektu

W folderze `SumoBuszka` znajduje się cała struktura symulacji:

- `BuszkaMain.net.xml` - pełny opis sieci (drogi, skrzyżowania, światła).
- `Buszka.sumocfg` = plik konfiguracyjny uruchamiający symulację.
- `routes8_9.rou.xml` / `routes15_45_16_45.rou.xml` - definicje ruchu (poranna i popołudniowa symulacja).
- `Statystyki/` - skrypty do zbierania danych i walidacji wyników.

---

### Folder z importu OSM

Podczas importu mapy z OpenStreetMap za pomocą `osmWebWizard.py`, SUMO automatycznie tworzy folder (np. `2025-04-22-13-34-51`), który zawiera:

- `osm.sumocfg` - konfiguracja symulacji,
- `osm.net.xml` - sieć drogowa,
- `osm.poly.xml` - dane wizualne,
- `osm.view.xml` - ustawienia widoku.

Ten folder **musi zostać zachowany** w projekcie, ponieważ pliki `.sumocfg` korzystają z niego przez ścieżki względne. Usunięcie lub przeniesienie folderu bez aktualizacji tych ścieżek prowadzi do błędów podczas uruchamiania. Dlatego folder ten został dołączony do repozytorium.

---

#### Ostateczna forma symulacji (po oczyszczeniu mapy i wprowadzeniu konfiguracji):

![alt text](Zdjęcia\image-10.png)
