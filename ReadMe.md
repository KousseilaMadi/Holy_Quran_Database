# Quran Database Structure

## Overview
`holy_quran.db` is a SQLite database containing the complete text of the Holy Quran in multiple languages with chapter metadata.

---

## Database Tables

### 1. `chapters` Table
Stores metadata about each chapter (Surah) of the Quran.

| Column Name        | Data Type | Description                                      |
|-------------------|-----------|--------------------------------------------------|
| `chapter_id`      | INTEGER   | Primary key. Chapter number (1-114)              |
| `name_arabic`     | TEXT      | Arabic name of the chapter (e.g., "الفاتحة")     |
| `name_translit`   | TEXT      | Transliterated name (e.g., "Al-Fatihah")        |
| `verse_count`     | INTEGER   | Total number of verses in the chapter            |
| `revelation_place`| TEXT      | Where revealed: "Makkah" or "Madinah"           |

**Example Row:**
```
chapter_id: 1
name_arabic: الفاتحة
name_translit: Al-Fatihah
verse_count: 7
revelation_place: Makkah
```

---

### 2. `verses` Table
Stores the actual text of each verse in multiple languages.

| Column Name       | Data Type | Description                                      |
|------------------|-----------|--------------------------------------------------|
| `chapter_id`     | INTEGER   | Foreign key referencing `chapters.chapter_id`    |
| `verse_number`   | INTEGER   | Verse number within the chapter                  |
| `text_arabic`    | TEXT      | Arabic text of the verse                         |
| `text_english`   | TEXT      | English translation of the verse                 |
| `text_bahasa`    | TEXT      | Bahasa Indonesia translation of the verse        |
| `text_translit`  | TEXT      | English transliteration of the Arabic text       |

**Primary Key:** `(chapter_id, verse_number)` - Composite primary key ensures each verse is unique

**Foreign Key:** `chapter_id` references `chapters(chapter_id)`

**Example Row:**
```
chapter_id: 1
verse_number: 1
text_arabic: بِسْمِ اللَّهِ الرَّحْمَٰنِ الرَّحِيمِ
text_english: In the name of Allah, the Entirely Merciful, the Especially Merciful.
text_bahasa: Dengan nama Allah Yang Maha Pengasih, Maha Penyayang.
text_translit: Bismillah ir-Rahman ir-Rahim
```

---

## Relationships
```
chapters (1) ----< (many) verses
```

- One chapter has many verses
- Each verse belongs to exactly one chapter
- Verses reference chapters via `chapter_id`

---

## Database Statistics

- **Total Chapters:** 114
- **Total Verses:** 6,236
- **Makki Chapters:** 86 (revealed in Makkah)
- **Madani Chapters:** 28 (revealed in Madinah)
- **Languages:** Arabic, English, Bahasa Indonesia, Transliteration

---

## Data Sources

- **Arabic Text:** Original Quranic text
- **English Translation:** [Specify translation used, e.g., "Sahih International"]
- **Bahasa Translation:** [Specify translation used]
- **Transliteration:** English phonetic representation

---

## License & Attribution

The Quranic text and translations used in this database are subject to their respective licenses. Please ensure proper attribution when using this data.

[Add specific license information for your translations here]

---

## Notes

- This is a **read-only reference database** - should not be modified by users
- Verse numbering follows standard Quranic verse numbering
- All text is stored as UTF-8 to properly handle Arabic and special characters

---

## File Information

- **File Name:** `holy_quran.db`
- **Format:** SQLite 3
- **Size:** ~5.01 MB
- **Encoding:** UTF-8

---

## Schema Diagram
```
┌─────────────────────────┐
│      chapters           │
├─────────────────────────┤
│ + chapter_id (PK)       │
│ - name_arabic           │
│ - name_english          │
│ - name_translit         │
│ - verse_count           │
│ - revelation_place      │
└───────────┬─────────────┘
            │
            │ 1:N
            │
┌───────────┴─────────────┐
│      verses             │
├─────────────────────────┤
│ + chapter_id (PK, FK)   │
│ + verse_number (PK)     │
│ - text_arabic           │
│ - text_english          │
│ - text_bahasa           │
│ - text_translit         │
└─────────────────────────┘
```
