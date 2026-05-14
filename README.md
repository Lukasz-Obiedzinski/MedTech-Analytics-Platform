# 🏥 Projekt: MedTech-Analytics-Platform (End-to-End)

> [!INFO] Cel projektu Budowa kompleksowej platformy analitycznej dla fikcyjnej firmy medycznej. Projekt demonstruje pełny proces: od generowania "brudnych" danych biznesowych (Python), przez modelowanie (SQL), po zaawansowane czyszczenie (Power Query) i wizualizację finansową (Power BI).

---
## 🏗️ Architektura Rozwiązania

1. **Generowanie danych (Python):** Symulacja systemów ERP/SAP z błędami.
2. **Magazynowanie (PostgreSQL/SSMS):** Relacyjna baza danych (Model Gwiazdy).
3. **ETL & Cleaning (Power Query):** Obsługa błędów, Query Folding, integracja z API NBP.
4. **Wizualizacja (Power BI):** Dashboardy finansowe, Aging Report, Analiza MPK.
---

## 📊 Model Danych (Star Schema)

### 1. Tabele Faktów (Fact Tables)

- **Fact_Invoice_Header** (Nagłówki)
    - `Invoice_ID` (Klucz główny, format: `FS/YYYY/MM/####`)
    - `Posting_Date` (Data księgowania - zawiera błędy!)
    - `Due_Date` (Termin płatności)
    - `Customer_ID` (Klucz obcy)
    - `Currency` (PLN, EUR, USD)
    - `Sales_Rep_ID` (Handlowiec)
    - `Total_Header_Discount` (%)
    - `Payment_Status` (Paid/Unpaid)
    - `Payment_Date` (Do analizy Aging)
- **Fact_Invoice_Lines** (Pozycje)
    
    - `Line_ID`, `Invoice_ID` (Klucz łączący), `Product_ID`, `Quantity`, `Unit_Price`, `Line_Discount`, `MPK_ID`.

### 2. Tabele Wymiarów (Dimension Tables)
- `Dim_Customers`: Dane teleadresowe, segmentacja.
- `Dim_Products`: Nazwy, kategorie produktów medycznych.
- `Dim_MPK`: Struktura kosztów (Dział, Pion, Manager).
- `Dim_Date`: Kalendarz (Time Intelligence).
- `Exchange_Rates`: Pobierane dynamicznie z NBP API (Power Query).
---

## 🐍 Logika Generowania Danych (Python)

### Strategia "Dirty Data" (Symulacja błędów)

Aby projekt był autentyczny, dane muszą zawierać błędy do naprawienia w Power Query:
1. **Błędy logiczne:** Daty typu `31.02.2023` lub `31.04.2023`.
2. **Błędy formatu:** Zamiana miejscami dnia z miesiącem (np. `05.12.2023` zamiast `12.05.2023`).
3. **Błędy techniczne:** Wartości `NULL`, `brak`, `00.00.0000`, różne separatory (`/`, `.`, `-`).
4. **Błędy biznesowe:** `Due_Date` wcześniejszy niż `Posting_Date`, duplikaty numerów faktur.
### Algorytm Generatora

- [ ] Stworzenie listy bazowej dat (iloczyn kartezjański list `days`, `months`, `years`).
- [ ] Dodanie do listy dat elementów "brudnych".
- [ ] Pętla generująca nagłówki (wykorzystanie `random.choice` do dat i klientów).
- [ ] Pętla generująca pozycje (1-N pozycji dla każdego nagłówka).
- [ ] Export do `.csv` lub `.xlsx`.
---

## 🛠️ Stack Techniczny & Zadania

### SQL (PostgreSQL)
- [ ] Stworzenie schematu bazy danych.
- [ ] Import plików CSV.
- [ ] Napisanie zapytań weryfikujących (np. suma sprzedaży per klient, lista faktur bez pozycji).
### Power Query (M)
- [ ] **Data Cleansing:** Transformacja "brudnych" dat przy użyciu `Try... Otherwise`.
- [ ] **Query Folding:** Zrzucenie ciężaru filtrowania na bazę SQL (optymalizacja).
- [ ] **API NBP:** Dynamiczne pobieranie kursów walut dla dat księgowania.
### Power BI (DAX)
- [ ] **Miary:** Total Sales, Sales YoY, Average Payment Delay, Margin %.
- [ ] **Aging Report:** Podział faktur na koszyki (0-30 dni, 31-60, itd.).
- [ ] **Visuals:** Waterfall chart dla kosztów MPK, Treemap dla produktów.
---
## 📝 Notatki do obrony projektu (Rekrutacja)

- **Dlaczego Python do danych?** "Chciałem mieć pełną kontrolę nad strukturą błędów, aby zaprezentować umiejętności czyszczenia danych (Data Cleansing)."
- **Dlaczego Query Folding?** "W realnych systemach (jak SAP) dane są ogromne. Optymalizacja zapytania na poziomie źródła jest kluczowa dla wydajności raportu."
- **Dlaczego MPK?** "Analiza samej sprzedaży to połowa sukcesu. Controlling kosztów na MPK to realna potrzeba biznesowa w firmach produkcyjnych."

---

## 📅 Harmonogram (Checklista)

- [x] Koncepcja i struktura tabel.
- [ ] Skrypt Python: Generator bazy dat (czystych + brudnych).
- [ ] Skrypt Python: Generator Headerów i Lines.
- [ ] Postawienie bazy PostgreSQL i załadowanie danych.
- [ ] Połączenie Power BI -> SQL.
- [ ] Budowa modelu i dashboardu.
- [ ] README na GitHubie.