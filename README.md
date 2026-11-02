# Task 24 – Data Type Cleaning (Employee Dataset)

**Veda Technology · AI & ML Track · Level 1 · Day 24**

## Objective
Identify incorrect data types in a dataset and convert the columns to appropriate types, to understand why correct data types matter for data analysis and machine learning.

## Tools Used
- Python
- Pandas
- Jupyter Notebook

## Dataset
A small Employee dataset (created in code) with 4 records and 7 columns. Numbers, dates and boolean values were deliberately stored as text, and some values were invalid (`"abc"`, `"N/A"`, `"50,000"`), to mimic messy real-world data.

## Files in this Repository
| File | Description |
|------|-------------|
| `data_type_cleaning.ipynb` | Notebook with inspection and conversion code |
| `employee_cleaned.csv` | Dataset after data type correction |
| `README.md` | Approach and explanation |

## 1. Original Data Types

Checked with `df.dtypes`:

| Column | Original dtype | Problem |
|--------|----------------|---------|
| emp_id | object | Numeric IDs stored as text |
| name | object | Correct |
| age | object | Numbers stored as text, contains invalid value `"abc"` |
| salary | object | Contains commas (`"50,000"`) and `"N/A"` |
| join_date | object | Dates stored as text |
| department | object | Repeated labels, better as category |
| is_active | object | `"True"`/`"False"` stored as text |

## 2. Conversions Applied

```python
df["emp_id"] = df["emp_id"].astype(int)
df["age"] = pd.to_numeric(df["age"], errors="coerce")
df["salary"] = pd.to_numeric(df["salary"].str.replace(",", ""), errors="coerce")
df["join_date"] = pd.to_datetime(df["join_date"])
df["department"] = df["department"].astype("category")
df["is_active"] = df["is_active"].map({"True": True, "False": False})
```

## 3. Corrected Data Types

| Column | Corrected dtype |
|--------|-----------------|
| emp_id | int64 |
| name | object (string) |
| age | float64 |
| salary | float64 |
| join_date | datetime64[ns] |
| department | category |
| is_active | bool |

## 4. Explanation of the Conversions

- **emp_id → `int`:** All values were clean whole numbers, so a direct `astype(int)` is safe.
- **age → `pd.to_numeric(errors="coerce")`:** The value `"abc"` cannot be a number. `errors="coerce"` turns it into `NaN` instead of crashing, so it can be handled later as a missing value. The column becomes float because `NaN` cannot be stored in an integer column.
- **salary → remove commas, then `to_numeric`:** Text like `"50,000"` cannot be converted directly. After removing commas, valid values become numbers and `"N/A"` becomes `NaN`.
- **join_date → `to_datetime`:** Enables date operations such as extracting year or month and calculating experience.
- **department → `category`:** The column has few repeated labels, so the category type saves memory and signals that it is categorical.
- **is_active → bool:** True/False flags should be boolean, not strings.

## 5. Verification
After conversion, `df.dtypes` and `df.info()` were checked to confirm every column has the intended type. The result is saved as `employee_cleaned.csv`.

## 6. Why Correct Data Types Matter
- Calculations (sum, mean, min, max) and sorting only work correctly on numeric types. As text, `"100"` sorts before `"20"`.
- Machine learning models need numeric input.
- Proper types reduce memory use and enable type-specific features (datetime and category operations).
- Invalid values get detected early instead of silently breaking the analysis.

## Interview Questions – Key Answers
1. **What is a data type?** It defines what kind of values a column stores (int, float, string, datetime, bool) and what operations are allowed on it.
2. **Why should numerical data have a numerical data type?** Mathematical operations, statistics, scaling and ML models work only on numbers. Text-based numbers cause errors or wrong results.
3. **How can you convert a column to numeric?** Use `astype(int/float)` for clean data, or `pd.to_numeric(col, errors="coerce")` when invalid values may exist.

## How to Run
```bash
pip install pandas jupyter
jupyter notebook data_type_cleaning.ipynb
```
