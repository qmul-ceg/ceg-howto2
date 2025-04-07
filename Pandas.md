```python
import pandas as pd
df = pd.read_csv('csv_file', low_memory=False) 
# without a dtype for columns > warning. low _memory arg stops warning

#everything
df
# Top 5 rows
df.head()

# Output table of specific column_names
df[['column_name','column_name2']]

# Select index_number (row number) and output column_name value
df.at[index_number,'column_name']
>>> value
# Change the value of the specific cell
df.at[index_number,'column_name'] = 'new_value'

# Select index_number (row number) and column_index (column number) and output value
df.iat[2,1]
>>> value

# Select index_number (row number) and vertical output values
df.loc[index_number]
>>> column_name      value
>>> column_name2    value2
>>> column_name3    value3
>>> Name: index_number, dtype: data_type

# Select index_number (row number) and output column_name value
df.loc[index_number].column_name
>>> value

# Select rows whose column value matches some_value
# Returns 'True' for matching indexes, which can be expanded by an outer df.loc
df['column_name'] == some_value
df.loc[df['column_name'] == some_value]
>>> column_name    value
>>> Name: index_number, dtype: data_type

df.loc[df['column_name'] == some_value,['column_name2', 'column_name3']] 




# To select rows whose column value is in an iterable, some_values, use isin:
df.loc[df['column_name'].isin(some_values)]

# Combine multiple conditions with &:
df.loc[(df['column_name'] >= A) & (df['column_name'] <= B)]
# Note the parentheses.

# To select rows whose column value does not equal some_value, use !=:
df.loc[df['column_name'] != some_value]

# To select rows whose column value starts with "U"
df.loc[df['column_name'].str.startswith('U')]

# To select rows whose column value contains 2 numbers (regex)
df.loc[df['column_name'].str.contains(r'\d{2}')]
```


https://re-thought.com/how-to-change-or-update-a-cell-value-in-python-pandas-dataframe/

