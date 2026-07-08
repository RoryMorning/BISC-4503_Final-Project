## Using Jupyter Notebooks (1 and 2)
This is just an introduction to using the Jupyter Notebooks to set up tables and create plots. 
```python
%matplotlib inline
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
sns.set(style = "darkgrid")
```

```python
df = pd.read_csv('/home/student/Desktop/classroom/myfiles/notebooks/fortune500.csv')
```

```python
df.head()
```


<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }
    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Year</th>
      <th>Rank</th>
      <th>Company</th>
      <th>Revenue (in millions)</th>
      <th>Profit (in millions)</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0</td>
      <td>1955</td>
      <td>1</td>
      <td>General Motors</td>
      <td>9823.5</td>
      <td>806</td>
    </tr>
    <tr>
      <td>1</td>
      <td>1955</td>
      <td>2</td>
      <td>Exxon Mobil</td>
      <td>5661.4</td>
      <td>584.8</td>
    </tr>
    <tr>
      <td>2</td>
      <td>1955</td>
      <td>3</td>
      <td>U.S. Steel</td>
      <td>3250.4</td>
      <td>195.4</td>
    </tr>
    <tr>
      <td>3</td>
      <td>1955</td>
      <td>4</td>
      <td>General Electric</td>
      <td>2959.1</td>
      <td>212.6</td>
    </tr>
    <tr>
      <td>4</td>
      <td>1955</td>
      <td>5</td>
      <td>Esmark</td>
      <td>2510.8</td>
      <td>19.1</td>
    </tr>
  </tbody>
</table>
</div>


```python
df.columns = ['year', 'rank', 'company', 'revenue', 'profit']
```

```python
df.head()
```


<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }
    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>year</th>
      <th>rank</th>
      <th>company</th>
      <th>revenue</th>
      <th>profit</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0</td>
      <td>1955</td>
      <td>1</td>
      <td>General Motors</td>
      <td>9823.5</td>
      <td>806</td>
    </tr>
    <tr>
      <td>1</td>
      <td>1955</td>
      <td>2</td>
      <td>Exxon Mobil</td>
      <td>5661.4</td>
      <td>584.8</td>
    </tr>
    <tr>
      <td>2</td>
      <td>1955</td>
      <td>3</td>
      <td>U.S. Steel</td>
      <td>3250.4</td>
      <td>195.4</td>
    </tr>
    <tr>
      <td>3</td>
      <td>1955</td>
      <td>4</td>
      <td>General Electric</td>
      <td>2959.1</td>
      <td>212.6</td>
    </tr>
    <tr>
      <td>4</td>
      <td>1955</td>
      <td>5</td>
      <td>Esmark</td>
      <td>2510.8</td>
      <td>19.1</td>
    </tr>
  </tbody>
</table>
</div>


```python
len(df)
```


    25500






```python
df.dtypes
```


    year         int64
    rank         int64
    company     object
    revenue    float64
    profit      object
    dtype: object






```python
non_numeric_profits = df.profit.str.contains('[^0-9.-]')
df.loc[non_numeric_profits].head()
```


<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }
    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>year</th>
      <th>rank</th>
      <th>company</th>
      <th>revenue</th>
      <th>profit</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>228</td>
      <td>1955</td>
      <td>229</td>
      <td>Norton</td>
      <td>135.0</td>
      <td>N.A.</td>
    </tr>
    <tr>
      <td>290</td>
      <td>1955</td>
      <td>291</td>
      <td>Schlitz Brewing</td>
      <td>100.0</td>
      <td>N.A.</td>
    </tr>
    <tr>
      <td>294</td>
      <td>1955</td>
      <td>295</td>
      <td>Pacific Vegetable Oil</td>
      <td>97.9</td>
      <td>N.A.</td>
    </tr>
    <tr>
      <td>296</td>
      <td>1955</td>
      <td>297</td>
      <td>Liebmann Breweries</td>
      <td>96.0</td>
      <td>N.A.</td>
    </tr>
    <tr>
      <td>352</td>
      <td>1955</td>
      <td>353</td>
      <td>Minneapolis-Moline</td>
      <td>77.4</td>
      <td>N.A.</td>
    </tr>
  </tbody>
</table>
</div>


```python
set(df.profit[non_numeric_profits])
```


    {'N.A.'}






```python
len(df.profit[non_numeric_profits])
```


    369






```python
bin_size, _, _ = plt.hist(df.year[non_numeric_profits], bins = range(1955, 2006))
```

![png](output_10_0.png)

```python
df  = df.loc[~non_numeric_profits].copy()
df['profit'] = pd.to_numeric(df['profit'])
```

```python
len(df)
```


    25131






```python
df.dtypes
```


    year         int64
    rank         int64
    company     object
    revenue    float64
    profit     float64
    dtype: object






