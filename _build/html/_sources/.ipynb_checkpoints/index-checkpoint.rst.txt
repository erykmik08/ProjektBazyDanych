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


    Indices and tables
    ==================
    * :ref:`genindex`
    * :ref:`modindex`
    * :ref:`search`


Partycjonowanie danych
------------------------

1. Cele stosowania partycjonowania danych

2. Zastosowanie partycjonowania

3. Zalety i wady partycjonowania

4. Implementacja partycjonowania w PostgreSQL

Wstęp teoretyczny
---------------------

Partycjonowanie danych wykorzystuje podzielenie zasobów bazy danych na wiele fizycznych urządzeń. Jest to rozwiązanie podobne do replikacji, lecz w tym przypadku współdzielona jest jedna kopia danych. Partycjonowanie danych może być używane do skalowania bazy danych poprzez dodawanie nowych serwerów lub wirtualnych maszyn, co pozwala na łatwe rozszerzenie przetwarzania danych. Poza tym partycjonowanie może pomóc w minimalizacji ryzyka utraty danych, gdyż w przypadku awarii jednego serwera dane z pozostałych partycji mogą być nadal dostępne. Prowadzi to do tego, że nie tylko jedna maszyna musi poradzić sobie z zapisami i odczytami. Dla przykładu, mając dane na jednym węźle, który obsługuje 100 zapytań na sekundę, to dzieląc dane na 10 węzłów, przynajmniej w teorii będziemy obsługiwać 1000 zapytań na sekundę. Zależy to jednak od faktu, czy dane są równomiernie rozłożone. Podziału tego można dokonać na wiele sposobów.

PostgreSQL wspiera podstawowe partycjonowanie tabel. Oferowane jest wsparcie dla następujących metod:

1. **Podział zakresu** - tabela jest podzielona na zakresy kluczy, które są przydzielone na daną fizyczną partycję, bez nakładania się zakresów. Przedział zakresów jest domknięty po lewej stronie i otwarty po prawej, tzn. np. [10,20).

2. **Partycjonowanie listowe** - tabela jest podzielona "z góry" na podstawie, jakie klucze trafiają na daną partycję.

3. **Partycjonowanie haszowane** - mamy określony dzielnik i resztę z dzielenia dla każdej partycji - przydzielamy klucze do nich na podstawie reszty z dzielenia przez ten dzielnik.


1. Cele stosowania partycjonowania danych
--------------------------------------------

Cele partycjonowania danych w bazach danych mogą obejmować:

    **Poprawę wydajności**: Partycjonowanie danych może poprawić wydajność zapytań, ponieważ bazy danych będą przeszukiwać tylko te partycje, które zawierają potrzebne dane, a nie całą bazę danych.

    **Zwiększenie dostępności**: Partycjonowanie danych może również poprawić dostępność systemu, ponieważ jeśli jedna partycja ulegnie awarii, to pozostałe partycje nadal będą działać.

    **Ułatwienie zarządzania**: Partycjonowanie danych może ułatwić zarządzanie bazą danych, ponieważ można skoncentrować się tylko na potrzebnych partycjach i wykonywać operacje na nich, zamiast na całej bazie danych.

    **Optymalizacja archiwizacji i backupów**: Partycjonowanie danych umożliwia lepsze zarządzanie archiwizacją i backupami, ponieważ można archiwizować i backupować tylko te partycje, które się zmieniły, a nie całą bazę danych.

    **Umożliwienie równoległego przetwarzania**: Partycjonowanie danych umożliwia równoległe przetwarzanie danych, co może skrócić czas wykonywania zadań.

    **Optymalizacja wykorzystania pamięci**: Partycjonowanie danych może zmniejszyć zużycie pamięci, ponieważ dane są dzielone na mniejsze części, co może zwiększyć wykorzystanie pamięci podręcznej.