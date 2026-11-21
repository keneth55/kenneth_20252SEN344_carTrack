# Dataset Card: Used Car Price Prediction Dataset

Project: **CarTrack** (Used Car Sales Tracker) | Student: Kenneth, ID 20252SEN344 | Course: INSY 8311

## 1. Source and description

| Item | Details |
|---|---|
| **Name** | Used Car Price Prediction Dataset |
| **Source link** | https://www.kaggle.com/datasets/taeefnajib/used-car-price-prediction-dataset |
| **Platform** | Kaggle |
| **Licence** | CONFIRM ON THE KAGGLE PAGE (write the exact licence name here) |
| **File** | `data/raw/used_cars.csv` (stored exactly as downloaded, never edited) |
| **Format** | CSV, comma separated, with a header row |
| **Size** | 4,009 records, 12 columns, about 594 KB |
| **Date collected** | 21 November 2026 |
| **One record represents** | One used car listing: the car, its condition and its asking price |
| **Personal data** | None. There are no names, phone numbers or ID numbers |

## 2. Columns in the raw file

| Column | Example | Meaning |
|---|---|---|
| `brand` | Ford | Car brand |
| `model` | Mustang GT | Car model and trim |
| `model_year` | 2020 | Year of the car |
| `milage` | 51,000 mi. | Distance driven (spelled "milage" in the file) |
| `fuel_type` | Gasoline | Type of fuel |
| `engine` | 300.0HP 3.7L V6 Cylinder Engine | Engine description in one text |
| `transmission` | 6-Speed A/T | Gearbox type |
| `ext_col` | Black | Exterior colour |
| `int_col` | Gray | Interior colour |
| `accident` | None reported | Accident or damage history |
| `clean_title` | Yes | Whether the car has a clean title |
| `price` | $10,300 | Asking price |

## 3. Data quality check (counts only, nothing was fixed)

| # | Check | Count |
|---|---|---|
| 1 | Total records | 4,009 |
| 2 | Fully duplicated rows | 0 |
| 3 | Empty `fuel_type` | 170 |
| 4 | Empty `accident` | 113 |
| 5 | Empty `clean_title` | 596 |
| 6 | `fuel_type` filled with "–" | 45 |
| 7 | `fuel_type` filled with "not supported" | 2 |
| 8 | `engine` filled with "–" | 45 |
| 9 | `transmission` filled with "–" | 4 |
| 10 | `ext_col` filled with "–" | 15 |
| 11 | `int_col` filled with "–" | 133 |
| 12 | Records with at least one empty or placeholder value | 884 |
| 13 | `price` stored as text with "$" and commas (for example `$10,300`) | 4,009 |
| 14 | `milage` stored as text with commas and " mi." (for example `51,000 mi.`) | 4,009 |
| 15 | Brand cut short: "Alfa" (should be Alfa Romeo) | 19 |
| 16 | Brand cut short: "Aston" (should be Aston Martin) | 9 |
| 17 | Brand cut short: "Land" (should be Land Rover); the rest of the name sits at the start of `model` | 130 |
| 18 | Different spellings in `transmission` (for example "A/T" and "Automatic") | 62 distinct values |
| 19 | `ext_col` values that become the same after fixing case and spaces | 319 distinct, 317 after |
| 20 | `int_col` values that become the same after fixing case and spaces | 156 distinct, 152 after |
| 21 | `engine` records holding several facts in one text (horsepower) | 3,201 |
| 22 | `engine` records holding engine size in one text (litres) | 3,790 |
| 23 | Distinct brands / distinct brand + model pairs | 57 / 1,899 |

**Other facts:** `model_year` runs from 1974 to 2024 and every value is a valid 4-digit year. `price` runs from $2,000 to $2,954,083.

**Missing from the dataset:** there is no unique ID column, no date, and no seller, buyer or sale information.

## 4. Notes for later phases

- The cleaning of items 3 to 22 is planned for **Phase IV**.
- Sellers, buyers and sales are not in the dataset. They will be created as anonymised sample data in Phase IV and linked to real cars.