```python
group_by_year = df.loc[:,['year', 'revenue', 'profit']].groupby('year')
avgs = group_by_year.mean()
x = avgs.index
y1 = avgs.profit
def plot(x, y, ax, title, y_label):
    ax.set_title(title)
    ax.set_ylabel(y_label)
    ax.plot(x,y)
    ax.margins(x =  0, y = 0)
```

```python
fig,ax = plt.subplots()
plot(x, y1, ax, 'Increase in mean Fortune 500 company profits 1955-2005', 'profits (millions)')
```

![png](output_15_0.png)

```python
y2 = avgs.revenue
fig,ax = plt.subplots()
plot(x,y2, ax,'Increase in mean Fortune 500 company revenues 1955-2005', 'revenue (millions)')
```

![png](output_16_0.png)

```python
def plot_with_std(x, y, stds, ax, title, y_label):
    ax.fill_between(x, y - stds, y + stds, alpha = 0.2)
    plot(x, y, ax, title, y_label)
fig, (ax1, ax2) = plt.subplots(ncols = 2)
title = 'Increase in mean and std fortune 500 company %s from 1955 to 2005'
stds1 = group_by_year.std().profit.values
stds2 = group_by_year.std().revenue.values
plot_with_std(x, y1.values, stds1, ax1, title % 'profits', 'Profit (millions)')
plot_with_std(x, y2.values, stds2, ax2, title % 'revenues','Revenue (millions)')
fig.set_size_inches(14,4)
fig.tight_layout()
```

![png](output_17_0.png)

```python

```
## Python Fundamentals
In this part, we go through different types of data, how to save variables, and print functions. 
```python
# Aby python interp. can be used a calculator: 
3 + 5 + 4
```


    12






```python
# save a value to a variable
weight_kg = 60.3
```

```python
print(weight_kg)
```
    60.3




```python
# weight0 = valid 
# 0weight = invalid 
# weight and Weight are diff.
```

```python
# types of data
# there are 4 types of data 
# int, floating point numbers, strings
```

```python
# string comprised of letters
patient_name = "John Smith"
```

```python
# string comprised of numbers
patient_id = '001'
```

```python
# use variables in python 
weight_lb = 2.2 * weight_kg
print(weight_lb)
```
    132.66




```python
# add a prefix to patient_id

patient_id = 'inflam_' + patient_id

print(patient_id)
```
    inflam_001




```python
# combine print statements

print(patient_id, 'weight in kg:', weight_kg)
```
    inflam_001 weight in kg: 60.3




```python
# call function in another function 

print(type(60.3))
print(type(patient_id))
```
    <class 'float'>
    <class 'str'>




```python
# calc. in a print function 

print('weight in lbs:', 2.2*weight_kg)
```
    weight in lbs: 132.66




```python
weight_kg = 65
print('weight in kg now:', weight_kg)
```
    weight in kg now: 65




```python

```
## Analyzing Data (1, 2 and 3)
Patient data is plotted and analyzed to find min, max, and std. 
```python
import numpy
```

```python
numpy.loadtxt(fname = "inflammation-01.csv", delimiter =',')
```


    array([[0., 0., 1., ..., 3., 0., 0.],
           [0., 1., 2., ..., 1., 0., 1.],
           [0., 1., 1., ..., 2., 1., 1.],
           ...,
           [0., 1., 1., ..., 1., 1., 1.],
           [0., 0., 0., ..., 0., 2., 0.],
           [0., 0., 1., ..., 1., 1., 0.]])






```python
data =  numpy.loadtxt(fname = "inflammation-01.csv", delimiter =',')
```

```python
print(type(data))
```
    <class 'numpy.ndarray'>




```python
print(data.shape)
```
    (60, 40)




```python
print('first value in data:', data[0,0])
```
    first value in data 0.0




```python
print('middle value in data:', data [29,19])
```
    middle value in data: 16.0




```python
print(data [0:4,0:10])
```
    [[0. 0. 1. 3. 1. 2. 4. 7. 8. 3.]
     [0. 1. 2. 1. 2. 1. 3. 2. 2. 6.]
     [0. 1. 1. 3. 3. 2. 6. 2. 5. 9.]
     [0. 0. 2. 0. 4. 2. 2. 1. 6. 7.]]




```python
print(data [5:10,0:10])
```
    [[0. 0. 1. 2. 2. 4. 2. 1. 6. 4.]
     [0. 0. 2. 2. 4. 2. 2. 5. 5. 8.]
     [0. 0. 1. 2. 3. 1. 2. 3. 5. 3.]
     [0. 0. 0. 3. 1. 5. 6. 5. 5. 8.]
     [0. 1. 1. 2. 1. 3. 5. 3. 5. 8.]]




```python
small = data[:3,36:]
```

