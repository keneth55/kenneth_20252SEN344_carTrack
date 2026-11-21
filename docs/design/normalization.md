# Normalization: CarTrack (Used Car Sales Tracker)

Student: Kenneth, ID 20252SEN344 | Course: INSY 8311 | Phase II, Logical Design

The raw file is `data/raw/used_cars.csv` (4,009 records, 12 columns). The examples below use real records from this file. The record number is the position of the row in the file (record 1 is the first row after the header).

---

## 1. Entities identified (Task 3)

The raw file is one wide table. Repeated values become separate tables, and the process from Phase I adds three more.

| # | Entity | Why it is a separate table |
|---|---|---|
| 1 | `BRAND` | The same brand name is repeated in many rows (Ford appears 386 times). |
| 2 | `MODEL` | A model belongs to one brand, so it depends on the brand, not on the car. |
| 3 | `FUEL_TYPE` | The same fuel text is repeated (Gasoline appears 3,309 times). |
| 4 | `TRANSMISSION` | The same gearbox text is repeated in thousands of rows. |
| 5 | `COLOR` | Exterior and interior colours use the same list of colours. |
| 6 | `CAR` | One row for each car (one raw record). |
| 7 | `SELLER` | From the business process: a seller brings the car. Not in the raw file. |
| 8 | `BUYER` | From the business process: a buyer buys the car. Not in the raw file. |
| 9 | `SALE` | From the business process: one sale for one car and one buyer. Not in the raw file. |

There is no many-to-many relationship, so no junction table is needed.

---

## 2. Normalization step by step (Task 4)

### 2.1 UNF (unnormalized form)

This is the raw file as downloaded. It has no key, and several values mix a number with text.

| Rec | brand | model | model_year | milage | fuel_type | engine | transmission | ext_col | int_col | accident | clean_title | price |
|---|---|---|---|---|---|---|---|---|---|---|---|---|
| 1 | Ford | Utility Police Interceptor Base | 2013 | 51,000 mi. | E85 Flex Fuel | 300.0HP 3.7L V6 Cylinder Engine Flex Fuel Capability | 6-Speed A/T | Black | Black | At least 1 accident or damage reported | Yes | $10,300 |
| 2 | Hyundai | Palisade SEL | 2021 | 34,742 mi. | Gasoline | 3.8L V6 24V GDI DOHC | 8-Speed Automatic | Moonlight Cloud | Gray | At least 1 accident or damage reported | Yes | $38,005 |
| 3 | Lexus | RX 350 RX 350 | 2022 | 22,372 mi. | Gasoline | 3.5 Liter DOHC | Automatic | Blue | Black | None reported | (empty) | $54,598 |
| 11 | Land | Rover Range Rover Sport 3.0 Supercharged HST | 2021 | 27,608 mi. | Gasoline | V6 | Automatic | Fuji White | Pimento / Ebony | None reported | (empty) | $73,897 |
| 24 | Ford | F-150 XLT | 2023 | 2,823 mi. | Gasoline | 3.5L V6 24V PDI DOHC Twin Turbo | Automatic | Iconic Silver Metallic | Black | None reported | Yes | $58,504 |

**Problems in UNF:**

- No column identifies a car.
- `milage` and `price` hold a number mixed with text ("51,000 mi.", "$10,300").
- `brand` is cut short in some rows (Land, with "Rover" at the start of `model`).
- The same brand, fuel, gearbox and colour text is repeated in many rows.

### 2.2 First normal form (1NF)

**What changes:** a key is added and every column holds one single value.

| car_id | brand | model | model_year | mileage | fuel_type | engine | transmission | ext_col | int_col | accident | clean_title | price |
|---|---|---|---|---|---|---|---|---|---|---|---|---|
| 1 | Ford | Utility Police Interceptor Base | 2013 | 51000 | E85 Flex Fuel | 300.0HP 3.7L V6 Cylinder Engine Flex Fuel Capability | 6-Speed A/T | Black | Black | At least 1 accident or damage reported | Yes | 10300 |
| 2 | Hyundai | Palisade SEL | 2021 | 34742 | Gasoline | 3.8L V6 24V GDI DOHC | 8-Speed Automatic | Moonlight Cloud | Gray | At least 1 accident or damage reported | Yes | 38005 |
| 3 | Lexus | RX 350 RX 350 | 2022 | 22372 | Gasoline | 3.5 Liter DOHC | Automatic | Blue | Black | None reported | NULL | 54598 |
| 11 | Land Rover | Range Rover Sport 3.0 Supercharged HST | 2021 | 27608 | Gasoline | V6 | Automatic | Fuji White | Pimento / Ebony | None reported | NULL | 73897 |
| 24 | Ford | F-150 XLT | 2023 | 2823 | Gasoline | 3.5L V6 24V PDI DOHC Twin Turbo | Automatic | Iconic Silver Metallic | Black | None reported | Yes | 58504 |

