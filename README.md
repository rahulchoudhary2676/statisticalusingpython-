# statisticalusingpython-
no po 
import pandas as pd
import numpy as np

# Create the DataFrame with corrected syntax
details = {
    'Brand': pd.Series(['Nokia', 'Asus', 'Nokia', 'Nokia', 'Samsung', 'ABC', 'Micromax', 'Apple', 'MI', 'Zen', 'Apple']),
    'Price': pd.Series([10000, 8000, 12500, 7000, 40000, 12000, 12999, 13999, 59999, np.nan, np.nan]),  # Added missing values
    'Rating(10)': pd.Series([7, 6.5, 8.5, 9, 8, 9.5, 7, 9, np.nan, 8.5, 9.2])  # Added missing values
}

df = pd.DataFrame(details)
print("Original DataFrame:")
print(df)
print("\n" + "="*50 + "\n")

# 1. Identify missing values in the dataset
print("1. IDENTIFYING MISSING VALUES:")
print("Missing values count per column:")
print(df.isnull().sum())
print(f"\nTotal missing values: {df.isnull().sum().sum()}")
print("\nMissing values locations:")
print(df.isnull())
print("\n" + "="*50 + "\n")

# 2. Handle missing values - Impute or replace appropriately
print("2. HANDLING MISSING VALUES:")

# Create a copy for imputation
df_imputed = df.copy()

# For Price column: Replace missing values with median (robust to outliers)
price_median = df_imputed['Price'].median()
df_imputed['Price'].fillna(price_median, inplace=True)
print(f"Price missing values replaced with median: {price_median}")

# For Rating column: Replace missing values with mean
rating_mean = df_imputed['Rating(10)'].mean()
df_imputed['Rating(10)'].fillna(rating_mean, inplace=True)
print(f"Rating missing values replaced with mean: {rating_mean:.2f}")

print("\nDataFrame after handling missing values:")
print(df_imputed)
print("\n" + "="*50 + "\n")

# 3. Generate statistical summary of the data
print("3. STATISTICAL SUMMARY:")
print("Descriptive statistics for all columns:")
print(df_imputed.describe(include='all'))
print("\n" + "="*50 + "\n")

# 4. Compute column-wise mean, mode, and median
print("4. COLUMN-WISE STATISTICS:")

# For numerical columns
numerical_cols = ['Price', 'Rating(10)']
for col in numerical_cols:
    print(f"\n{col}:")
    print(f"  Mean: {df_imputed[col].mean():.2f}")
    print(f"  Median: {df_imputed[col].median():.2f}")
    print(f"  Mode: {df_imputed[col].mode().iloc[0]:.2f}")

# For categorical columns
categorical_cols = ['Brand']
for col in categorical_cols:
    print(f"\n{col}:")
    print(f"  Mode: {df_imputed[col].mode().iloc[0]}")
    print(f"  Unique values: {df_imputed[col].nunique()}")
    print(f"  Value counts:\n{df_imputed[col].value_counts()}")

print("\n" + "="*50 + "\n")

# 5. Sort dataset based on brand names alphabetically, using rating for reference
print("5. SORTED DATASET:")
print("Sorting by Brand (alphabetically), then by Rating(10) (descending):")

# Sort by Brand alphabetically, then by Rating descending
df_sorted = df_imputed.sort_values(['Brand', 'Rating(10)'], ascending=[True, False])
df_sorted = df_sorted.reset_index(drop=True)

print(df_sorted)
print("\n" + "="*50 + "\n")

# Additional analysis
print("6. ADDITIONAL INSIGHTS:")
print("Brand-wise statistics:")
brand_stats = df_imputed.groupby('Brand').agg({
    'Price': ['mean', 'min', 'max'],
    'Rating(10)': ['mean', 'min', 'max']
}).round(2)
print(brand_stats)

print(f"\nHighest rated brand: {df_imputed.loc[df_imputed['Rating(10)'].idxmax(), 'Brand']}")
print(f"Most expensive brand: {df_imputed.loc[df_imputed['Price'].idxmax(), 'Brand']}")
print(f"Best value (highest rating/price ratio): {df_imputed.loc[(df_imputed['Rating(10)'] / df_imputed['Price'] * 10000).idxmax(), 'Brand']}")