```python
print('small is:')
print(small)
```
    small is:
    [[2. 3. 0. 0.]
     [1. 1. 0. 1.]
     [2. 2. 1. 1.]]




```python
# let's use a numpy function
print(numpy.mean(data))
```
    6.14875




```python
maxval, minval, stdval = numpy.amax(data), numpy.amin(data), numpy.std(data)
```

```python
print(maxval, minval, stdval)
```
    20.0 0.0 4.613833197118566




```python
print('max. inflammation:', maxval, 'min.:', minval, 'std.:', stdval)
```
    max. inflammation: 20.0 min.: 0.0 std.: 4.613833197118566




```python
# to look at max inflamm. per patient or avg. from day one

patient_0 =  data[0:1] # 0 on the first axis (rows), everything on the second (columns)
print('max. inflammation for patient 0:', numpy.amax(patient_0))
```
    max. inflammation for patient 0: 18.0




```python
print('max. inflammation for patient 2:', numpy.amax(data[2:]))
```
    max. inflammation for patient 2: 20.0




```python
print(numpy.mean(data, axis = 0))
```
    [ 0.          0.45        1.11666667  1.75        2.43333333  3.15
      3.8         3.88333333  5.23333333  5.51666667  5.95        5.9
      8.35        7.73333333  8.36666667  9.5         9.58333333 10.63333333
     11.56666667 12.35       13.25       11.96666667 11.03333333 10.16666667
     10.          8.66666667  9.15        7.25        7.33333333  6.58333333
      6.06666667  5.95        5.11666667  3.6         3.3         3.56666667
      2.48333333  1.5         1.13333333  0.56666667]




```python
print(numpy.mean(data, axis = 0).shape)
```
    (40,)




```python
print(numpy.mean(data, axis = 1))
```
    [5.45  5.425 6.1   5.9   5.55  6.225 5.975 6.65  6.625 6.525 6.775 5.8
     6.225 5.75  5.225 6.3   6.55  5.7   5.85  6.55  5.775 5.825 6.175 6.1
     5.8   6.425 6.05  6.025 6.175 6.55  6.175 6.35  6.725 6.125 7.075 5.725
     5.925 6.15  6.075 5.75  5.975 5.725 6.3   5.9   6.75  5.925 7.225 6.15
     5.95  6.275 5.7   6.1   6.825 5.975 6.725 5.7   6.25  6.4   7.05  5.9  ]




```python
# heat map of patient inflammation overtime
import matplotlib.pyplot
image = matplotlib.pyplot.imshow(data)
matplotlib.pyplot.show()
```

![png](output_20_0.png)

```python
# Avg. inflammation over time

avg_inflam = numpy.mean(data, axis = 0)
avg_plot = matplotlib.pyplot.plot(avg_inflam)
matplotlib.pyplot.show
```


    <function matplotlib.pyplot.show(*args, **kw)>






![png](output_21_1.png)

```python
max_plot = matplotlib.pyplot.plot(numpy.amax(data, axis = 0))
matplotlib.pyplot.show
```


    <function matplotlib.pyplot.show(*args, **kw)>






![png](output_22_1.png)

```python
min_plot = matplotlib.pyplot.plot(numpy.amin(data, axis = 0))
matplotlib.pyplot.show
```


    <function matplotlib.pyplot.show(*args, **kw)>






![png](output_23_1.png)

```python
fig = matplotlib.pyplot.figure(figsize = (10, 3))

ax1, ax2, ax3 = fig.add_subplot(1, 3, 1), fig.add_subplot(1, 3, 2), fig.add_subplot(1, 3, 3)

ax1.set_ylabel('average')
ax1.plot(numpy.mean(data, axis = 0))

ax2.set_ylabel('max')
ax2.plot(numpy.amax(data, axis = 0))


ax3.set_ylabel('min')
ax3.plot(numpy.amin(data, axis = 0))

fig.tight_layout()

matplotlib.pyplot.savefig('inflammation.png')
matplotlib.pyplot.show

```


    <function matplotlib.pyplot.show(*args, **kw)>






![png](output_24_1.png)

```python

```
## Storing Values in Lists
Here, we go through how to make lists and remove and add elements. 
```python
odds = [1,3, 5, 7]
print('odds are:', odds)
```
    odds are: [1, 3, 5, 7]




```python
print('first element:', odds[0])
print('last element:', odds[3])
print('-1 element:', odds[-1])
```
    first element: 1
    last element: 7
    -1 element: 7




```python
names = ['Curie', 'Darwing', 'Turing'] # Typo in Darwin's name
print('name is orignally:', names)
names[1] = 'Darwin' # correct name
print('final value of names', names)
```
    name is orignally: ['Curie', 'Darwing', 'Turing']
    final value of names ['Curie', 'Darwin', 'Turing']




```python
#name = 'Darwin'
#name[0] = d
```