**Key:** `car_id` (new column, because the raw file has no ID).

**Dependency removed:** a value that mixed a number and text in one column (`milage`, `price`) now holds only the number, and the cut-short brand "Land" is joined with "Rover" from `model`. The file has no repeating list of values in one cell, so there is no repeating group to split. The `engine` text is kept as one descriptive column.

### 2.3 Second normal form (2NF)

**What changes:** nothing needs to be split.

2NF removes partial dependencies, which can only exist when the key has two or more columns. The key of this table is the single column `car_id`, so every other column depends on the whole key. **The table is already in 2NF.**

**Dependency removed:** none. There is no partial dependency.

### 2.4 Third normal form (3NF)

**What changes:** columns that depend on another non-key column, or that repeat the same text many times, move to their own tables.

| Split | Reason (dependency removed) |
|---|---|
| `BRAND` and `MODEL` split from `CAR` | **Transitive dependency:** `brand` depends on `model` (a model always belongs to one brand), and `model` depends on `car_id`. So `car_id` → `model` → `brand`. The brand moves to `BRAND`, and `CAR` keeps only the model. |
| `FUEL_TYPE` split from `CAR` | **Repeated value:** the same fuel text is repeated in thousands of rows, so it moves to a lookup table. |
| `TRANSMISSION` split from `CAR` | **Repeated value:** the same gearbox text is repeated in thousands of rows, so it moves to a lookup table. |
| `COLOR` split from `CAR` | **Repeated value:** `ext_col` and `int_col` use the same colours, so both point to one `COLOR` table. |
| `SELLER` split from `CAR` | **Transitive dependency:** the seller's name, phone and location depend on the seller, not on the car (`car_id` → `seller_id` → seller details). |
| `BUYER` and `SALE` | **Transitive dependency:** the buyer's details depend on the buyer, and the sale date and price depend on the sale, not on the car. |
| Price category not stored | **Transitive dependency avoided:** the category depends on `price`, so a PL/SQL function calculates it instead of storing it. |

**Resulting tables (real data from the five records above):**

`BRAND`

| brand_id | brand_name |
|---|---|
| 1 | Ford |
| 2 | Hyundai |
| 3 | Lexus |
| 4 | Land Rover |

`MODEL`

| model_id | brand_id | model_name |
|---|---|---|
| 1 | 1 | Utility Police Interceptor Base |
| 2 | 2 | Palisade SEL |
| 3 | 3 | RX 350 RX 350 |
| 4 | 4 | Range Rover Sport 3.0 Supercharged HST |
| 5 | 1 | F-150 XLT |

`FUEL_TYPE`

| fuel_type_id | fuel_name |
|---|---|
| 1 | E85 Flex Fuel |
| 2 | Gasoline |

`TRANSMISSION`

| transmission_id | transmission_name |
|---|---|
| 1 | 6-Speed A/T |
| 2 | 8-Speed Automatic |
| 3 | Automatic |

`COLOR`

| color_id | color_name |
|---|---|
| 1 | Black |
| 2 | Moonlight Cloud |
| 3 | Gray |
| 4 | Blue |
| 5 | Fuji White |
| 6 | Pimento / Ebony |
| 7 | Iconic Silver Metallic |

`CAR` (the columns `seller_id` and `status` are added in Phase IV)

