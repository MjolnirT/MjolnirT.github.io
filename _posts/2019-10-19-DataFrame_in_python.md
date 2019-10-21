---
title: Data Structure in Python
data: 10-19-2019 9:30
categories:
- Python
---

# 1.DataFrame

DataFrame is a common data structure in pandas, and it can be multi dimensional instead of two like the list in dataset. Both row and column have index, thus it can be regard as a dictionary out of series. Each series is an entry of DataFrame.

pandas.DataFrame(data=,index=,columns=,dtype=,copy=False)

permutation in dataframe

```python
#first way
for row in df.iterrows():
	do_something
#second way
for i in range(len(df)):
    do_something
```



# 2.list

add an element to the list

```python
list.append('element') #add to the end
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