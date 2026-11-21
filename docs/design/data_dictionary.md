# Data Dictionary: CarTrack (Used Car Sales Tracker)

Student: Kenneth, ID 20252SEN344 | Course: INSY 8311 | Phase II, Logical Design

Oracle data types and sizes are left out on purpose. They are added in Phase IV.

**Key to the columns**

- **Key:** PK = primary key, FK = foreign key.
- **Required:** Yes means the column cannot be empty. No means it can be empty (NULL).
- **Unique:** Yes means no two rows can hold the same value.
- **Source:** the raw field in `used_cars.csv`, or "Generated" for columns that are not in the dataset.

---

## 1. Tables at a glance

| # | Table | Purpose | Source of the data |
|---|---|---|---|
| 1 | `BRAND` | Car brands | Raw field `brand` |
| 2 | `MODEL` | Car models, each linked to one brand | Raw field `model` |
| 3 | `FUEL_TYPE` | Types of fuel | Raw field `fuel_type` |
| 4 | `TRANSMISSION` | Types of gearbox | Raw field `transmission` |
| 5 | `COLOR` | Colours used for the outside and inside of cars | Raw fields `ext_col`, `int_col` |
| 6 | `SELLER` | People or companies that bring cars to the dealer | Generated sample data (anonymised) |
| 7 | `BUYER` | People or companies that buy cars | Generated sample data (anonymised) |
| 8 | `CAR` | Each car in the dealer's stock | Raw file, one row per record |
| 9 | `SALE` | Each sale of a car to a buyer | Generated sample data |

---

## 2. Table descriptions

### 2.1 BRAND

| Column | Meaning | Key | Required | Unique | Source |
|---|---|---|---|---|---|
| `brand_id` | Number that identifies a brand | PK | Yes | Yes | Generated |
| `brand_name` | Name of the brand, for example Ford | | Yes | Yes | `brand` |

### 2.2 MODEL

| Column | Meaning | Key | Required | Unique | Source |
|---|---|---|---|---|---|
| `model_id` | Number that identifies a model | PK | Yes | Yes | Generated |
| `brand_id` | Brand that makes this model | FK to `BRAND` | Yes | No | `brand` |
| `model_name` | Name and trim of the model, for example Mustang GT | | Yes | Yes together with `brand_id` | `model` |

### 2.3 FUEL_TYPE

| Column | Meaning | Key | Required | Unique | Source |
|---|---|---|---|---|---|
| `fuel_type_id` | Number that identifies a fuel type | PK | Yes | Yes | Generated |
| `fuel_name` | Name of the fuel, for example Gasoline or Hybrid | | Yes | Yes | `fuel_type` |

### 2.4 TRANSMISSION

| Column | Meaning | Key | Required | Unique | Source |
|---|---|---|---|---|---|
| `transmission_id` | Number that identifies a gearbox type | PK | Yes | Yes | Generated |
| `transmission_name` | Name of the gearbox, for example 6-Speed A/T | | Yes | Yes | `transmission` |

### 2.5 COLOR

| Column | Meaning | Key | Required | Unique | Source |
|---|---|---|---|---|---|
| `color_id` | Number that identifies a colour | PK | Yes | Yes | Generated |
| `color_name` | Name of the colour, for example Black | | Yes | Yes | `ext_col`, `int_col` |

### 2.6 SELLER

| Column | Meaning | Key | Required | Unique | Source |
|---|---|---|---|---|---|
| `seller_id` | Number that identifies a seller | PK | Yes | Yes | Generated |
| `seller_name` | Name of the seller (sample name, not a real person) | | Yes | No | Generated |
| `phone` | Phone number of the seller (sample number) | | No | No | Generated |
| `location` | Town or city of the seller | | No | No | Generated |

### 2.7 BUYER

| Column | Meaning | Key | Required | Unique | Source |
|---|---|---|---|---|---|
| `buyer_id` | Number that identifies a buyer | PK | Yes | Yes | Generated |
| `buyer_name` | Name of the buyer (sample name, not a real person) | | Yes | No | Generated |
| `phone` | Phone number of the buyer (sample number) | | No | No | Generated |
| `location` | Town or city of the buyer | | No | No | Generated |