```python
odds.append(11)
print('odds after adding a value', odds)
```
    odds after adding a value [1, 3, 5, 7, 11]




```python
removed_element = odds.pop(0)
print('odds after removing element', odds)
print('removed element', removed_element)
```
    odds after removing element [3, 5, 7, 11]
    removed element 1




```python
odds.reverse()
print('odds after reversing',odds)
```
    odds after reversing [11, 7, 5, 3]




```python
odds = [3, 5, 7]
primes = odds
primes.append(2)
print('primes', primes)
print ('odds', odds)
```
    primes [3, 5, 7, 2]
    odds [3, 5, 7, 2]




```python
odds = [3, 5,7]
primes = list(odds)
primes.append(2)
print('primes', primes)
print ('odds', odds)
```
    primes [3, 5, 7, 2]
    odds [3, 5, 7]




```python
binomial_name = 'Drosophilia melangoster'

group = binomial_name[0:10]
print('group:', group)

species = binomial_name[11:23]
print('species:', species)

chromosomes = ['X', 'Y', '2', '3', '4']
autosomes = chromosomes[2:5]
print('autosomes:', autosomes)

last = chromosomes[-1]
print('last:', last)

```
    group: Drosophili
    species:  melangoster
    autosomes: ['2', '3', '4']
    last: 4




```python
date = 'Monday 4 January 2023'
day = date[0:6]
print('using 0 to begin range:', day)
day = date[:6]
print('omitting beginning index', day)
```
    using 0 to begin range: Monday
    omitting beginning index Monday




```python
months = ['jan','feb', 'mar', 'apr', 'may', 'june', 'july', 'aug', 'sept', 'oct', 'nov', 'dec']
sond = months[8:12]
print('with known last position:', sond)

sond = months[8:len(months)]
print('using len() to get last entry:', sond)

sond = months[8:]
print("omitting ending index", sond)
```
    with known last position: ['sept', 'oct', 'nov', 'dec']
    using len() to get last entry: ['sept', 'oct', 'nov', 'dec']
    omitting ending index ['sept', 'oct', 'nov', 'dec']




```python

```
## Using Loops
This section shows us how to use for loops. 
```python
odds = [1, 3, 5, 7]

print(odds[0])
print(odds[1])
print(odds[2])
print(odds[3])
```
    1
    3
    5
    7




```python
odds = [1, 3, 5, 7, 9, 11, 13, 15, 17, 19]

for num in odds: 
    print(num)
```
    1
    3
    5
    7
    9
    11
    13
    15
    17
    19




```python
length = 0 
names = ['Curie', 'Darwin', 'Turing']

for value in names: 
    length = length + 1
print('There are', length, 'names in the list.')
```
    There are 3 names in the list.




```python
name = 'Rosalind'

for name in ['Curie', 'Darwin', 'Turing']:
    print(name)
print('after the loop, name is', name)
```
    Curie
    Darwin
    Turing
    after the loop, name is Turing




```python
print(len([0, 1, 2, 3]))
name = ['Curie', 'Darwin', 'Turing']
print(len(name))

```
    4
    3




```python

```
## Using Multiple Files
In this section, we use multiple data files to create plots. 
```python
import glob
```

```python
print(glob.glob('inflammation*.csv'))
```
    ['inflammation-10.csv', 'inflammation-09.csv', 'inflammation-11.csv', 'inflammation-06.csv', 'inflammation-05.csv', 'inflammation-08.csv', 'inflammation-01.csv', 'inflammation-07.csv', 'inflammation-04.csv', 'inflammation-03.csv', 'inflammation-02.csv', 'inflammation-12.csv']




```python
import glob
import numpy
import matplotlib.pyplot

filenames = sorted(glob.glob('inflammation*.csv')) 
filenames = filenames[0:3]

for filenames in filenames: 
    print(filenames)
    
    data = numpy.loadtxt(fname = filenames, delimiter = ',')
    
    fig = matplotlib.pyplot.figure(figsize = (10, 3))
    
    ax1 = fig.add_subplot(1, 3, 1)
    ax2 = fig.add_subplot(1, 3, 2)
    ax3 = fig.add_subplot(1, 3, 3)
    
    ax1.set_ylabel('avg')
    ax1.plot(numpy.mean(data, axis = 0))
    
    ax2.set_ylabel('max')
    ax2.plot(numpy.amax(data, axis = 0))
    
    ax3.set_ylabel('min')
    ax3.plot(numpy.amin(data, axis = 0))
    
    fig.tight_layout()
    matplotlib.pyplot.show

```
    inflammation-01.csv
    inflammation-02.csv
    inflammation-03.csv




![png](output_2_1.png)

![png](output_2_2.png)

![png](output_2_3.png)

```python

```
## Making Choices
Here we use if statements to analyze patient data to determine which data files are questionable.
```python
import numpy
```

