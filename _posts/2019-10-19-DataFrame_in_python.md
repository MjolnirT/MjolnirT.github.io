---
title: Data Structure in Python
data: 10-19-2019 9:30
categories:
- Python
---

# 1.DataFrame

DataFrame is a common data structure in pandas, and it can be multi dimensional instead of two like the list in dataset. Both row and column have index, thus it can be regard as a dictionary out of series. Each series is an entry of DataFrame.

pandas.DataFrame(data=,index=,columns=,dtype=,copy=False)



# 2.list



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