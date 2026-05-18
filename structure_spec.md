# structure_spec.md

## Cel zadania

Przekształć dokument źródłowy `LW044_AD_D_TTD_PTD_PL_3.pdf` oraz pomocniczy plik OCR/DOCX do dwóch uporządkowanych plików wynikowych:

1. `policy.json` – pełna, logiczna reprezentacja treści warunków ubezpieczenia.
2. `policy.html` – czytelna wersja HTML zachowująca strukturę dokumentu, numerację, tabele i hierarchię nagłówków.

Dokument dotyczy warunków ubezpieczenia osób fizycznych od utraty dochodu i następstw nieszczęśliwych wypadków, oznaczony kodem `LW044/AD_D_TTD_PTD/PL/3`.

## Źródła

- PDF jest źródłem referencyjnym dla układu, numeracji, tabel, nagłówków, kolejności treści i wyglądu dokumentu.
- DOCX/OCR może być używany jako pomocnicze źródło tekstu.
- Jeżeli OCR/DOCX różni się od obrazu PDF, pierwszeństwo ma obraz PDF.
- Nie wolno streszczać, skracać ani parafrazować treści. Należy przepisać pełną treść dokumentu.

## Najważniejsze problemy OCR, które trzeba ręcznie kontrolować

OCR może błędnie odczytywać:

- `§ 6` jako `86`;
- `§ 2` jako `82`;
- `§ 1` jako `§1` bez spacji;
- `Całkowita` jako `Calkowita`;
- `dłoni` jako `dioni`;
- polskie znaki, np. `ł`, `ą`, `ę`, `ś`, `ż`, `ź`, `ń`;
- podwójne słowa, np. `od od`, `i i`, `w w`;
- kolejność tabel – OCR może rozbić tabelę na nieczytelny ciąg tekstu;
- nagłówki i stopki jako część treści zasadniczej.

Każdą taką sytuację należy sprawdzić z obrazem PDF.

## Ogólna struktura dokumentu

Dokument ma 15 stron. Należy zachować następującą strukturę logiczną:

1. Metadane dokumentu.
2. Skorowidz z tabelą wymaganą przez art. 17 ust. 1 ustawy o działalności ubezpieczeniowej i reasekuracyjnej.
3. Postanowienia ogólne.
4. Maksymalna wysokość świadczeń.
5. Sekcja I – ubezpieczenie śmierci oraz inwalidztwa w następstwie nieszczęśliwych wypadków.
6. Sekcja II – ubezpieczenie od utraty dochodu.
7. Postanowienia wspólne dla wszystkich sekcji.
8. Wyłączenia.
9. Ryzyka aktywnego życia.
10. Klasy ryzyka zawodowego.
11. Procedura roszczeniowa / wypłata świadczeń.
12. Definicje.
13. Reklamacje, jurysdykcja, prawo właściwe i postanowienia końcowe.

Nie należy traktować nagłówków/stopki z numerem strony jako treści paragrafów. Można je jednak zachować w metadanych stron, jeżeli jest to przydatne.

## Docelowa struktura JSON

Plik `policy.json` powinien mieć strukturę zbliżoną do poniższej:

```json
{
  "document": {
    "code": "LW044/AD_D_TTD_PTD/PL/3",
    "title": "Warunki ubezpieczenia osób fizycznych od utraty dochodu i następstw nieszczęśliwych wypadków",
    "language": "pl",
    "pages": 15,
    "source_files": {
      "reference_pdf": "LW044_AD_D_TTD_PTD_PL_3.pdf",
      "ocr_docx": "..."
    }
  },
  "index": [
    {
      "information_type": "Przesłanki wypłaty odszkodowania i innych świadczeń",
      "references": [
        "§ 3",
        "§ 4 ustęp 2 i 3",
        "§ 5 ustęp 1-7"
      ]
    }
  ],
  "sections": [
    {
      "id": "section-i",
      "title": "Postanowienia dotyczące ubezpieczenia śmierci oraz inwalidztwa w następstwie nieszczęśliwych wypadków",
      "paragraphs": [
        {
          "number": "§ 3",
          "title": "Zakres ubezpieczenia Sekcji I",
          "clauses": [
            {
              "number": null,
              "text": "Ochrona ubezpieczeniowa...",
              "subclauses": []
            }
          ],
          "tables": []
        }
      ]
    }
  ],
  "definitions": [
    {
      "term": "...",
      "definition": "...",
      "source_paragraph": "§ 15"
    }
  ],
  "tables": [
    {
      "id": "disability-benefits-table",
      "title": "Tabela uszczerbków",
      "source_paragraph": "§ 5 ustęp 1",
      "columns": ["Rodzaj uszczerbku", "Prawa", "Lewa", "Wartość"],
      "rows": []
    }
  ],
  "quality_control": {
    "checked_against_pdf_images": true,
    "known_ocr_corrections": [
      {"wrong": "86", "correct": "§ 6"},
      {"wrong": "82", "correct": "§ 2"}
    ],
    "requires_human_review": []
  }
}
```

## Zasady dla JSON

- JSON ma być poprawny składniowo i możliwy do parsowania przez `JSON.parse()`.
- Nie używaj komentarzy wewnątrz JSON.
- Zachowaj pełną numerację: `§`, ustępy, litery `(a)`, `(b)`, `(c)` itd.
- Każdy paragraf powinien mieć osobny obiekt.
- Każdy ustęp powinien mieć osobny obiekt w tablicy `clauses`.
- Wyliczenia literowe powinny być zagnieżdżone jako `subclauses`.
- Tabele należy zapisać jako dane strukturalne, a nie jako jeden blok tekstu.
- Definicje z paragrafu definicyjnego powinny być dodatkowo zapisane w osobnej tablicy `definitions`.
- Zachowaj dokładne brzmienie pojęć pisanych wielką literą, np. `Ubezpieczony`, `Ubezpieczyciel`, `Umowa ubezpieczenia`, `Polisa`.
- Nie poprawiaj merytorycznie treści. Poprawiaj tylko oczywiste błędy OCR po weryfikacji z PDF.

## Docelowa struktura HTML

Plik `policy.html` powinien być samodzielnym dokumentem HTML.

Minimalna struktura:

```html
<!doctype html>
<html lang="pl">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <title>LW044/AD_D_TTD_PTD/PL/3</title>
  <link rel="stylesheet" href="styles.css" />
</head>
<body>
  <main class="policy-document">
    <header class="document-header">
      <p class="document-code">LW044/AD_D_TTD_PTD/PL/3</p>
      <h1>Warunki ubezpieczenia osób fizycznych od utraty dochodu i następstw nieszczęśliwych wypadków</h1>
    </header>

    <section class="index-section" id="skorowidz">
      <h2>Skorowidz</h2>
      <table>
        <thead>
          <tr>
            <th>Rodzaj informacji</th>
            <th>Numer jednostki redakcyjnej wzorca umownego</th>
          </tr>
        </thead>
        <tbody></tbody>
      </table>
    </section>
  </main>
</body>
</html>
```

## Zasady dla HTML

- HTML ma być semantyczny i czytelny.
- Używaj `h1` dla tytułu dokumentu.
- Używaj `h2` dla sekcji głównych, np. `SEKCJA I`, `SEKCJA II`.
- Używaj `h3` dla paragrafów, np. `§ 1 POSTANOWIENIA OGÓLNE`.
- Używaj list uporządkowanych dla ustępów, ale zachowaj oryginalne numery ustępów.
- Używaj list literowych dla punktów `(a)`, `(b)`, `(c)`.
- Tabele z PDF odwzoruj jako prawdziwe tabele HTML, nie jako tekst z odstępami.
- Dla tabel z kolumnami `Prawa` i `Lewa` zachowaj osobne kolumny.
- Nie kopiuj nagłówków i stopek strony jako zwykłych akapitów.
- Możesz dodać atrybuty `data-page`, `data-paragraph`, `data-clause`, aby później łatwiej mapować HTML do JSON.

