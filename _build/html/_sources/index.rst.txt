.. Partycjonowanie danych documentation master file, created by
   sphinx-quickstart on Thu Apr 13 12:41:27 2023.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

Sprawozdanie projektowe z kursu Bazy danych 1
==================================================

.. toctree::
   :maxdepth: 2
   :caption: Contents:


:Authors:
    Eryk Mika,
    Michał Łabowicz

:Version: 1.0 of 20.04.2023
:Course: Databases I

..
    Indices and tables
    ==================
    * :ref:`genindex`
    * :ref:`modindex`
    * :ref:`search`


Części sprawozdania
---------------------

1. Cele stosowania partycjonowania danych

2. Zastosowanie partycjonowania

3. Zalety i wady partycjonowania

4. Implementacja partycjonowania w PostgreSQL

Wstęp teoretyczny
---------------------

Partycjonowanie danych wykorzystuje podzielenie zasobów bazy danych na wiele fizycznych urządzeń. Jest to rozwiązanie podobne do replikacji, lecz w tym przypadku współdzielona jest jedna kopia danych. Partycjonowanie danych może być używane do skalowania bazy danych poprzez dodawanie nowych serwerów lub wirtualnych maszyn, co pozwala na łatwe rozszerzenie przetwarzania danych. Poza tym partycjonowanie może pomóc w minimalizacji ryzyka utraty danych, gdyż w przypadku awarii jednego serwera dane z pozostałych partycji mogą być nadal dostępne. Prowadzi to do tego, że nie tylko jedna maszyna musi poradzić sobie z zapisami i odczytami. Dla przykładu, mając dane na jednym węźle, który obsługuje 100 zapytań na sekundę, to dzieląc dane na 10 węzłów, przynajmniej w teorii będziemy obsługiwać 1000 zapytań na sekundę. Zależy to jednak od faktu, czy dane są równomiernie rozłożone. Podziału tego można dokonać na wiele sposobów.

Metody partycjonowania danych
------------------------------

1. **Partycjonowanie danych klucz-wartość** - losujemy partycję zapisu danych, metoda naiwna, w przypadku odczytu trzeba sprawdzić wszystkie partycje

2. **Według zakresu kluczy** - możliwe nadmierne obciążenie pewnych partycji, można uniknąć w przypadku dużej liczby partycji

3. **Według funkcji haszującej** - dane będą równomiernie rozłożone, wykonuje się obliczenie reszty z dzielenia przez liczbę partycji. Odczyt i zapis zawsze są z tego samego serwera na podstawie identyfikatora wpisu.

4. **Partycjonowanie według indeksu pomocniczego (drugorzędnego)**, który nie identyfikuje jednoznacznie wpisu, tylko przyspiesza przeszukiwanie i odczyt. Innymi słowy, indeks pomocniczy to tylko wskaźnik do wpisu (wiersza, dokumentu).
