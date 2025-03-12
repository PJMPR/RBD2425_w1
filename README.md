
# Bazowe pojęcia dotyczące baz danych

Przedmiot ten w całości poświęcony będzie bazom danych. Celowo nie wspominam o relacyjnych, bowiem w trakcie semestru poznamy kilka rodzajów baz danych. Niemniej, zacząć należy od zrozumienia, czym są i do czego służą Bazy Danych.

## Czym jest baza danych?

Jako bazę danych rozumiemy uporządkowaną kolekcję danych, wykorzystywanych do modelowania danych, ich kształtu i związków między nimi. Technicznie rzecz biorąc, każdą formę zbierania danych, chociażby przy użyciu kartki papieru i długopisu, możemy określić jako bazę danych. Niemniej, na tych zajęciach zajmować się będziemy jedynie informatyczną implementacją baz danych. Najprostszym jej przykładem będą pliki tekstowe i arkusze kalkulacyjne i to właśnie arkuszami kalkulacyjnymi posłużymy się jako wstępnym przykładem do zrozumienia, czym są i jak działają bazy danych.

### Przykład tabeli studentów:

| Imię       | Nazwisko    | Kolos 1 | Ocena |
|------------|------------|---------|-------|
| Paweł      | Lelental   | 60%     | 3     |
| Jan        | Nowak      | 74%     | 4     |
| Paweł      | Kowal      | 80%     | 4     |
| Jarosław   | K.         | 8%      | 2     |

Pojedyncza komórka sama w sobie nie przedstawia dla nas żadnej wartości informacyjnej, bowiem pojedyncza dana, np. "Paweł", niewiele nam mówi. Dopiero postawienie takiej danej w kontekście przedstawia jakąkolwiek wartość informacyjną.

## Podstawowe pojęcia

- **Encja (tabela)** - w teorii relacyjnej określana również jako relacja. Baza danych składa się z przynajmniej jednej tabeli, a każda tabela składa się z przynajmniej jednego pola.
- **Krotka (rekord)** - unikatowy wpis w tabeli opisujący cechy jakiegoś obiektu.
- **Atrybut (pole)** - najmniejszy element struktury bazy danych, opisujący jedną cechę rekordu.
- **Klucz główny (Primary Key, PK)** - specjalne pole tabeli, które pozwala na jednoznaczną identyfikację rekordu.

Dla studentów unikalnym identyfikatorem może być numer indeksu:

| Indeks | Imię       | Nazwisko    | Kolos 1 | Ocena |
|--------|------------|-------------|---------|-------|
| s13820 | Paweł      | Lelental    | 60%     | 3     |
| s2137  | Jan        | Nowak       | 74%     | 4     |
| s42069 | Paweł      | Kowal       | 80%     | 4     |
| s500   | Jarosław   | K.          | 8%      | 2     |

### Zasady projektowania tabel:

- Każde pole powinno zawierać jedną wartość.
- Nazwa pola powinna jednoznacznie identyfikować rodzaj tej wartości.

W źle zaprojektowanych bazach danych standardowymi błędami będą:

- **Pola złożone** - zawierające więcej niż jedną wartość w ramach jednego pola.
- **Pola wielowartościowe** - np. pole "Uprawnienia", zawierające "admin, user".
- **Pole obliczeniowe** - zawierające wynik obliczenia wartości z dwóch lub więcej pól.

## Relacje w bazach danych

Tabele w bazie mogą być ze sobą w jakiś sposób powiązane. Istnieją trzy główne rodzaje relacji:

1. **Jeden do jednego (1:1)** - jeden rekord w tabeli A odpowiada dokładnie jednemu rekordowi w tabeli B.
2. **Jeden do wielu (1:n)** - jeden rekord w tabeli A może być powiązany z wieloma rekordami w tabeli B.
3. **Wiele do wielu (m:n)** - wiele rekordów w tabeli A może być przypisanych do wielu rekordów w tabeli B.

### Przykład relacji:

#### Osoby:
| Indeks  | Imię   | Nazwisko  |
|---------|--------|----------|
| s13820  | Paweł  | Lelental |
| s2137   | Jan    | Nowak    |

#### Oceny:
| Przedmiot | Ocena | Indeks  |
|-----------|-------|--------|
| WIA2      | 3     | s13820 |
| RBD       | 3,5   | s2137  |
| RBD       | 4     | s13820 |

Tabela "Oceny" wskazuje, że do indeksu s2137 przypisana jest ocena 3.5 z RBD. Z tabeli "Osoby" możemy wywnioskować, że do indeksu s2137 przypisany jest Jan Nowak.

Z tabeli **Oceny** wywnioskować możemy, że do indeksu `s2137` przypisana jest ocena `3.5` z **RBD**. Z tabeli **Osoby** wywnioskować możemy, że do indeksu `s2137` przypisany jest **Jan Nowak**. W taki sposób łączyć będziemy ze sobą dane znajdujące się w oddzielnych tabelach, ale ze sobą powiązane. Istnieją 3 rodzaje relacji:

### Jeden do jednego - 1:1
Najprostszy do zrozumienia rodzaj relacji. Łączy dokładnie **jeden** rekord z **Tabeli 1** z dokładnie **jednym** rekordem z **Tabeli 2**. Stosowany często do rozszerzania pewnych informacji. 

Dla przykładu, w danym momencie jedna osoba może posiadać **tylko jeden paszport** w danym kraju. W momencie wydania nowego, stary jest unieważniany. Dlatego w krajowej bazie danych paszport możemy połączyć z daną osobą połączeniem **1:1**. 

Zapis na diagramie ERD:
```
PERSONS
  |
  | have
  |
PASSPORTS
```

### Jeden do wielu - 1:n
Odrobinę bardziej złożony i bardzo często stosowany. W tym rodzaju relacji **jeden rekord** z **Tabeli 1** przypisany może być do **wielu rekordów** z **Tabeli 2**. 

Przykładem takiej relacji może być związek pomiędzy przedstawionymi wyżej tabelami **Osoby** i **Oceny**, ale również przedstawiony poniżej przykład, w którym **wiele biletów lotniczych** może być przypisanych do **jednego paszportu**, ale na **jednym bilecie** może być **tylko jeden paszport**.

Zapis na diagramie ERD:
```
PASSPORTS
  |
  | have assigned
  |
TICKETS
```

### Wiele do wielu - m:n
Ten rodzaj relacji określa, że **wiele rekordów** z **Tabeli 1** może być przypisanych do **wielu rekordów** **Tabeli 2**. W implementacji bazy danych taka relacja wymaga stworzenia dodatkowej tabeli, tzw. **łączącej** lub **asocjacyjnej**. Spotkać się można również z określeniem **tabela agregująca**, ale jest ono błędne.

Przykładem takiej relacji może być sytuacja z **rezerwacjami lotniczymi**, w której na **jednej rezerwacji** znajduje się **wiele osób**, a **każda osoba** może być przypisana do **wielu rezerwacji**.