```python
data = numpy.loadtxt(fname='inflammation-01.csv', delimiter=',')
```

```python
max_inflammation_0 = numpy.amax(data, axis = 0)[0]
```

```python
max_inflammation_20 = numpy.amax(data, axis = 0)[20]

if max_inflammation_0 == 0 and max_inflammation_20 == 20: 
    print('Sus looking maxima!')

if numpy.sum(numpy.amin(data, axis = 0)) == 0: 
    print('Minima add up to zero!')
else: 
    print('Seems OK!')
```
    Sus looking maxima!
    Seems OK!




```python
data = numpy.loadtxt(fname = 'inflammation-03.csv', delimiter = ',')

max_inflammation_0 = numpy.amax(data, axis = 0)[0]

max_inflammation_20 = numpy.amax(data, axis = 0)[20]

if max_inflammation_0 == 0 and max_inflammation_20 == 20: 
    print('Sus looking maxima!')
elif numpy.sum(numpy.amin(data, axis = 0)) == 0: 
    print('Minima add up to zero! -> HEALTHY PARTICPANT ALERT! ')
else: 
    print('Seems okay!')
```
    Minima add up to zero! -> HEALTHY PARTICPANT ALERT! 




```python

```
## Functions (1, 2, 3 and 4)
We created mutiple functions to convert values, analyze data, etc. more efficiently. 
```python
# convert F to C

f_val = 99

c_val = ((f_val-32)*(5/9))

print(c_val)
```
    37.22222222222222




```python
f_val1 = 43

c_val1 = ((f_val1-32)*(5/9))

print(c_val1)
```
    6.111111111111112




```python
def explicit_f_to_C(temp): 
    # assign the converted val to variable
    converted = ((temp - 32)*(5/9))
    # return the value of the new variable 
    return converted
```

```python
def f_to_C(temp): 
    # return converted values more efficiently than using the return function w/o 
    # a new variable. This code does the same thing as the previous one, but is more explicit 
    # in explaining how the return command works
      return ((temp - 32)*(5/9))
```

```python
f_to_C(32)
```


    0.0






```python
explicit_f_to_C(32)
```


    0.0






```python
print('freezing point of water:', f_to_C(32), 'C')
print('boiling point of water:', f_to_C(212), 'C')
```
    freezing point of water: 0.0 C
    boiling point of water: 100.0 C




```python
def c_to_k(temp_c): 
    return temp_c + 273.15
print('freezing point of water:', c_to_k(0), 'K')
```
    freezing point of water: 273.15 K




```python
def f_to_k(temp_f): 
    temp_c = f_to_C(temp_f)
    temp_k = c_to_k(temp_c)
    return temp_k
print('boiling point of water:', f_to_k(212), 'K')
```
    boiling point of water: 373.15 K




```python
print('again, temp in Kelvin was', f_to_k(212))
```
    again, temp in Kelvin was 373.15




```python
def print_temp():
    print('temp in F was:', temp_f)
    print('temp in C was:', temp_c)
    print('temp in K was:', temp_k)
    
temp_f = 212
temp_c = f_to_C(temp_f)
temp_k = f_to_k(temp_f)
    
print_temp()
```
    temp in F was: 212
    temp in C was: 100.0
    temp in K was: 373.15




```python

```


```python
import numpy
import matplotlib
import matplotlib.pyplot
import glob
```

```python
def visualize(filename): 
    
    data = numpy.loadtxt(fname = filename, delimiter = ',')
    
    fig = matplotlib.pyplot.figure(figsize = (10, 3))
    
    ax1 = fig.add_subplot(1, 3, 1)
    ax2 = fig.add_subplot(1, 3, 2)
    ax3 = fig.add_subplot(1, 3, 3)
    
    ax1.set_ylabel('avg')
    ax1.plot(numpy.mean(data, axis = 0))
    
    ax2.set_ylabel('max')
    ax2.plot(numpy.amax(data, axis = 0))
    
    ax3.set_ylabel('min')
    ax3.plot(numpy.amin(data, axis = 0))
    
    fig.tight_layout()
    matplotlib.pyplot.show()
    
```

```python
def detect_problems(filename):
    
    data = numpy.loadtxt(fname = filename, delimiter = ',')
    
    if numpy.amax(data, axis = 0)[0] == 0 and numpy.amax(data, axis = 0)[20] == 20: 
        print('Sus looking maxima!')
    elif numpy.sum(numpy.amin(data, axis = 0)) == 0: 
        print('Minima add up to zero!')
    else: 
        print('Seems okay!')
```

```python
filenames = sorted(glob.glob('inflammation*.csv'))

for filename in filenames[:3]:
    print(filename)
    visualize(filename)
    detect_problems(filename)
```
    inflammation-01.csv




![png](output_3_1.png)

    Sus looking maxima!
    inflammation-02.csv




