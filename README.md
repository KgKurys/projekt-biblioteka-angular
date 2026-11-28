# Biblioteka – projekt Angular

**Temat:** Biblioteka
**Skład grupy:** Grzegorz Kuryś, Łukasz Regliński

Aplikacja do obsługi biblioteki: katalog książek, czytelnicy oraz wypożyczenia i zwroty. Dane trzymane na json-server.

## Klasy i pola

- **Book** – `id: number`, `title: string`, `isbn: string`, `publishedDate: Date`, `pageCount: number`, `genres: string[]`, `author: Author`, `copies: BookCopy[]`
- **Author** – `firstName: string`, `lastName: string`, `birthDate: Date`
- **BookCopy** – `inventoryNumber: string`, `condition: string`, `available: boolean`
- **Reader** – `id: number`, `firstName: string`, `lastName: string`, `email: string`, `birthDate: Date`, `address: Address`
- **Address** – `street: string`, `city: string`, `postalCode: string`
- **Loan** – `id: number`, `bookId: number`, `inventoryNumber: string`, `readerId: number`, `loanDate: Date`, `dueDate: Date`, `returnDate: Date | null`

## Funkcjonalności

- dodawanie, edycja i usuwanie książek
- dodawanie, edycja i usuwanie czytelników
- sortowanie książek (tytuł, data wydania – rosnąco/malejąco)
- wyszukiwanie książek po tytule i autorze
- szczegóły książki (`/books/:id`)
- wypożyczanie i zwrot konkretnego egzemplarza książki (zmiana jego dostępności)
- lista wypożyczeń po terminie

## Architektura komponentów

- **AppComponent** – główny komponent aplikacji. Zawiera pasek nawigacji i `router-outlet`, w którym wyświetlają się pozostałe widoki.
- **NavbarComponent** – menu z linkami do książek, czytelników i wypożyczeń.
- **BookListComponent** (`/books`) – lista książek z wyszukiwaniem i sortowaniem. Pobiera dane z `BookService`. Każdą książkę wyświetla w `BookCardComponent`, a sortowanie obsługuje `SortBarComponent`.
- **BookCardComponent** – karta jednej książki. Dostaje książkę od listy (`@Input book: Book`) i informuje listę o usunięciu (`@Output deleteBook: number` – id książki).
- **SortBarComponent** – przyciski sortowania. Przekazuje liście wybrany sposób sortowania (`@Output sortChange: string`).
- **BookDetailsComponent** (`/books/:id`) – szczegóły jednej książki: autor i egzemplarze. Bierze `id` z adresu i pobiera książkę z `BookService`.
- **BookFormComponent** (`/books/new`, `/books/:id/edit`) – formularz dodawania i edycji książki. Zapisuje przez `BookService`.
- **ReaderListComponent** (`/readers`) – lista czytelników z możliwością usuwania. Korzysta z `ReaderService`.
- **ReaderFormComponent** (`/readers/new`, `/readers/:id/edit`) – formularz dodawania i edycji czytelnika. Zapisuje przez `ReaderService`.
- **LoanListComponent** (`/loans`) – lista wypożyczeń z przyciskiem zwrotu, wypożyczenia po terminie są wyróżnione. Zwrot ustawia `returnDate` w wypożyczeniu (`LoanService`) i oznacza egzemplarz jako dostępny (`BookService`).
- **LoanFormComponent** (`/loans/new`) – formularz wypożyczenia: wybór czytelnika, książki i jednego z jej dostępnych egzemplarzy. Pobiera listy z `ReaderService` i `BookService`, zapisuje wypożyczenie przez `LoanService` i oznacza egzemplarz jako niedostępny (`BookService`).

Serwisy `BookService`, `ReaderService` i `LoanService` łączą się z json-server (`http://localhost:3000`).
