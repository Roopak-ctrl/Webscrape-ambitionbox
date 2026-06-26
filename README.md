# AmbitionBox Companies Dataset — Web Scraper

A Python web scraper that collects company-level data (ratings, reviews, jobs, interviews, and salary listing counts) from [AmbitionBox](https://www.ambitionbox.com/list-of-companies), compiled into a clean dataset of **5,460 companies**.

📊 Dataset also published on Kaggle: [ambitionbox-dataset](https://www.kaggle.com/datasets/roopak2205/ambitionbox-dataset)

## Overview
AmbitionBox lists company review data across hundreds of paginated pages, with no public API. This project scrapes the listing pages directly, parses each company's card for its key stats, and consolidates everything into a single structured CSV — useful for analyzing company popularity, review volume, and hiring activity across industries.

## Tech Stack
- **Python**
- **Requests** — fetching raw HTML from each page
- **BeautifulSoup4** (with the `lxml` parser) — parsing and extracting data from the HTML
- **Pandas** — structuring and exporting the final dataset
- **Jupyter Notebook** — development environment

## How It Works
1. Iterates through ~500 paginated listing pages on AmbitionBox.
2. For each page, fetches the HTML with a custom `User-Agent` header and parses it with BeautifulSoup.
3. Locates each company's card (`companyCardWrapper`) and extracts:
   - Company name
   - Star rating
   - Review count, job count, interview count, and salary listing count (matched by label text, since these counts share a common CSS class and are distinguished only by an adjacent title span)
4. Wraps each extraction in a `try/except` block so a single malformed card doesn't break the whole run — missing fields are recorded as `None` instead of crashing.
5. Appends each page's results into a running DataFrame and exports the final combined dataset to CSV.

## Dataset
| Column | Description |
|---|---|
| `Name` | Company name |
| `Rating` | Average employee rating (out of 5) |
| `Reviews` | Number of employee reviews |
| `Jobs` | Number of job listings |
| `Interviews` | Number of interview experiences shared |
| `Salaries` | Number of salary entries shared |

**5,460 rows** across **6 columns**. Note that `Reviews`, `Jobs`, `Interviews`, and `Salaries` are stored as scraped (e.g. `"1.1L"`, `"67.8k"`) using India's lakh/thousand shorthand, rather than as plain integers.

## Repository Structure
- `webscrapping.ipynb` — the scraping notebook (data collection + export)
- `ambitionbox.csv` — the resulting dataset

## Setup & Usage
```bash
pip install pandas requests beautifulsoup4 lxml
```
Open `webscrapping.ipynb` in Jupyter and run all cells. The scrape takes a while since it sequentially requests ~500 pages — expect it to run for several minutes.

## Future Improvements
- Convert the `k`/`L` shorthand columns into proper numeric types for direct analysis
- Add a rate limit / delay between requests to be gentler on the target server
- Add an exploratory analysis notebook (top-rated companies, rating distribution by review volume, etc.)

## Links
- 🔗 [Kaggle Dataset](https://www.kaggle.com/datasets/roopak2205/ambitionbox-dataset)
- 💻 [GitHub Repository](https://github.com/Roopak-ctrl/Webscrape-ambitionbox)