![png](output_3_3.png)

    Sus looking maxima!
    inflammation-03.csv




![png](output_3_5.png)

    Minima add up to zero!




```python
def offset_mean(data, target_mean_value): 
    return (data - numpy.mean(data)) + target_mean_value
```

```python
z = numpy.zeros((2,2))
print(offset_mean(z, 3))
```
    [[3. 3.]
     [3. 3.]]




```python
data = numpy.loadtxt(fname = 'inflammation-01.csv', delimiter = ',')

print(offset_mean(data, 0))
```
    [[-6.14875 -6.14875 -5.14875 ... -3.14875 -6.14875 -6.14875]
     [-6.14875 -5.14875 -4.14875 ... -5.14875 -6.14875 -5.14875]
     [-6.14875 -5.14875 -5.14875 ... -4.14875 -5.14875 -5.14875]
     ...
     [-6.14875 -5.14875 -5.14875 ... -5.14875 -5.14875 -5.14875]
     [-6.14875 -6.14875 -6.14875 ... -6.14875 -4.14875 -6.14875]
     [-6.14875 -6.14875 -5.14875 ... -5.14875 -5.14875 -6.14875]]




```python
print('original min, mean and max are:', numpy.amin(data), numpy.mean(data), numpy.amax(data))
offset_data = offset_mean(data, 0)
print('min, mean, and max of offset data are:', numpy.amin(offset_data), numpy.mean(offset_data),
    numpy.amin(offset_data), numpy.amax(offset_data))
    
```
    original min, mean and max are: 0.0 6.14875 20.0
    min, mean, and max of offset data are: -6.14875 2.842170943040401e-16 -6.14875 13.85125




```python
print('std dev before and after:', numpy.std(data), numpy.std(offset_data))
```
    std dev before and after: 4.613833197118566 4.613833197118566




```python
print('difference in std before and after:', numpy.std(data) - numpy.std(offset_data))

```
    difference in std before and after: 0.0




```python
# offset_mean(data, target_mean_value): 
# return a new array containing the original data with its mean offset to match the desired value

def offset_mean(data, target_mean_value):
    return (data - numpy.mean(data)) + target_mean_value
```

```python
def offset_mean(data, target_mean_value): 
    return  (data - numpy.mean(data)) + target_mean_value
```

```python
help(offset_mean)
```
    Help on function offset_mean in module __main__:
    
    offset_mean(data, target_mean_value)
    





```python
def offset_mean(data):
    """return a new arraw containing the original data with its mean offset to match the desired value
    
        Examples 
        --------
        >>> offset_mean([1,2,3], 0)
        array([-1., 0., 1.])
        """
    return (data - numpy.mean(data))
```

```python
help(offset_mean)
```
    Help on function offset_mean in module __main__:
    
    offset_mean(data, target_mean_value)
        return a new arraw containing the original data with its mean offset to match the desired value
        
        Examples 
        --------
        >>> offset_mean([1,2,3], 0)
        array([-1., 0., 1.])
    





```python
numpy.loadtxt('inflammation-01.csv', delimiter = ',')

```


    array([[0., 0., 1., ..., 3., 0., 0.],
           [0., 1., 2., ..., 1., 0., 1.],
           [0., 1., 1., ..., 2., 1., 1.],
           ...,
           [0., 1., 1., ..., 1., 1., 1.],
           [0., 0., 0., ..., 0., 2., 0.],
           [0., 0., 1., ..., 1., 1., 0.]])






```python
test_data = numpy.zeros((2,2))
print(offset_mean(test_data, 3))
```
    [[3. 3.]
     [3. 3.]]




```python
print(offset_mean(test_data))
```
    [[0. 0.]
     [0. 0.]]




```python
def display(a = 1, b = 2, c = 3):
    print('a:', a, 'b:', b, 'c:', c)

print('no parameters:')
display()
print('one parameter:')
display(55)
print('two parameters:')
display(55,66)
    
```
    no parameters:
    a: 1 b: 2 c: 3
    one parameter:
    a: 55 b: 2 c: 3
    two parameters:
    a: 55 b: 66 c: 3




```python
print('only setting the value of c')
display(c = 77)
```
    only setting the value of c
    a: 1 b: 2 c: 77




```python
numpy.loadtxt('inflammation-01.csv', delimiter = ',')
```

## Defensive Programming
This sections show how we can use assertions to check for errors.  
```python
numbers = [1.5, 2.3, 0.7, 0.001, 4.4]
total = 0

for num in numbers: 
    assert num > 0, 'Data should only contain positive values'
    total += num 
print('total is:', total)
```
    total is: 8.901




