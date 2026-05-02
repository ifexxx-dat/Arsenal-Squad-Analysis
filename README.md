Arsenal FC Squad Analysis (Web Scraping, Data Cleaning & EDA)

A complete end-to-end data analytics project that scrapes real football player data from Transfermarkt, cleans it with pandas, and performs exploratory data analysis to uncover insights about Arsenal FC’s squad.

Project Structure

arsenal-squad-analysis/
├── data/
│   ├── arsenal_players.csv          # Raw scraped data
│   └── arsenal_players_clean.csv    # Cleaned data
├── notebooks/
│   ├── 01_scraping.ipynb            # Web scraping
│   ├── 02_cleaning.ipynb            # Data cleaning
│   └── 03_eda.ipynb                 # Exploratory data analysis
└── README.md


Tools & Libraries

- `requests` — fetching web pages
- `BeautifulSoup` — parsing HTML
- `pandas` — data manipulation and cleaning
- `matplotlib` & `seaborn` — data visualisation
- `Jupyter Notebook` — development environment


Part 1 — Web Scraping

**Target:** Transfermarkt Arsenal squad page (detailed view)

**Data collected:**

- Jersey number, name, position, nationality
- Date of birth, height, preferred foot
- Date joined, signed from, contract end date, market value

**Challenges & Solutions:**

|Challenge                                            |Solution                                                                  |
|-----------------------------------------------------|--------------------------------------------------------------------------|
|Site returning JavaScript instead of player data     |Added User-Agent header to simulate a real browser request                |
|Nationality stored as a flag image, not text         |Used `img['title']` attribute instead of `.text`                          |
|Wrong cell index mapping assumed without verification|Used `enumerate()` to print all cells with their indexes before extraction|
|Header and separator rows included in player loop    |Added `if len(cells) < 13: continue` to skip invalid rows                 |

**Result:** 25 players extracted across 10 columns, saved to CSV.

-----

## Part 2 — Data Cleaning

**Issues identified via `df.info()`:**

|Column              |Problem                             |Fix                                               |
|--------------------|------------------------------------|--------------------------------------------------|
|`market_value`      |`€` and `m` symbols, `-` for missing|`str.replace()` + `pd.to_numeric(errors='coerce')`|
|`dob`               |Date and age combined in one cell   |Regex extraction to split into two columns        |
|`height`            |Comma decimal separator + `m` unit  |`str.replace()` + `pd.to_numeric()` → height in cm|
|`joined`, `contract`|Dates stored as plain text          |`pd.to_datetime(errors='coerce')`                 |

**Key decisions:**

- Extracted age into a separate column instead of discarding it
- Height in cm (183) kept instead of metres (1.83) — more standard in football analytics
- Raw CSV preserved untouched; cleaned version saved separately
- All cleaning steps written in one reproducible cell

**Result:** 11 clean columns with correct dtypes ready for analysis.


Part 3 — Exploratory Data Analysis

Section 1: Squad Composition

**Age Distribution**
Most Arsenal players fall between 24-28 years old, confirming the club prioritises prime-age players over youth projects or aging veterans.

**Nationality Breakdown**
The squad contains players from 12 nationalities. England leads with 8 players, followed by Spain (5) and Brazil (3), reflecting strong scouting across Europe and South America.

**Foot Preference**
60% right-footed, 40% left-footed — an unusually balanced ratio compared to most football squads where left-footed players are significantly rarer.


Section 2: Market Value Analysis

**Top 5 Most Valuable Players**

|Player          |Market Value|
|----------------|------------|
|Declan Rice     |€120m       |
|Bukayo Saka     |€120m       |
|William Saliba  |€90m        |
|Martin Zubimendi|€80m        |
|Gabriel         |€75m        |

**Average Market Value by Position**
Central Midfielders command the highest average value at €75m. Goalkeepers have the lowest at €21m.

**Age vs Market Value**
Players aged 24-28 hold the highest market values. Values decline noticeably after 30, confirming the football market’s preference for prime-age players.

Section 3: Contract Analysis

**Contract Expiry Risk**
2028 has the highest number of expiring contracts (9 players), followed by 2030 (7 players). Only 1 contract expires in 2026, meaning minimal immediate squad disruption but a significant renewal challenge approaching in 2028.

-----

 Key Learnings

- Always inspect HTML structure manually before writing extraction code
- `errors='coerce'` is safer than `.astype()` on real-world messy data
- Regular expressions are powerful for extracting patterns from mixed-format strings
- Every chart needs an interpreted insight, not just a visual
- Write all cleaning steps in one reproducible block from the start
- Raw data should never be overwritten — always save a separate clean file


Author

Okoli Ifechukwu Chinwe
