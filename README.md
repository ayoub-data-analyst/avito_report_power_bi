# Avito Marché Immobilier Marocain — Power BI Report

This Power BI dashboard is the reporting and visualization layer of a larger end-to-end data project. It consumes the data produced by the [avito-end-to-end-data-pipeline](https://github.com/ayoub-data-analyst/avito-end-to-end-data-pipeline) — a fully containerized ETL pipeline that scrapes real estate listings from Avito.ma, cleans them, and loads them into a PostgreSQL data warehouse.

The report covers **3,403 property listings** across Moroccan cities and delivers interactive business intelligence across three analytical pages.

---

## Project Architecture

The full project spans two repositories:

```
[Avito.ma]
    |
    | Selenium scraper
    v
[Staging CSV]
    |
    | Pandas cleaning & enrichment
    v
[Clean CSV]
    |
    | SQLAlchemy loader
    v
[PostgreSQL]
    ├── bi_schema  (Star Schema)   <── Power BI connects here
    └── ml_schema  (OBT)
    |
    | Power BI Desktop
    v
[This report — 3 dashboard pages]
```

| Repository                                                                                          | Role                                        |
| --------------------------------------------------------------------------------------------------- | ------------------------------------------- |
| [avito-end-to-end-data-pipeline](https://github.com/ayoub-data-analyst/avito-end-to-end-data-pipeline) | Scraping, cleaning, and PostgreSQL loading  |
| avito_report_pb (this repo)                                                                         | Power BI dashboards on top of the warehouse |

---

## Dashboard Pages

### 1. Vue Globale du Marché Immobilier Marocain

Global overview of the Moroccan real estate market.

* KPI cards — total listings (3.4K), min/max prices (105K – 10M MAD), and average market price (1.17M MAD)
* Bar chart — ad distribution by city (Casablanca leads with 904 listings, followed by Marrakech at 444)
* Donut chart — price category segmentation (High 42%, Medium 36%, Low 22%)
* Slicers — filter by city, price category, surface area (m²), number of rooms, and number of bathrooms

### 2. Intelligence des Prix & Analyse Segmentée

Price intelligence and segmented analysis.

* KPI cards — average market price (1.17M MAD) and average price per m² (12.02K MAD)
* Histogram — price distribution across the market
* Bar chart — average price by city (Oualidia and Taghazout top the list at 4.6M and 4.3M MAD)
* Bar chart — average price by category (High: 1.97M, Medium: 0.72M, Low: 0.37M)

### 3. Intelligence Géographique Immobilière

Geospatial real estate intelligence.

* Interactive map — geographic distribution of average property prices across Morocco
* Horizontal bar chart — ranking of cities by average price (Oualidia 4.6M → Bouznika 1.1M)

---

## Project Structure

```
avito_report_pb/
├── power_bi/
│   └── avito_power_bi.pbix        # Main Power BI report file
├── dashboard_screenshots/
│   ├── dashboard-1.png            # Global Market Overview page
│   ├── dashboard-2.png            # Price Intelligence page
│   └── dashboard-3.png            # Geographic Intelligence page
└── README.md
```

---

## Getting Started

### Prerequisites

* Power BI Desktop (free) — [Download here](https://powerbi.microsoft.com/desktop/)
* A running instance of the data pipeline (see the [pipeline repo](https://github.com/ayoub-data-analyst/avito-end-to-end-data-pipeline)) to have up-to-date data in PostgreSQL, or use the sample data already embedded in the `.pbix` file.

### Opening the Report

1. Clone or download this repository.
2. Open Power BI Desktop.
3. Go to **File → Open report** and select `power_bi/avito_power_bi.pbix`.
4. Use the slicers on the left panel to filter by city, price category, surface, rooms, and bathrooms.

### Refreshing Data from the Pipeline

To refresh the report with fresh data:

1. Run the full ETL pipeline from the [pipeline repo](https://github.com/ayoub-data-analyst/avito-end-to-end-data-pipeline):
   ```bash
   cd dockerdocker-compose up --build
   ```
2. In Power BI Desktop, update the PostgreSQL connection string under  **Transform Data → Data source settings** .
3. Click **Refresh** to pull the latest data from `bi_schema`.

---

## Key Insights

| Metric              | Value                                 |
| ------------------- | ------------------------------------- |
| Total listings      | 3,403                                 |
| Average price       | 1.17M MAD                             |
| Average price / m² | 12,020 MAD                            |
| Price range         | 105K – 10M MAD                       |
| Most listings       | Casablanca (904)                      |
| Highest avg. price  | Oualidia (4.6M MAD)                   |
| Price categories    | High (42%) / Medium (36%) / Low (22%) |

---

## Data Source

Listings data sourced from [Avito.ma](https://www.avito.ma/) — Morocco's main real estate classifieds platform. Scraping, cleaning, and warehousing is handled by the [avito-end-to-end-data-pipeline](https://github.com/ayoub-data-analyst/avito-end-to-end-data-pipeline).

---

## License

This project is for educational and analytical purposes. Data belongs to Avito.ma.
