# 📜 The Data Profiler
### *A Most Distinguished Compendium of Data Analysis & Exploration*

> *"In the grand tradition of empirical enquiry, one must first comprehend one's data before presuming to draw conclusions therefrom."*

---

## 🏛️ Table of Contents

- [🪶 Preface](#-preface)
- [⚙️ Prerequisites & Dependencies](#️-prerequisites--dependencies)
- [📂 Project Architecture](#-project-architecture)
- [📖 Part A — Fundamentals](#-part-a--fundamentals)
- [🗄️ Part B — Data Acquisition](#️-part-b--data-acquisition)
- [🔬 Part C — Data Understanding & Cleansing](#-part-c--data-understanding--cleansing)
- [📊 Part D — Exploratory Data Analysis](#-part-d--exploratory-data-analysis)
- [🗺️ Data Sources Employed](#️-data-sources-employed)
- [📜 Licence](#-licence)

---

## 🪶 Preface

Hearken well, dear scholar, for this repository doth present a thorough and methodical treatise upon the noble art of **Data Profiling**. Constructed with great diligence and scholarly vigour, this notebook undertaketh the complete lifecycle of data analysis — from the initial acquisition of raw data, through its thorough cleansing and examination, unto the production of grand visual representations and automated profiling reports.

This work hath been divided into four noble Parts, each building upon the last with the precision of a master craftsman. Whether thou art a novice apprentice or a seasoned practitioner of the data sciences, thou shalt find herein both instruction and enlightenment. 🎓

---

## ⚙️ Prerequisites & Dependencies

Before thou dost embark upon this scholarly endeavour, ensure that thine computing apparatus hath been furnished with the following libraries and packages of great repute:

```bash
pip install pandas numpy matplotlib seaborn requests ydata-profiling
```

| 📦 Package | 🎯 Purpose |
|---|---|
| `pandas` | The cornerstone of data manipulation and wrangling |
| `numpy` | Numerical computation and array operations |
| `matplotlib` | Low-level charting and figure construction |
| `seaborn` | High-level statistical visualisation |
| `requests` | Fetching data from remote APIs |
| `sqlite3` | Communing with local SQL databases *(standard library)* |
| `json` | Parsing and loading JSON data files *(standard library)* |
| `ydata-profiling` | Generating automated, comprehensive HTML profiling reports |

> 💡 **A Word of Counsel:** It is most strongly advisable to employ a virtual environment, lest thine global Python installation be subjected to unwanted perturbations.

---

## 📂 Project Architecture

```
Data-Profiler/
│
├── 📓 Data-Profiler.ipynb                        ← The primary notebook of this treatise
│
├── 📄 employees_practice.csv                     ← Employee data (CSV format)
├── 📄 employees_practice.json                    ← Employee data (JSON format, nested)
├── 📄 customers(from sql).csv                    ← Customer records (derived from SQL)
│
└── 📊 Generated Reports (upon execution)
    ├── Data-Profiler_employees_practice.html
    ├── Data-Profiler_employees_practice.json.html
    ├── Data-Profiler_employees.html
    └── Data-Profiler_users.html
```

---

## 📖 Part A — Fundamentals

### 🔍 What is Data Analysis?

Data Analysis is the honourable process of inspecting, cleansing, transforming, and modelling data so as to discover useful intelligence and support sound decision-making. It proceedeth through six noble stages:

| Stage | Description |
|---|---|
| 📥 Data Collection | Gathering raw data from surveys, databases, sensors, or APIs |
| 🧹 Data Cleaning | Remedying missing values, duplicates, and erroneous entries |
| 🔭 EDA | Employing statistics and visualisations to discern patterns |
| 🔄 Data Transformation | Normalising and reshaping data for analytical suitability |
| 🧮 Modelling | Applying statistical or machine-learning models |
| 📈 Reporting | Presenting findings through charts, dashboards, and reports |

The four **types** of analysis, arranged in ascending order of sophistication:

```
📊 Descriptive  →  🔎 Diagnostic  →  🔮 Predictive  →  🎯 Prescriptive
  "What happened?"   "Why did it?"   "What might?"   "What should we do?"
```

---

### 🗺️ Planning a Data Science Project

A well-ordered Data Science project doth follow six cardinal steps, each indispensable to the whole:

1. **🎯 Problem Definition** — Establish with clarity what one endeavours to predict or decide; vagueness at this juncture doth poison all subsequent work.
2. **📥 Data Collection Strategy** — Identify relevant sources, address legal and privacy concerns, and plan storage before a single model is trained.
3. **🔬 EDA + Data Engineering** — The true seat of labour; explore the data's true shape, cleanse it, engineer features, and build reproducible pipelines.
4. **🤖 Model Selection** — Begin with a humble baseline model, then benchmark and experiment with more sophisticated alternatives.
5. **⚖️ Evaluation** — Test rigorously on unseen data using pre-agreed metrics; obtain stakeholder approbation before proceeding.
6. **🚀 Deployment & Monitoring** — Serve the model and monitor for performance degradation as real-world data drifts with the passage of time.

---

### 🧮 Tensors — A Brief Exposition

A **Tensor** is, at its essence, a multi-dimensional array of numbers — a container that stores numerical data in an organised fashion across any number of dimensions.

```python
# 3D Tensor — a single colour image frame
frame = np.zeros((3, 1080, 1920), dtype=np.uint8)
# Shape: (colour_channels, height, width) → (R, G, B) × 1080 × 1920
```

| Dimensions | Name | Example |
|---|---|---|
| 0D | Scalar | A single temperature reading: `42.5` |
| 1D | Vector | A row of salaries: `[50k, 60k, 75k]` |
| 2D | Matrix | A spreadsheet of employee records |
| 3D | Tensor | A colour image (height × width × channels) |

---

## 🗄️ Part B — Data Acquisition

This notebook doth load data from **four distinct and honourable sources**, demonstrating admirable versatility in the art of data ingestion.

### 1️⃣ Loading from CSV

```python
Data1 = pd.read_csv("employees_practice.csv")
```

### 2️⃣ Loading from JSON *(with Nested Structure)*

```python
with open("employees_practice.json") as file:
    raw = json.load(file)

Data2 = pd.json_normalize(raw, record_path=['employees'])
```

> 🔖 **Scholarly Note:** `pd.json_normalize()` is employed here on account of the JSON's nested structure (e.g., `employees[].personal.gender`), which it doth flatten into convenient dot-notation columns such as `personal.gender`.

### 3️⃣ Loading from SQL *(via CSV Fallback)*

```python
# Intended approach — a SQLite query of great ambition:
conn = sqlite3.connect('retails.db')
df = pd.read_sql_query("SELECT * FROM retails WHERE amount > 40", conn)
conn.close()

# Practical resolution — the CSV substitute:
Data3 = pd.read_csv('customers(from sql).csv')
```

> ⚠️ **A Candid Admission:** The SQL database connection did encounter certain difficulties during development. The CSV transformation was therefore employed as a sound and pragmatic substitute.

### 4️⃣ Loading from a REST API

```python
url = 'https://dummyjson.com/users'
response = requests.get(url)
data = response.json()
Data4 = pd.DataFrame(data['users'])
```

---

## 🔬 Part C — Data Understanding & Cleansing

### 🧭 Initial Exploration

Upon loading each dataset, the following quartet of examinations is dutifully performed:

```python
DataFrame.head()      # Inspect the first few records
DataFrame.info()      # Survey data types and null counts
DataFrame.describe()  # Obtain summary statistics
DataFrame.isnull().sum()  # Count missing values per column
```

### 🧹 Data Cleansing Operations

#### Handling Missing Values

| Dataset | Treatment Applied |
|---|---|
| `Data1` | Filled with column mode (most frequent value) |
| `M_Data2` | Individual nested columns filled with respective modes |
| `Data3` | No action required — zero missing values |
| `Data4` | No action required — zero missing values |

#### Correcting Data Types

```python
Data1['salary'].astype(int)          # Salary cast from float to integer
Data3['customer_id'].astype(int)     # Customer ID cast to integer
# Data2 and Data4 — types already in good order, requiring no adjustment
```

#### Managing Duplicates

```python
Data1.drop_duplicates()    # Full row deduplication
Data3.drop_duplicates()    # Full row deduplication
# Data2 & Data4 — unhashable columns (containing lists/dicts) preclude standard deduplication
```

> 🔖 **Scholarly Note on Unhashable Data:** `Data2` and `Data4` contain columns with list or dictionary values, which Pandas is unable to hash for the purposes of duplicate detection. This is not an error, but rather an intrinsic characteristic of richly nested data.

---

## 📊 Part D — Exploratory Data Analysis

### 📏 Univariate Analysis *(Single Variable Examination)*

Each numerical variable is examined in isolation using a **three-plot combination**, rendered side by side:

```
📊 Histogram  +  〰️ KDE Curve  +  📦 Box Plot
```

| Plot Type | What It Revealeth |
|---|---|
| **Histogram** | Raw frequency distribution, organised into bins |
| **KDE** (Kernel Density Estimate) | A smooth probability density curve — a continuous estimate of the distribution |
| **Box Plot** | Median, interquartile range, and outliers at a glance |

Variables analysed across all four datasets include `age`, `salary`, `performance_score`, `compensation.salary`, `customer_id`, and `height`.

---

### 📐 Bivariate Analysis *(Two-Variable Relationships)*

Relationships between pairs of variables are examined thus:

| Technique | Variables | Insight Provided |
|---|---|---|
| **Box Plot** | Categorical × Numeric | Distribution of numeric variable across categories |
| **Violin Plot** | Categorical × Numeric | As box plot, enriched with full KDE shape |
| **Scatter Plot** | Numeric × Numeric | Raw relationship between two continuous variables |
| **Regression Plot** | Numeric × Numeric | Scatter augmented with a fitted trend line |

Notable analyses performed:
- 👤 **Gender vs Salary** — `Data1`
- 📅 **Experience vs Salary** — `Data1` and `Data2`
- 🎯 **Gender vs Performance Score** — `Data2`
- 🗺️ **State vs Signup Date** — `Data3`
- 📏 **Age vs Height** — `Data4`

---

### 🌐 Multivariate Analysis *(Multiple Variables Simultaneously)*

#### Correlation Heatmaps

```python
co_matrix = DataFrame.select_dtypes(include='number').corr()
sns.heatmap(co_matrix, annot=True, cmap='coolwarm', linewidths=0.5, fmt='.2f')
```

> Values range from **−1** to **+1**. The `coolwarm` palette renders positive correlations in warm red and negative correlations in cool blue, providing intuitive visual clarity.

#### Pair Plots

```python
sns.pairplot(DataFrame[selected_columns], hue='department', diag_kind='kde')
```

> A magnificent grid of scatter plots for every numeric pair, with KDE curves gracing the diagonal. The `hue` parameter colours points by categorical variable, permitting swift identification of group-level patterns.

---

### 🤖 Automated Profiling Reports *(The Crowning Achievement)*

The notebook concludes with the generation of **four comprehensive HTML profiling reports** via `ydata-profiling`, one for each dataset — a most efficient and thorough means of automated data intelligence:

```python
from ydata_profiling import ProfileReport

report = ProfileReport(Data1, title="Data Profiler Report")
report.to_file("Data-Profiler_employees_practice.html")

report = ProfileReport(M_Data2, title="Data Profiler Report")
report.to_file("Data-Profiler_employees_practice.json.html")

report = ProfileReport(Data3, title="Data Profiler Report")
report.to_file("Data-Profiler_employees.html")

report = ProfileReport(Data4, title="Data Profiler Report")
report.to_file("Data-Profiler_users.html")
```

Each report doth furnish, with admirable thoroughness, an overview of variable types, missing values, distributions, correlations, and a great many further insights — all rendered in a handsome, navigable HTML document.

---

## 🗺️ Data Sources Employed

| 🏷️ Reference | 📋 Format | 📝 Description |
|---|---|---|
| `Data1` | CSV | Employee records — age, salary, department, performance score |
| `Data2` | JSON (nested) | Employee data with nested personal, job & compensation fields |
| `Data3` | CSV (from SQL) | Customer records — state, signup date, customer ID |
| `Data4` | REST API | User profiles fetched from `dummyjson.com/users` |

---

## 🔑 Key Concepts Illustrated

```
🗄️ Multi-Source Data Loading     →   CSV · JSON · SQL · REST API
🧹 Data Cleaning & Imputation    →   fillna · astype · drop_duplicates
📊 Univariate Analysis           →   Histograms · KDE · Box Plots
📐 Bivariate Analysis            →   Scatter · Regression · Violin · Box
🌐 Multivariate Analysis         →   Correlation Heatmaps · Pair Plots
🤖 Automated Profiling           →   ydata-profiling HTML Reports
```

---

## 📜 Licence

This work is presented for educational purposes in the grand spirit of the open sharing of knowledge. Thou art most welcome to study, adapt, and build upon it — provided thou dost acknowledge its origins with the good grace befitting a scholar. 🎩

---

*Crafted with scholarly diligence and Pythonic precision* 🐍

*"Data is not information, information is not knowledge, knowledge is not understanding, understanding is not wisdom."*

</div>
