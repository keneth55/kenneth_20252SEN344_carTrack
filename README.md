# kenneth_20252SEN344_carTrack
<div align="center">

# CarTrack

### Used Car Sales Tracker

*An Oracle database with PL/SQL that records used cars, sellers, buyers and sales in one place.*
## Project Information

| | |
|---|---|
| **Student** | Kenneth |
| **Student ID** | 20252SEN344 |
| **University** | Adventist University of Central Africa (AUCA) |
| **Faculty** | Faculty of Information Technology |
| **Course** | INSY 8311, Database Development with PL/SQL |
| **Instructor** | Eric Maniraguha |
| **Project field** | Business and Entrepreneurship |
| **Keywords** | Used cars, Sales tracking, Oracle PL/SQL |

---

## Contents

1. [Problem Statement](#problem-statement)
2. [Objectives](#objectives)
3. [Data Source](#data-source)
4. [Innovation](#innovation)
5. [Planned Tables](#planned-tables)
6. [Planned PL/SQL Features](#planned-plsql-features)
7. [Phase I – Business Process Modeling](#phase-i--business-process-modeling)
8. [Repository Structure](#repository-structure)
9. [References](#references)

---

## Problem Statement

Used car dealers often record their sales on paper or in scattered files. This causes lost records, wrong prices and the same car being sold twice. A dealer who cannot trust the records also cannot see which brands sell well or how prices change over time.

**CarTrack** solves this by keeping cars, sellers, buyers and sales in one Oracle database. PL/SQL procedures and triggers protect the data, and a dashboard helps the manager follow the sales.

## Objectives

**Main objective:** To build an Oracle database with PL/SQL that records and controls the sale of used cars.

**Specific objectives:**

- Store cars, sellers, buyers and sales in normalized (3NF) tables.
- Give every car a price category automatically.
- Prevent the sale of a car that is already sold.
- Give the manager clear reports on sales by brand and by year.

## Data Source

| | |
|---|---|
| **Dataset** | Used Car Price Prediction Dataset |
| **Platform** | Kaggle |
| **Link** | <https://www.kaggle.com/datasets/taeefnajib/used-car-price-prediction-dataset> |
| **Content** | About 4,000 used car listings with brand, model, year, mileage, fuel type, engine, transmission, accident history and price |
| **Personal data** | None |
| **Licence** | Educational use confirmed on the Kaggle page |

## Innovation

**Option:** Analytics dashboard
**Tool:** Power BI, connected live to the Oracle database

The dashboard will show total sales and revenue, sales by brand, sales by year, cars by price category, and filters for brand, fuel type and year. All business rules stay in PL/SQL. Power BI only shows the data.

## Planned Tables

| Table | Purpose |
|---|---|
| `BRAND` | Car brands |
| `CAR` | Cars with model, year, mileage, fuel, price, price category and status |
| `SELLER` | People or companies that bring cars |
| `BUYER` | People or companies that buy cars |
| `SALE` | Each sale with date, agreed price, car, seller and buyer |

An audit table will be added in a later phase.

## Planned PL/SQL Features

| Feature | Where it is used |
|---|---|
| **Function** | Returns the price category (Cheap, Medium or Expensive) of a car |
| **Procedure** | `record_sale` saves a sale and changes the car status to Sold |
| **Cursor** | Lists the available cars by brand |
| **Trigger** | Blocks the sale of a car that is already sold |
| **Package** | Groups the functions and procedures of CarTrack |
| **Exceptions** | Handle a missing car, a missing buyer or a car that is not available |

---

## Phase I – Business Process Modeling

### Selected business process

The **Used Car Sale Process** follows a used car from the moment a seller brings it to the dealer until it is sold to a buyer. The sales clerk registers the car, the database gives it a price category, and the clerk searches and records the sale when a buyer asks for a car. The database then marks the car as Sold, and the manager views the sales report.

**Actors:** Seller, Buyer, Sales Clerk, CarTrack DB, Manager.

**Decision point:** "Available?" If the car is available the sale continues. If not, the process ends.

### Modeling tool

**diagrams.net (draw.io)**

### Published diagram link

🔗 **<https://viewer.diagrams.net/?tags=%7B%7D&lightbox=1&highlight=0000ff&edit=_blank&layers=1&nav=1&title=Process%20Model.drawio&dark=auto#Uhttps%3A%2F%2Fdrive.google.com%2Fuc%3Fid%3D1c7MGW2wxypGu7CjK4bm-8mamcI4T4t5u%26export%3Ddownload#%7B%22pageId%22%3A%22fb6RJ7E-5GRBvwAPFzOb%22%7D>**

### Supporting documents

| Document | Link |
|---|---|
| Business process explanation | docs/business_process/business_process_explanation (1).pdf |
| Business process diagram | [business_process_diagram.pdf](docs/business_process/business_process_diagram.pdf) |

---

## Repository Structure

```
kenneth_20252SEN344_carTrack/
│
├── README.md
├── data/
└── docs/
    ├── research/
    ├── proposal/
    ├── presentation/
    └── business_process/
        ├── business_process_diagram.pdf
        └── business_process_explanation.pdf
```

## References

taeefnajib. (n.d.). *Used car price prediction dataset* [Data set]. Kaggle. https://www.kaggle.com/datasets/taeefnajib/used-car-price-prediction-dataset

*More references are listed in `docs/research/research_log.md`.*
