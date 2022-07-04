# Małe zadanie - Podobne wiersze

Należy napisać program **wyszukujący w tekście grupy podobnych wierszy**.

## Definicje

Wiersz tekstu składa się ze słów oddzielonych białymi znakami. Białe znaki mogą też występować na początku i na końcu wiersza. Słowo składa się z ciągu znaków o kodach ASCII od `33` do `126`. Białe znaki są to znaki reprezentowane w języku C jako `' ', '\t', '\n', '\v', '\f', '\r'`, czyli znaki o kodach ASCII odpowiednio `32, 9, 10, 11, 12, 13`. Wiersz kończy się (na ogół) znakiem przejścia do nowej linii, czyli znakiem `'\n'`. Słowo może być liczbą bądź nieliczbą. Słowo jest liczbą, jeśli reprezentuje:

- liczbę całkowitą zapisaną dziesiętnie (rozpoznawane wartości z przedziału przynajmniej od `-9223372036854775808` do `18446744073709551615`), np. `42`, `-1`, `09`, `+2`, `0`, `-0`, `+0`, `-02`;
- liczbę całkowitą nieujemną zapisaną szesnastkowo (rozpoznawane wartości przynajmniej do `18446744073709551615`), np. `0X0001`, `0XABC`;
- liczbę całkowitą nieujemną zapisaną ósemkowo (rozpoznawane wartości przynajmniej do `18446744073709551615`), np. `0177`, `0001`;
- liczbę zmiennoprzecinkową, np. `0.25`, `.33`, `-1E-1`, `INF`, `-INF`.

Jeśli słowo nie jest liczbą, to jest nieliczbą. **Uwaga**: `nan`, jak sama nazwa wskazuje (ang. *not a number*), jest nieliczbą.

Dwie **liczby** są podobne, jeśli reprezentują tę samą liczbę lub są **nieodróżnialne w arytmetyce komputera**. Dwie **nieliczby** są podobne, jeśli **reprezentują takie samo słowo**. Przy czym małe litery alfabetu angielskiego są w słowach nieodróżnialne od ich wielkich odpowiedników. Dwa **wiersze** są podobne, jeśli **zawierają jednakowe wielozbiory nieliczb i liczb**.

## Opis działania programu

Program czyta wiersz po wierszu ze standardowego wejścia. Program **ignoruje wiersze zaczynające się znakiem `#` i niezawierające żadnego słowa**. Po wczytaniu i przetworzeniu wszystkich wierszy z wejścia program wypisuje na standardowe wyjście grupy wierszy podobnych, w każdym wierszu jedna grupa. Dane wyjściowe mają zostać posortowane rosnąco według numerów wierszy. Wiersze są numerowane od jedynki. Numerowane są wszystkie wiersze, również te ignorowane i błędne.

## Obsługa błędów i zakończenie programu

Jeśli wiersz **zawiera niedozwolony znak**, to jest to **błędny wiersz** i program wypisuje na standardowe wyjście diagnostyczne komunikat `ERROR nr\n`, gdzie `nr` jest numerem tego wiersza (komunikat zakończony jest linuksowym znakiem przejścia do nowej linii, czyli w języku C znakiem `\n` o kodzie ASCII `10`). Błędnych wierszy program dalej nie przetwarza.

Program kończy działanie po przetworzeniu wszystkich wierszy z wejścia. Program powinien wtedy jawnie zwolnić całą zaalokowaną pamięć i zakończyć się kodem 0. Jeśli wystąpi błąd krytyczny, np. zabraknie pamięci, program powinien zakończyć się awaryjnie kodem `1`.

## Przykładowe dane

Przykładowe dane dla programu znajdują się w załączonej paczce `example`, czyli:
- [**example.in**](https://github.com/kfernandez31/IPP-1-Similar-Lines/blob/main/src/test/example.in)
- [**example.out**](https://github.com/kfernandez31/IPP-1-Similar-Lines/blob/main/src/test/example.out)
- [**example.err**](https://github.com/kfernandez31/IPP-1-Similar-Lines/blob/main/src/test/example.err)


## Makefile

Częścią zadania jest napisanie pliku [**makefile**](https://github.com/kfernandez31/IPP-1-Similar-Lines/blob/main/makefile). W wyniku wywołania polecenia make powinien powstać program wykonywalny `similar_lines`. Jeśli któryś z plików źródłowych ulegnie zmianie, ponowne wpisanie make powinno na nowo stworzyć plik wykonywalny. Plik [**makefile**](https://github.com/kfernandez31/IPP-1-Similar-Lines/blob/main/makefile) powinien działać w następujący sposób:

- osobno kompiluje każdy plik .c,
- linkuje wszystkie pliki .o,
- przy zmianie w pliku .c lub .h wykonuje tylko niezbędne akcje,
- wywołanie make clean usuwa plik wykonywalny i dodatkowe pliki powstałe podczas kompilowania.

Docelowo pliki rozwiązania należy kompilować programem `gcc` z opcjami:
```
-Wall -Wextra -std=c11 -O2
```

## Skrypt testujący

Osobną częścią zadania jest napisanie skryptu [**test.sh**](https://github.com/kfernandez31/IPP-1-Similar-Lines/blob/main/test.sh). Po wywołaniu
```
./test.sh prog dir
```
skrypt powinien uruchomić program `prog` dla wszystkich plików wejściowych postaci `dir/*.in`, porównać wyniki z odpowiadającymi im plikami `dir/*.out` i `dir/*.err`, a następnie wypisać, które testy zakończyły się powodzeniem, a które niepowodzeniem. Skrypt powinien akceptować parametry z dowolną ścieżką, jaką akceptuje powłoka.

Do wykrywania problemów z zarządzaniem pamięcią należy użyć programu `valgrind`.

## Pozostałe wymagania

Rozwiązanie zadania powinno być napisane w języku C i korzystać z dynamicznie alokowanych struktur danych. Implementacja powinna być jak najefektywniejsza. Należy unikać zbędnego alokowania pamięci i kopiowania danych.

Program będzie sprawdzany na maszynie **students.mimuw.edu.pl** i musi kompilować się oraz działać na tej maszynie.

Kod programu powinien być podzielony na **moduły**.

Moduł zwykle składa się z dwóch plików, np. `x.c` i `x.h`, gdzie `x` jest nazwą modułu, implementowanej przez ten moduł struktury danych lub tp. Plik nagłówkowy `x.h` zawiera deklaracje operacji, struktur udostępnianych przez moduł `x`, a plik `x.c` – ich implementację. W pliku nagłówkowym należy umieszczać jedynie deklaracje i definicje, które są częścią interfejsu tego modułu. Wszystkie szczegóły powinny być ukryte w pliku z implementacją.

Moduł może też składać się z samego pliku nagłówkowego, jeśli udostępnia jedynie definicje stałych bądź typów, lub funkcji, które sugerujemy kompilatorowi do rozwijania w miejscu wywołania (`static inline`).

Moduł może też składać się z samego pliku z implementacją, jeśli nie udostępnia żadnego interfejsu – żadne funkcje z tego modułu nie są wywoływane z innych modułów.

Ponadto rozwiązanie powinno zawierać pliki:
- `makefile` – Patrz punkt ["**makefile**"](https://github.com/kfernandez31/IPP-1-Similar-Lines#makefile).
- `test.sh` – Patrz punkt [**"skrypt testujący”**](https://github.com/kfernandez31/IPP-1-Similar-Lines#skrypt-testuj%C4%85cy).

Rozwiązanie należy oddać jako archiwum skompresowane programem `zip` lub parą programów `tar` i `gzip`.