```python
def normalize_rec(rect):
    """normalize a rec so that it is at the ori. and 1 units long
    input should be of the format (x0, y0, x1, y1) 
    this defines the lower and upper right corners of rec"""
    
    assert len(rect) == 4, 'rect must contain 4 coord.'
    x0, y0, x1, y1 = rect
    assert x0 < x1, 'invalid x coord' 
    assert y0 < y1, 'invalid y coord' 
    
    dx = x1 - x0 
    dy = y1 - y0 
    
    if dx > dy: 
        scaled = dx/dy
        upx, upy = 1, scaled
    else: 
        scaled = dx/dy
        upx, upy = scaled, 1
        
    assert 0 < upx <= 1, 'calc upper x invalid'
    assert 0 < upy <= 1, 'calc upper y invalid'
    
    return (0, 0, upx, upy)
```

```python
print(normalize_rec(0, 1, 2))
```

    ---------------------------------------------------------------------------

    TypeError                                 Traceback (most recent call last)

    <ipython-input-12-4df19797542a> in <module>
    ----> 1 print(normalize_rec(0, 1, 2))
    

    TypeError: normalize_rec() takes 1 positional argument but 3 were given




```python
print(normalize_rec(4, 2, 1, 5))
```

    ---------------------------------------------------------------------------

    TypeError                                 Traceback (most recent call last)

    <ipython-input-13-058baf2b204d> in <module>
    ----> 1 print(normalize_rec(4, 2, 1, 5))
    

    TypeError: normalize_rec() takes 1 positional argument but 4 were given




```python
print(normalize_rec((0, 0, 1, 5)))
```
    (0, 0, 0.2, 1)




```python
print(normalize_rec((0, 0, 5, 0)))
```

    ---------------------------------------------------------------------------

    AssertionError                            Traceback (most recent call last)

    <ipython-input-21-301b63d57c22> in <module>
    ----> 1 print(normalize_rec((0, 0, 5, 0)))
    

    <ipython-input-18-f5b7206448f9> in normalize_rec(rect)
          7     x0, y0, x1, y1 = rect
          8     assert x0 < x1, 'invalid x coord'
    ----> 9     assert y0 < y1, 'invalid y coord'
         10 
         11     dx = x1 - x0


    AssertionError: invalid y coord




```python

```
## Transcribing DNA into RNA
Here we use clostridium DNA sequence and convert to RNA. 
```python
# prompt user to enter the input fasta file name 

input_file_name = input ('enter the name of the input fasta file')
```
    enter the name of the input fasta file clostridium




```python
# open the input FASTA file and read the DNA seq. 
with open(input_file_name, 'r') as input_file: 
    DNA_seq = ""
    for line in input_file: 
        if line.startswith('>'): 
            continue
        DNA_seq += line.strip()
```

```python
# transcribe the DNA to RNA
RNA_seq = ""
for nucleotide in DNA_seq: 
    if nucleotide == "T": 
        RNA_seq += "U"
    else: 
        RNA_seq += nucleotide
        
```

```python
# prompt user to enter the output file name

output_file_name = input('enter the name of output file')
```
    enter the name of output file clostridium_RNA




```python
# save the RNA seq to a txt file 

with open(output_file_name,'w') as output_file: 
    output_file.write(RNA_seq)
    print(f'RNA seq has been saved to {output_file_name}')
```
    RNA seq has been saved to clostridium_RNA




```python
print(RNA_seq)
```
    AUGAAAAGAAAGAUUUGUAAGGCGCUUAUUUGUGCCGCGCUAGCAACUAGCCUAUGGGCUGGGACAUCAACUAAAGUCUACGCUUGGGAUGGAAAGAUUGAUGGAACAGGAACUCAUGCUAUGAUUGUAACUCAAGGGGUUUCAAUCUUAGAAAAUGAUCUGUCCAAAAAUGAACCAGAAAGUGUAAGAAAAAACUUAGAGAUUUUAAAAGAGAACAUGCAUGAACUUCAAUUAGGUUCUACUUAUCCAGAUUAUGAUAAGAAUGCAUAUGAUCUAUAUCAAGAUCAUUUCUGGGAUCCUGAUACAGAUAAUAAUUUCUCAAAGGAUAAUAGUUGGUAUUUAGCUUAUUCUAUACCUGAUACAGGGGAAUCACAAAUAAGAAAAUUUUCAGCAUUAGCUAGAUAUGAAUGGCAAAGAGGAAACUAUAAACAAGCUACAUUCUAUCUUGGAGAGGCUAUGCACUAUUUUGGAGAUAUAGAUACUCCAUAUCAUCCUGCUAAUGUUACUGCCGUUGAUAGCGCAGGACAUGUUAAGUUUGAAACUUUUGCAGAGGAAAGAAAAGAACAGUAUAAAAUAAACACAGCAGGUUGCAAAACUAAUGAGGAUUUUUAUGCUGAUAUCUUAAAAAACAAAGAUUUUAAUGCAUGGUCAAAAGAAUAUGCAAGAGGUUUUGCUAAAACAGGGAAAUCAAUAUACUAUAGUCAUGCUAGCAUGAGUCAUAGUUGGGAUGAUUGGGAUUAUGCAGCAAAGGUAACUCUAGCUAACUCUCAAAAAGGAACAGCGGGAUAUAUUUAUAGAUUCUUACACGAUGUAUCAGAGGGUAAUGAUCCAUCAGUUGGAAAGAAUGUAAAAGAACUAGUAGCUUACAUAUCAACUAGUGGUGAAAAAGAUGCUGGAACAGAUGACUACAUGUAUUUUGGAAUCAAAACAAAGGAUGGAAAAACUCAAGAAUGGGAAAUGGACAACCCAGGAAAUGAUUUUAUGACUGGAAGUAAAGACACUUAUACUUUCAAAUUAAAAGAUGAAAAUCUAAAAAUUGAUGAUAUACAAAAUAUGUGGAUUAGAAAAAGAAAAUAUACAGCAUUCCCAGAUGCUUAUAAGCCAGAAAACAUAAAGUUAAUAGCAAAUGGAAAAGUUGUAGUGGACAAGGAUAUAAAUGAGUGGAUUUCAGGAAAUUCAACUUAUAAUAUAAAAUAA