### 2.8 CAR

| Column | Meaning | Key | Required | Unique | Source |
|---|---|---|---|---|---|
| `car_id` | Number that identifies a car | PK | Yes | Yes | Generated |
| `model_id` | Model of the car (the brand is found through the model) | FK to `MODEL` | Yes | No | `model` |
| `seller_id` | Seller who brought the car | FK to `SELLER` | Yes | No | Generated |
| `fuel_type_id` | Fuel used by the car | FK to `FUEL_TYPE` | No | No | `fuel_type` |
| `transmission_id` | Gearbox of the car | FK to `TRANSMISSION` | No | No | `transmission` |
| `exterior_color_id` | Colour outside the car | FK to `COLOR` | No | No | `ext_col` |
| `interior_color_id` | Colour inside the car | FK to `COLOR` | No | No | `int_col` |
| `model_year` | Year the car was made | | Yes | No | `model_year` |
| `mileage` | Distance driven, in miles | | Yes | No | `milage` |
| `engine_description` | Engine details as one text (power, size, cylinders) | | No | No | `engine` |
| `accident_history` | Whether an accident or damage was reported | | No | No | `accident` |
| `clean_title` | Whether the car has a clean title | | No | No | `clean_title` |
| `price` | Asking price of the car in dollars | | Yes | No | `price` |
| `status` | Available or Sold (starts as Available) | | Yes | No | Generated |

### 2.9 SALE

| Column | Meaning | Key | Required | Unique | Source |
|---|---|---|---|---|---|
| `sale_id` | Number that identifies a sale | PK | Yes | Yes | Generated |
| `car_id` | Car that was sold | FK to `CAR` | Yes | Yes | Generated |
| `buyer_id` | Buyer who bought the car | FK to `BUYER` | Yes | No | Generated |
| `sale_date` | Date of the sale | | Yes | No | Generated |
| `sale_price` | Price agreed with the buyer, in dollars | | Yes | No | Generated |

`car_id` is unique in `SALE`, so one car can be sold only once.

---

## 3. Relationships

| # | Relationship | Cardinality | Optionality |
|---|---|---|---|
| 1 | `BRAND` to `MODEL` | One to many | A brand has at least one model. A model belongs to exactly one brand. |
| 2 | `MODEL` to `CAR` | One to many | A model can have no cars or many cars. A car has exactly one model. |
| 3 | `SELLER` to `CAR` | One to many | A seller can have no cars or many cars. A car has exactly one seller. |
| 4 | `FUEL_TYPE` to `CAR` | One to many | A fuel type can have many cars. A car has zero or one fuel type. |
| 5 | `TRANSMISSION` to `CAR` | One to many | A transmission can have many cars. A car has zero or one transmission. |
| 6 | `COLOR` to `CAR` (exterior) | One to many | A colour can be the outside colour of many cars. A car has zero or one exterior colour. |
| 7 | `COLOR` to `CAR` (interior) | One to many | A colour can be the inside colour of many cars. A car has zero or one interior colour. |
| 8 | `CAR` to `SALE` | One to zero or one | A car is sold at most once. A sale is for exactly one car. |
| 9 | `BUYER` to `SALE` | One to many | A buyer can have no sales or many sales. A sale has exactly one buyer. |

There is no many-to-many relationship, so no junction table is needed.

---

## 4. Design decisions

- **Brand is not stored in `CAR`.** The brand is found through the model (`CAR` → `MODEL` → `BRAND`). This removes a transitive dependency.
- **Seller is stored in `CAR`, not in `SALE`.** The seller is known from the car, so repeating it in `SALE` would be redundant.
- **Price category is not stored.** It depends on the price, so saving it would be a transitive dependency. A PL/SQL function will calculate it from `price`.
- **Optional columns:** `fuel_type_id`, `transmission_id`, `exterior_color_id`, `interior_color_id`, `engine_description`, `accident_history` and `clean_title` have empty or "–" values in the raw file, so they are not required.
- **Cleaning for Phase IV:** `mileage` and `price` are text in the raw file and become numbers. Empty values and "–" become NULL. Brand names cut short ("Land", "Alfa", "Aston") are joined with the start of `model` to get the full brand.