Przykład:

```html
<section class="policy-section" id="sekcja-i" data-page-start="3">
  <h2>SEKCJA I</h2>
  <p class="section-subtitle">Postanowienia dotyczące ubezpieczenia śmierci oraz inwalidztwa w następstwie nieszczęśliwych wypadków</p>

  <article class="paragraph" id="par-3" data-paragraph="§ 3">
    <h3>§ 3 Zakres ubezpieczenia Sekcji I</h3>
    <p>Ochrona ubezpieczeniowa udzielana na podstawie Sekcji I niniejszych warunków obejmuje...</p>
  </article>
</section>
```

## Szczególne wskazówki dla tabel

### Skorowidz

Tabela skorowidza ma dwie kolumny:

1. `Rodzaj informacji`
2. `Numer jednostki redakcyjnej wzorca umownego`

W JSON powinna być osobną tablicą `index`. W HTML powinna być tabelą.

### Tabela uszczerbków z § 5

To najważniejsza tabela w dokumencie. Należy ją potraktować jako strukturę danych, nie jako tekst.

Występują w niej różne typy wierszy:

- nagłówki kategorii, np. `Całkowite trwałe uszczerbki na zdrowiu`, `Trwałe uszczerbki głowy`, `Trwałe uszczerbki kończyn górnych`, `Trwałe uszczerbki kończyn dolnych`;
- wiersze z jedną wartością procentową;
- wiersze z dwiema wartościami procentowymi dla `Prawa` i `Lewa`;
- wiersze opisowe wieloliniowe.

Przykładowa struktura JSON dla wiersza tabeli:

```json
{
  "category": "Trwałe uszczerbki kończyn górnych",
  "item": "Utrata jednego ramienia lub jednej dłoni",
  "right": "60%",
  "left": "50%",
  "value": null
}
```

Dla wiersza bez podziału prawa/lewa:

```json
{
  "category": "Trwałe uszczerbki głowy",
  "item": "Utrata jednego oka",
  "right": null,
  "left": null,
  "value": "40%"
}
```

## Kontrola jakości

Po wygenerowaniu `policy.json` i `policy.html` wykonaj kontrolę jakości:

1. Sprawdź, czy wszystkie paragrafy od `§ 1` do końca dokumentu są obecne.
2. Sprawdź, czy nie ma błędów OCR typu `86`, `82`, `Calkowita`, `dioni`, `wyplaty`.
3. Sprawdź, czy tabele mają kompletne wiersze i wartości procentowe.
4. Sprawdź, czy tekst nie jest zdublowany.
5. Sprawdź, czy JSON jest poprawny składniowo.
6. Sprawdź, czy HTML otwiera się w przeglądarce bez błędów strukturalnych.
7. Sprawdź, czy treść HTML i JSON jest zgodna z PDF, a nie tylko z OCR.

## Pliki wynikowe

Utwórz następujące pliki:

- `policy.json`
- `policy.html`
- opcjonalnie `styles.css`
- opcjonalnie `quality_report.md`

W `quality_report.md` opisz:

- które strony zostały sprawdzone ręcznie z obrazem PDF;
- jakie błędy OCR poprawiono;
- które fragmenty wymagają dalszej ludzkiej weryfikacji;
- czy wszystkie tabele zostały odwzorowane.

## Zakazy

- Nie streszczaj dokumentu.
- Nie pomijaj powtarzalnych lub technicznych fragmentów.
- Nie zastępuj tabel zwykłym tekstem.
- Nie zmieniaj sensu postanowień.
- Nie dopisuj nowych postanowień.
- Nie usuwaj definicji ani wyłączeń.
- Nie generuj JSON tylko dla części dokumentu.
- Nie używaj OCR bez weryfikacji obrazu PDF.
