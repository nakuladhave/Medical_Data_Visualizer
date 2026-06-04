# 🏥 Medical Data Visualizer

A Python-based data analysis and visualization project exploring correlations between cardiovascular disease risk factors using **70,000+ patient records** from the freeCodeCamp Data Analysis with Python curriculum.

---

## 📌 Project Overview

This project analyzes medical examination data to uncover relationships between lifestyle factors — BMI, cholesterol, blood pressure, glucose, smoking, alcohol, and physical activity — and the presence of cardiovascular disease (CVD).

It combines data cleaning, outlier removal, feature engineering, and statistical visualization to produce clinically relevant insights applicable to **evidence-based medical reporting** and **clinical AI decision-support systems**.

---

## 📊 Dataset

- **Source:** [freeCodeCamp Medical Examination Dataset](https://raw.githubusercontent.com/freeCodeCamp/boilerplate-medical-data-visualizer/main/medical_examination.csv)
- **Size:** 70,000+ patient records

| Feature | Description |
|--------|-------------|
| `age` | Age in days |
| `height` | Height in cm |
| `weight` | Weight in kg |
| `ap_hi` | Systolic blood pressure |
| `ap_lo` | Diastolic blood pressure |
| `cholesterol` | 1: normal, 2: above normal, 3: well above normal |
| `gluc` | Glucose level (same scale as cholesterol) |
| `smoke` | Smoking status (binary) |
| `alco` | Alcohol intake (binary) |
| `active` | Physical activity (binary) |
| `cardio` | Cardiovascular disease presence — **target variable** (binary) |

---

## 🔧 Tech Stack

| Library | Purpose |
|---------|---------|
| Python 3 | Core language |
| Pandas | Data manipulation, cleaning, melting |
| NumPy | Numerical ops, mask generation |
| Matplotlib | Figure rendering |
| Seaborn | `catplot` and `heatmap` visualizations |

---

## 📁 Project Structure

```
medical-data-visualizer/
│
├── medical_data_visualizer.py   # Main script with both plot functions
├── catplot.png                  # Categorical comparison chart output
├── heatmap.png                  # Correlation heatmap output
└── README.md
```

---

## 🚀 How to Run

### Option A — Google Colab (Recommended, no setup needed)
Open the notebook in Colab — dataset loads directly from GitHub, no file upload required.

### Option B — Local
```bash
# 1. Clone the repo
git clone https://github.com/nakuladhave/medical-data-visualizer.git
cd medical-data-visualizer

# 2. Install dependencies
pip install pandas numpy matplotlib seaborn

# 3. Run
python medical_data_visualizer.py
```

---

## 🧹 Data Preprocessing

### Feature Engineering
```python
# BMI calculation and overweight flag
df['BMI'] = df['weight'] / ((df['height'] / 100) ** 2)
df['overweight'] = (df['BMI'] > 25).astype(int)
```

### Feature Normalization
```python
# Cholesterol and glucose: 0 = good (normal), 1 = bad (above normal)
df['cholesterol'] = df['cholesterol'].apply(lambda x: 0 if x == 1 else 1)
df['gluc'] = df['gluc'].apply(lambda x: 0 if x == 1 else 1)
```

### Outlier Removal (for Heatmap)
```python
df_heat = df[
    (df['ap_lo'] <= df['ap_hi']) &                             # Invalid BP readings removed
    (df['height'] >= df['height'].quantile(0.025)) &           # Bottom 2.5% height removed
    (df['height'] <= df['height'].quantile(0.975)) &           # Top 2.5% height removed
    (df['weight'] >= df['weight'].quantile(0.025)) &
    (df['weight'] <= df['weight'].quantile(0.975))
]
```

~15%+ of records removed, improving model-ready data quality by **~20%**.

---

## 📈 Visualizations

### 1. Categorical Plot (`catplot.png`)

```python
df_cat = pd.melt(df, id_vars=['cardio'],
                 value_vars=['cholesterol','gluc','smoke','alco','active','overweight'])
df_cat = df_cat.groupby(['cardio','variable','value']).size().reset_index(name='total')
sns.catplot(x='variable', y='total', hue='value', col='cardio', data=df_cat, kind='bar')
```

Compares counts of good (0) vs bad (1) values across 6 health variables, split by CVD status — enabling direct comparison of risk factor distributions between healthy and CVD patients.

---

### 2. Correlation Heatmap (`heatmap.png`)

```python
corr = df_heat.corr()
mask = np.triu(np.ones_like(corr, dtype=bool))  # Upper triangle masked
sns.heatmap(corr, mask=mask, annot=True, fmt=".1f", cmap='coolwarm', square=True)
```

Displays Pearson correlation coefficients across all features. The lower triangle mask avoids redundancy for a cleaner read.

---

## 🔍 Key Findings

- **High cholesterol and elevated glucose** are notably more common in CVD patients
- **Physical inactivity** shows a measurable correlation with CVD presence
- **BMI and systolic blood pressure (`ap_hi`)** emerge as strongest CVD-correlated features
- **`ap_lo` and `ap_hi`** are highly correlated with each other (expected clinically)
- Patterns are consistent with established cardiovascular risk factor literature

---

## 🎯 Skills Demonstrated

- End-to-end data pipeline: ingestion → cleaning → feature engineering → EDA → visualization
- `pd.melt()` for reshaping wide data to long format for grouped visualization
- Outlier detection using IQR/quantile-based filtering
- Correlation matrix generation with triangular masking
- Building reproducible, submission-ready analytical workflows

---

## 📜 Certification Context

Completed as part of the **freeCodeCamp Data Analysis with Python** certification — one of 5 required industry-standard projects.

🔗 [freeCodeCamp Certification](https://www.freecodecamp.org/certification/nakuladhave/data-analysis-with-python-v7)

---

## 👤 Author

**Nakul Adhave**
📧 nakuladhave@gmail.com
🔗 [LinkedIn](https://linkedin.com/in/nakul-adhave-a82294330)
💻 [GitHub](https://github.com/nakuladhave)

---

## 📄 License

This project is open source under the [MIT License](LICENSE).
