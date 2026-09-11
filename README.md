<img width="513" height="81" alt="image" src="https://github.com/user-attachments/assets/016a6d33-1ec3-4109-b474-5c110f59946d" /># RupaRupa-Featured-Products-Web-Scraping

A web scraper that extract discounted product data (name, pricing, discount %, rating, review count, product link, and image) from a real, live e-commerce promo page — [Ruparupa's 9.9 sale page] (https://www.ruparupa.com/ms/promo-9-9)

**Disclaimer** This project was built to practice web scraping skills, the scraped data is not sold, redistributed, or used for any commercial purpose.

## Why this is different from a typical scraping tutorial

Unlike from books.toscrape.com / quotes.toscrape.com which already in a clean and static state. This project scrapes **Rupa - Rupa website** - a **real, live commercial site** - which introduced problems a sandbox never would: 

**robots.txt, checked first** - confirmed the target page wasn't disallowed for scraping.
**JavaScript-rendered content** - the page uses lazy-loading, which a basic 'request.get()' couldn't retrieve all the product from the featured page. Confirmed by comparing the count in raw page vs the browser's live DOM.
**Selenium + headless Chrome** - used to actually render the page and scroll it
**Timed delays between scrolls** - pauses between each scrolls to avoid risking an IP block because too many request too quickly.
**Duplicate detection** - scrolling triggered some products to appear in more than one page section, duplicates were filtered out by product name
**Inconsistent/missing fields** - extraction handles missing/inconsistent value defensively, rather than assuming every field is always present.

## Methodology

1. **Checked `robots.txt`** - to confirm the target page could be scraped
2. **Loaded the page with Selenium** - since initial inspection showed the page relies on JavaScript to load additional products based on user scrolls
3. **Scrolled programmatically** - to trigger all lazy-loading content, then captured `driver.page_source` for the fully rendered HTML
4. **Parsed with BeautifulSoup** - targeting `div.row.product-card` as the repeating product container.
5. **Deduplicated** - Ensuring all the extracted data don't have any same products
6. **Extracted 8 fields per product** - product name, initial price, discount %, final price, rating, review, product url, and product image url - each with defensive handling for missing value.
7. **Exported to CSV** via pandas.

## Sample Output
This is a small sample of extracted data from Rupa-Rupa 
| name | initial_price | discount | final_price | rating | review |
|---|---|---|---|---|---|
| Krisbow Sync Smart Air Sterilizer 2-in-1... | Rp2.299.000 | 25% | Rp1.724.250 | 5 | 7(ulasan) |
| Informa Bronto Sofa Bed Fabric - Hijau | Rp4.299.000 | 20% | Rp3.399.000 | 5 | 41(ulasan) |
| Krisbow Masker KF94 Disposable... | *(none)* | *(none)* | Rp49.900 | 4.3 | 7(ulasan) |

## A note on scope and responsible use 
- This was a **one-time, small-scale scrape** for learning purpose, with delays (`time.sleep()`) between actions to avoid hammering the server.
- `robots.txt` was checked and confirmed before scraping.
- The **full scraped dataset is intentionally not published** in this repo - only a small sample, since the underlying data (real prices, live promo data) belongs to Ruparupa and is time-sensitive (tied to a specific sale event). The code itself is what's being showcased here, not the dataset.

## Tech used

- Python, `requests`, `BeautifulSoup4`, `pandas`
- `selenium` (headless Chrome) for JavaScript-rendered content
- Google Colab (development environment)
