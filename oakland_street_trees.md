

```python
#imports libraries and csv file
import pandas as pd
df = pd.read_csv(('oakland-street-trees.csv'), index_col=False)
```

## Summary
Data from https://www.kaggle.com/cityofoakland/oakland-street-trees

## Assess
#### Data Quality
- change column names to lower case

#### Data Tidiness
- make column for latitude and longituge
- make genus column
- drop unecessary columns

## Clean

### Change columns to lowercase


```python
#changes column names to lower case
df.columns = df.columns.str.lower()
```

### Latitude/Longitude


```python
#extracts lat/long into separate columns and drops unnecessary columns
df['latitude_1'] = df['location 1'].str.extract('(\'\d\d\.\d\d\d\d\d\d\d\d\d\d\d\d\d\d)', expand=True)
df['latitude'] = df['latitude_1'].str.extract('(\d\d\.\d\d\d\d\d\d\d\d\d\d\d\d\d\d)', expand=True)
df['longitude'] = df['location 1'].str.extract('(-\d\d\d\.\d\d\d\d\d\d\d\d\d\d\d\d\d\d)', expand=True)
df.drop(columns={'latitude_1', 'location 1', 'stname'}, inplace=True)
```

### Fixing species columns
Only names not latin are: bonsai, fruit, palm, dead/stump/shrub


```python
#changes names to lower case and extracts genus from species column
df['species'] = df['species'].str.lower()
df['species'] = df['species'].str.replace(' sp', '')
df['genus'] = df['species'].str.extract('([a-z]\w{0,})')
#changes common for latin names
df['genus'] = df['genus'].str.replace('walnut', 'juglans')
df['genus'] = df['genus'].str.replace('fig', 'ficus')
df['genus'] = df['genus'].str.replace('banana', 'musa')
df['genus'] = df['genus'].str.replace('apricot', 'prunus')
df['genus'] = df['genus'].str.replace('almond', 'prunus')
df['genus'] = df['genus'].str.replace('tbd', 'NaN')
df['genus'] = df['genus'].str.replace('other', 'NaN')
df['genus'] = df['genus'].str.replace('unknown', 'NaN')
#drops species column
df.drop(columns={'species'}, inplace=True)
```

    /anaconda3/lib/python3.6/site-packages/ipykernel/__main__.py:4: FutureWarning: currently extract(expand=None) means expand=False (return Index/Series/DataFrame) but in a future version of pandas this will be changed to expand=True (return DataFrame)


## Save to CSV


```python
df.to_csv('oakland_street_trees.csv')
```

## References
- https://www.kaggle.com/cityofoakland/oakland-street-trees
- https://stackoverflow.com/questions/19726029/how-can-i-make-pandas-dataframe-column-headers-all-lowercase
- https://chrisalbon.com/python/data_wrangling/pandas_regex_to_create_columns/
- https://www.wikipedia.org