| car_id | model_id | fuel_type_id | transmission_id | exterior_color_id | interior_color_id | model_year | mileage | engine_description | accident_history | clean_title | price |
|---|---|---|---|---|---|---|---|---|---|---|---|
| 1 | 1 | 1 | 1 | 1 | 1 | 2013 | 51000 | 300.0HP 3.7L V6 Cylinder Engine Flex Fuel Capability | At least 1 accident or damage reported | Yes | 10300 |
| 2 | 2 | 2 | 2 | 2 | 3 | 2021 | 34742 | 3.8L V6 24V GDI DOHC | At least 1 accident or damage reported | Yes | 38005 |
| 3 | 3 | 2 | 3 | 4 | 1 | 2022 | 22372 | 3.5 Liter DOHC | None reported | NULL | 54598 |
| 11 | 4 | 2 | 3 | 5 | 6 | 2021 | 27608 | V6 | None reported | NULL | 73897 |
| 24 | 5 | 2 | 3 | 7 | 1 | 2023 | 2823 | 3.5L V6 24V PDI DOHC Twin Turbo | None reported | Yes | 58504 |

`SELLER`, `BUYER` and `SALE` are not in the raw file. Their columns are in `data_dictionary.md`, and their rows are created as anonymised sample data in Phase IV.

**Check:** Ford now appears once in `BRAND` and is used by two models. Every non-key column depends on the key, the whole key and nothing but the key, so the design is in 3NF.

---

## 3. Mapping of every raw field (Task 5)

No raw field is dropped.

| Raw field | Target table.column | Conversion needed in Phase IV |
|---|---|---|
| `brand` | `BRAND.brand_name` | Fix names cut short: "Land" to Land Rover (130 rows), "Alfa" to Alfa Romeo (19), "Aston" to Aston Martin (9). Move the missing word from the start of `model`. |
| `model` | `MODEL.model_name` | Remove the part moved to `brand`. Text stays text. Link to `BRAND` by `brand_id`. |
| `model_year` | `CAR.model_year` | Text to NUMBER. |
| `milage` | `CAR.mileage` | Remove commas and " mi.", then text to NUMBER (4,009 rows). |
| `fuel_type` | `FUEL_TYPE.fuel_name`, linked by `CAR.fuel_type_id` | Empty, "–" and "not supported" values (217 rows) become NULL. |
| `engine` | `CAR.engine_description` | "–" (45 rows) becomes NULL. Text stays text. |
| `transmission` | `TRANSMISSION.transmission_name`, linked by `CAR.transmission_id` | "–" (4 rows) becomes NULL. Make the spellings consistent (62 values). |
| `ext_col` | `COLOR.color_name`, linked by `CAR.exterior_color_id` | "–" (15 rows) becomes NULL. Fix case and spaces (319 values become 317). |
| `int_col` | `COLOR.color_name`, linked by `CAR.interior_color_id` | "–" (133 rows) becomes NULL. Fix case and spaces (156 values become 152). |
| `accident` | `CAR.accident_history` | Empty (113 rows) becomes NULL. |
| `clean_title` | `CAR.clean_title` | Empty (596 rows) becomes NULL. |
| `price` | `CAR.price` | Remove "$" and commas, then text to NUMBER (4,009 rows). |

**Columns and tables that are not in the raw file (generated):** `car_id`, `CAR.seller_id`, `CAR.status`, and the tables `SELLER`, `BUYER` and `SALE`.

---

## 4. Class demonstration: where one record ends up

Record 1 of the raw file:

| Raw value | Ends up in |
|---|---|
| Ford | `BRAND`: brand_id 1, brand_name Ford |
| Utility Police Interceptor Base | `MODEL`: model_id 1, linked to brand_id 1 |
| 2013 | `CAR.model_year` |
| 51,000 mi. | `CAR.mileage` = 51000 |
| E85 Flex Fuel | `FUEL_TYPE`: fuel_type_id 1, linked by `CAR.fuel_type_id` |
| 300.0HP 3.7L V6 Cylinder Engine Flex Fuel Capability | `CAR.engine_description` |
| 6-Speed A/T | `TRANSMISSION`: transmission_id 1, linked by `CAR.transmission_id` |
| Black (exterior) | `COLOR`: color_id 1, linked by `CAR.exterior_color_id` |
| Black (interior) | `COLOR`: color_id 1, linked by `CAR.interior_color_id` |
| At least 1 accident or damage reported | `CAR.accident_history` |
| Yes | `CAR.clean_title` |
| $10,300 | `CAR.price` = 10300 |