​
## Translating RNA into Protein
The clostridium RNA seq is then converted to protein sequence. 
```python
# prompt user to enter the input RNA file name 

RNA_file_name = input ('enter the name of the input RNA file')
```
    enter the name of the input RNA file clostridium_RNA




```python
# open the input RNA file and read the RNA seq. 
with open(RNA_file_name, 'r') as input_file:
    RNA_seq = input_file.read().strip()
```

```python
# Define codon table
codon_table = {
    "UUU": "F", "UUC": "F", "UUA": "L", "UUG": "L",
    "UCU": "S", "UCC": "S", "UCA": "S", "UCG": "S",
    "UAU": "Y", "UAC": "Y", "UAA": "",  "UAG": "",
    "UGU": "C", "UGC": "C", "UGA": "",  "UGG": "W",

    "CUU": "L", "CUC": "L", "CUA": "L", "CUG": "L",
    "CCU": "P", "CCC": "P", "CCA": "P", "CCG": "P",
    "CAU": "H", "CAC": "H", "CAA": "Q", "CAG": "Q",
    "CGU": "R", "CGC": "R", "CGA": "R", "CGG": "R",

    "AUU": "I", "AUC": "I", "AUA": "I", "AUG": "M",
    "ACU": "T", "ACC": "T", "ACA": "T", "ACG": "T",
    "AAU": "N", "AAC": "N", "AAA": "K", "AAG": "K",
    "AGU": "S", "AGC": "S", "AGA": "R", "AGG": "R",

    "GUU": "V", "GUC": "V", "GUA": "V", "GUG": "V",
    "GCU": "A", "GCC": "A", "GCA": "A", "GCG": "A",
    "GAU": "D", "GAC": "D", "GAA": "E", "GAG": "E",
    "GGU": "G", "GGC": "G", "GGA": "G", "GGG": "G"
}
```

```python
# translate the RNA to Protien
protein_seq = ""
for i in range (0, len(RNA_seq), 3):
    codon = RNA_seq[i:i+3]
    if len(codon) == 3: 
        amino_acid = codon_table[codon]
        if amino_acid == "*": 
            break
        protein_seq += amino_acid
        
```

```python
# prompt user to enter the output file name

output_file_name = input('enter the name of output file')
```
    enter the name of output file clostridium_protein




```python
# save the protein seq to a txt file 

with open(output_file_name,'w') as output_file: 
    output_file.write(protein_seq)
    print(f'the protein seq has been saved to {output_file_name}')
```
    the protein seq has been saved to clostridium_protein




```python
print(protein_seq)
```
    MKRKICKALICAALATSLWAGTSTKVYAWDGKIDGTGTHAMIVTQGVSILENDLSKNEPESVRKNLEILKENMHELQLGSTYPDYDKNAYDLYQDHFWDPDTDNNFSKDNSWYLAYSIPDTGESQIRKFSALARYEWQRGNYKQATFYLGEAMHYFGDIDTPYHPANVTAVDSAGHVKFETFAEERKEQYKINTAGCKTNEDFYADILKNKDFNAWSKEYARGFAKTGKSIYYSHASMSHSWDDWDYAAKVTLANSQKGTAGYIYRFLHDVSEGNDPSVGKNVKELVAYISTSGEKDAGTDDYMYFGIKTKDGKTQEWEMDNPGNDFMTGSKDTYTFKLKDENLKIDDIQNMWIRKRKYTAFPDAYKPENIKLIANGKVVVDKDINEWISGNSTYNIK




```python

```
