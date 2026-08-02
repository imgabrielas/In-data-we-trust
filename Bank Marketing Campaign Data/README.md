# Cleaning Bank Marketing Campaign Data

A data cleaning project (DataCamp) that reshapes a bank's personal loan marketing
campaign data into a clean, well-typed, relational structure ready to be loaded
into a PostgreSQL database.

## Background

Personal loans are a lucrative revenue stream for banks. A bank has run a
marketing campaign aimed at getting customers to take out a personal loan, and
plans to run more campaigns in the future. This project takes the raw export
from that campaign and cleans, reformats, and splits it into three tables that
conform to a fixed schema, so the bank can import this campaign — and future
ones — into a database with minimal friction.

## Input

- `bank_marketing.csv` — raw export from the marketing campaign (41,188 rows,
  16 columns): client demographics, campaign contact details, and economic
  indicators all in a single flat file, with inconsistent formatting
  (`"unknown"` values, `.` in category names, dates split across `day`/`month`
  columns with no year, etc.).

## Output

Cleaning splits the raw file into three normalized CSVs:

### `client.csv`

| column | type | notes |
|---|---|---|
| `client_id` | `integer` | |
| `age` | `integer` | |
| `job` | `object` | `.` removed (e.g. `admin.` → `admin`) |
| `marital` | `object` | |
| `education` | `object` | `.` → `_`; `"unknown"` → `NaN` |
| `credit_default` | `bool` | `"yes"` → `True`, else `False` |
| `mortgage` | `bool` | `"yes"` → `True`, else `False` |

### `campaign.csv`

| column | type | notes |
|---|---|---|
| `client_id` | `integer` | |
| `number_contacts` | `integer` | contacts made during this campaign |
| `contact_duration` | `integer` | seconds |
| `previous_campaign_contacts` | `integer` | contacts made during the previous campaign |
| `previous_outcome` | `bool` | `"success"` → `True`, else `False` |
| `campaign_outcome` | `bool` | `"yes"` → `True`, else `False` |
| `last_contact_date` | `datetime` (`YYYY-MM-DD`) | built from `day` + `month`, assuming year `2022` |

### `economics.csv`

| column | type | notes |
|---|---|---|
| `client_id` | `integer` | |
| `cons_price_idx` | `float` | monthly consumer price index |
| `euribor_three_months` | `float` | daily 3-month Euribor rate |

## Project structure

```
.
├── bank_marketing.csv   # raw input data
├── notebook.ipynb       # cleaning and splitting logic
├── client.csv           # output: client demographics
├── campaign.csv         # output: campaign contact data
├── economics.csv        # output: economic indicators
└── piggy_bank.jpg
```

## Usage

Open `notebook.ipynb` and run all cells. It reads `bank_marketing.csv` and
writes `client.csv`, `campaign.csv`, and `economics.csv` in the project root.

Requires `pandas` and `numpy`.

```bash
pip install pandas numpy
jupyter notebook notebook.ipynb
```