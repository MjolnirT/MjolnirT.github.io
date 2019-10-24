---
title: Data Structure in Python
data: 10-19-2019 9:30
categories:
- Python
---

# 1.DataFrame

DataFrame is a common data structure in pandas, and it can be multi dimensional instead of two like the list in dataset. Both row and column have index, thus it can be regard as a dictionary out of series. Each series is an entry of DataFrame.

### pandas.DataFrame(data=,index=,columns=,dtype=,copy=False)

```python
df = pd.DataFrame(data=, columns=, index=)#index is the label in the vertical way
```

### permutation in dataframe

```python
#first way
for row in df.iterrows():
	do_something
#second way
for i in range(len(df)):
    do_something
```

### add entry

```Python
df#the dataframe you need to operate
df.loc[('a', i),:]=None#adding index the the ith line
new_df = df.append(data)#we must have stated the data(dataframe) before appeding to the exist df
```

### delete entry

```python
#filtering
df[df['a']<10]
df[df['a'].notnull()]
df[df['a'].isnull()]
#deleting
df.drop(df.index[i])#delete ith entry
c = []#put the index of entries which need to be deleted into a list c
df.drop(df.index[c])
df.drop(df.index[i:])#delete i continuous entries
#delete columns
df.drop['column_name', axis=1,inplace=False]#default set is False, if we want to change in the original dataset, the inplace paremeter should be set as True
```

### select entry

```python
data.loc[data['c']=='one']#selecting the entry whose value of key c is one
data.loc[data['b'].isin(['1','3'])]
data.loc[(data['a']=='m')&(data['d']=='m')]#seletring the entry whose value of key a and b are both m
data.loc[~data['c'].isin(['one','two'])]
```

### getting back 

```python
df.isnull().sum(axis=1)#returning the total number of rows whose value it is NaN
df.isnull().sum(axis=0)#returning the total number of columns whose value it is NaN
df.count()#returning the total number of columns whose value is not null
df.count(axis=1)#returning the total number of rows whose value is not null
```



# 2.list

### add an element to the list

```python
list.append('element') #add to the end
```

### select list by rows

```python
n = list_data[1:3] #select certain piece of python: 1 to 3 th line, except the first line 0
o = list_data.loc[1]
u = list_data.loc[[1,3]]
list_data_drop = list_data.drop([1,2], axis=0)#delete rows but not change the index of others
```

### select list by columns

```python
q = list_data['column_name']
q = list_data[['column_name_1','column_name_2']]
r = list_data[[0,1,2]]
x = list_data[list(range(5))]
```



# 3.Disctionary

# 4.other trick

### view a glance of the data

```python
#python 3.x
head,*tail = data
head 
```

```python
#for list file
list[:n] #show the first n arrays
```

```python
#for dictionary
dict(list(dict_data)[:n])
```

### drop() function

```python
df[df.isnull()] #geting a bool serires
df[df.notnull()]

df.dropna()
df.dropna(axis=1, thresh=3) #deleting three terms in column whose values are NaN
df.dropna(how='ALL')  #deleting the row whose values are all NaN
```

inplace parameter

```
DF = DF.drop('column_name', axis=1)；
DF.drop('column_name',axis=1, inplace=True)
DF.drop([DF.columns[[0,1, 3]]], axis=1, inplace=True)   # Note: zero indexed
```

Those commands have changed the original data, df, with the inplace parameter having been set True.

### regular expression

```python
import re

txt = "here is the text need to be matched"
x = re.search("^here.*matched", txt) #judge whether txt can be matched or not
x = re.findall("ed", txt) #return all matched result

```

