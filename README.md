# Programming Assignment 4: Data Wrangling and Visualization

**Student Name:** Basal, Earl David M.  
**Section:** 2ECE-B  
**Course:** Electronics Engineering / Computer Programming  
**Notebook File:** `PA4_Basal.ipynb`  

---

## 1. Executive Summary

This repository contains the Python implementation and documentation for **Programming Assignment 4 (PA4)**. The objective of this assignment is to apply data wrangling techniques using the `pandas` library in Python to extract, transform, and analyze academic performance records from an Excel dataset (`board2.xlsx`). 

The analysis focuses on slicing specific subsets of data based on geographic origin (Hometown), academic track, gender, and performance criteria (average scores across subjects).

---

## 2. Dataset Overview

The dataset used in this assignment is stored in `board2.xlsx`, which contains board exam performance metrics and demographic metadata for 30 students across 8 attributes:

| Column Name | Data Type | Description |
| :--- | :--- | :--- |
| `Name` | Object / String | Identifier code for each student (e.g., S1, S2, ...) |
| `Gender` | Object / String | Gender identity of the student (`Male`, `Female`) |
| `Track` | Object / String | ECE Specialization (`Instrumentation`, `Communication`, `Microelectronics`) |
| `Hometown` | Object / String | Geographic origin region (`Luzon`, `Visayas`, `Mindanao`) |
| `Math` | Integer | Numerical grade in Mathematics |
| `Electronics` | Integer | Numerical grade in Electronics |
| `GEAS` | Integer | Numerical grade in General Engineering & Applied Sciences |
| `Communication` | Integer | Numerical grade in Communications |

---

## 3. Required Dependencies & Environment

To execute the notebook, ensure you have Python 3.x installed along with the following required libraries:

* `pandas` – Data manipulation and DataFrame analysis
* `matplotlib` – Data visualization foundation
* `openpyxl` – Excel file reading engine for `pandas`

Install required packages via pip if necessary:
```bash
pip install pandas matplotlib openpyxl
```

---

## 4. Methodology & Code Explanation

The Jupyter Notebook is structured into data loading, preliminary summary statistics, and two distinct data filtering and restructuring tasks.

### Step 1: Library Import & Data Ingestion
The required libraries (`pandas` and `matplotlib.pyplot`) are loaded. The dataset `board2.xlsx` is imported into a Pandas DataFrame named `df`.

```python
import pandas as pd
import matplotlib.pyplot as plt

df = pd.read_excel('board2.xlsx')
```

### Step 2: Exploratory Data Analysis (EDA)
The `.describe()` method is invoked on `df` to generate descriptive statistics (count, mean, standard deviation, min, max, quartiles) for all four numerical subjects (`Math`, `Electronics`, `GEAS`, `Communication`).

```python
df.describe()
```

---

### Step 3: Data Wrangling Tasks

#### Task A: Visayas Communication DataFrame (`VisComm`)
**Objective:** Extract records for all students whose `Hometown` is **Visayas** and whose `Track` is **Communication**, calculate their overall average across all four subjects, and present selected columns.

1. **Calculated Column Creation:** A temporary copy of the dataset (`temp_df`) is generated to compute the row-wise mean score across `Math`, `Electronics`, `GEAS`, and `Communication` stored in a new `Average` column.
2. **Boolean Indexing:** Applied bitwise condition `(Hometown == 'Visayas') & (Track == 'Communication')`.
3. **Column Reordering & Selection:** Selected columns `['Name', 'Gender', 'Math', 'Electronics', 'Average']`.
4. **Row Count Verification:** Output length printed alongside the final DataFrame.

```python
# Create temporary copy and compute Average across 4 subjects
temp_df = df.copy()
temp_df['Average'] = temp_df[['Math', 'Electronics', 'GEAS', 'Communication']].mean(axis=1)

# Apply explicit filtering conditions
VisComm = temp_df[(temp_df['Hometown'] == 'Visayas') & (temp_df['Track'] == 'Communication')]

# Select required fields
VisComm = VisComm[['Name', 'Gender', 'Math', 'Electronics', 'Average']]

# Print output summary
print(f"Number of rows: {len(VisComm)}")
VisComm
```

**Results:** Sliced a subset of 5 student records meeting all conditional criteria.

---

#### Task B: Visayas Female DataFrame (`VisFemale`)
**Objective:** Extract female students (`Gender == 'Female'`) originating from **Visayas** (`Hometown == 'Visayas'`), retain specific attributes, and highlight those with an overall `Average` score of **60.0 or higher**.

1. **Filtering:** Applied logical condition `(Hometown == 'Visayas') & (Gender == 'Female')`.
2. **Attribute Selection:** Retained columns `['Name', 'Track', 'GEAS', 'Electronics', 'Average']`.
3. **Passing Grade Filter:** Queried `VisFemale[VisFemale['Average'] >= 60.0]` without mutating the underlying `VisFemale` table.

```python
# Compute subject average on DataFrame copy
temp_df = df.copy()
temp_df['Average'] = temp_df[['Math', 'Electronics', 'GEAS', 'Communication']].mean(axis=1)

# Filter for Visayas Female students
VisFemale = temp_df[(temp_df['Hometown'] == 'Visayas') & (temp_df['Gender'] == 'Female')]

# Retain required fields
VisFemale = VisFemale[['Name', 'Track', 'GEAS', 'Electronics', 'Average']]

# Display complete subset
print("Complete VisFemale DataFrame:")
display(VisFemale)

# Display subset filtering for Average >= 60.0
print("\nStudents with Average >= 60:")
display(VisFemale[VisFemale['Average'] >= 60.0])
```

**Results:** Identified 6 female students from Visayas, 4 of whom maintained an overall average grade of 60.0 or higher.

---

## 5. Execution Instructions

1. Place `PA4_Basal.ipynb` and `board2.xlsx` in the same working directory.
2. Launch Jupyter Notebook or VS Code with the Python Interactive extension.
3. Run all cells sequentially from top to bottom (`Kernel` -> `Restart & Run All`).

---

*Submitted in partial fulfillment of the requirements for PA4 - Data Wrangling and Visualization.*
