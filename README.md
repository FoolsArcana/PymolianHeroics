
#### Observations
Observation 1: greater effort would need to be made in courting and appealing to whales, as if any have been truly retained, they have been underutilized, and giving them a berth would yield a steady and reliable revenue.
Observation 2: 4-1 gender ratio? Not sure if that's typical, per se, but it is interesting that females are 17.5% of the playerbase, yet 16.5% of the revenue. i.e. males are purchasing more/person than females.
Observation 3: seriously, who's giving kids under 15 accounts capable of purchasing, and then validating those accounts? Don't do it. Or at least provide a more obvious 'parental lock'
Observation 4: clearly playerbase is accrued in early college/late high school years, peaks in early 20's, with significant attrition of interest in purchasing afterwards. this could be remedied with increasing number of desireable products, or trying to increase player retention over time, with rotating content, etc.

#### Data initialization


```python
import pandas as pd
import numpy as np
```


```python
initFile1 = pd.read_json('purchase_data.json')
```


```python
initFile2 = pd.read_json('purchase_data2.json')
```


```python
initFile = initFile1
```


```python
initFile['PLAYER']= initFile.apply(lambda row: str(row['Age'] )+str(row['Gender']) +str(  row['SN']), axis=1)
initFile['ITEM']= initFile.apply(lambda row: str(row['Item ID'])+'/'+row['Item Name'] + '/'+str( row['Price']), axis=1)
initFile['AGEBIN']=initFile.apply(lambda row:str( int(row['Age']/5)*5)+'-'+str(int(row['Age']/5)*5+4), axis=1)
initFile.head()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Age</th>
      <th>Gender</th>
      <th>Item ID</th>
      <th>Item Name</th>
      <th>Price</th>
      <th>SN</th>
      <th>PLAYER</th>
      <th>ITEM</th>
      <th>AGEBIN</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>38</td>
      <td>Male</td>
      <td>165</td>
      <td>Bone Crushing Silver Skewer</td>
      <td>3.37</td>
      <td>Aelalis34</td>
      <td>38MaleAelalis34</td>
      <td>165/Bone Crushing Silver Skewer/3.37</td>
      <td>35-39</td>
    </tr>
    <tr>
      <th>1</th>
      <td>21</td>
      <td>Male</td>
      <td>119</td>
      <td>Stormbringer, Dark Blade of Ending Misery</td>
      <td>2.32</td>
      <td>Eolo46</td>
      <td>21MaleEolo46</td>
      <td>119/Stormbringer, Dark Blade of Ending Misery/...</td>
      <td>20-24</td>
    </tr>
    <tr>
      <th>2</th>
      <td>34</td>
      <td>Male</td>
      <td>174</td>
      <td>Primitive Blade</td>
      <td>2.46</td>
      <td>Assastnya25</td>
      <td>34MaleAssastnya25</td>
      <td>174/Primitive Blade/2.46</td>
      <td>30-34</td>
    </tr>
    <tr>
      <th>3</th>
      <td>21</td>
      <td>Male</td>
      <td>92</td>
      <td>Final Critic</td>
      <td>1.36</td>
      <td>Pheusrical25</td>
      <td>21MalePheusrical25</td>
      <td>92/Final Critic/1.3599999999999999</td>
      <td>20-24</td>
    </tr>
    <tr>
      <th>4</th>
      <td>23</td>
      <td>Male</td>
      <td>63</td>
      <td>Stormfury Mace</td>
      <td>1.27</td>
      <td>Aela59</td>
      <td>23MaleAela59</td>
      <td>63/Stormfury Mace/1.27</td>
      <td>20-24</td>
    </tr>
  </tbody>
</table>
</div>



#### Player count and purchasing analysis


```python
numUniquePlayers = len(initFile['PLAYER'].value_counts())
numUniqueItems = len(initFile['ITEM'].value_counts())
avgPrice = initFile['Price'].mean()
purchaseCount = len(initFile['Age'])
sumPrice = initFile['Price'].sum()
pd.DataFrame(data={'Player Count':[numUniquePlayers],'Item Count':[numUniqueItems],'Average Item Price':[avgPrice],'Purchases Made':[purchaseCount],'Revenue':[sumPrice]})
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Average Item Price</th>
      <th>Item Count</th>
      <th>Player Count</th>
      <th>Purchases Made</th>
      <th>Revenue</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2.931192</td>
      <td>183</td>
      <td>573</td>
      <td>780</td>
      <td>2286.33</td>
    </tr>
  </tbody>
</table>
</div>



#### Gender Demographics


```python
nextFrame = pd.DataFrame(initFile.drop_duplicates('PLAYER')['Gender'].value_counts())
nextFrame['Gender %'] = nextFrame['Gender']/numUniquePlayers*100
nextFrame.head()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Gender</th>
      <th>Gender %</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Male</th>
      <td>465</td>
      <td>81.151832</td>
    </tr>
    <tr>
      <th>Female</th>
      <td>100</td>
      <td>17.452007</td>
    </tr>
    <tr>
      <th>Other / Non-Disclosed</th>
      <td>8</td>
      <td>1.396161</td>
    </tr>
  </tbody>
</table>
</div>



#### Purchase Data by Gender


```python
genderedFile = initFile.groupby('Gender')
genderedAnalysisFrame = pd.DataFrame(initFile['Gender'].value_counts())
genderedAnalysisFrame['Avg Price'] = genderedFile['Price'].mean()
genderedAnalysisFrame['Normalized Price']= genderedAnalysisFrame['Avg Price'] 
genderedAnalysisFrame['Total Revenue'] = genderedFile['Price'].sum()
genderedAnalysisFrame = genderedAnalysisFrame.rename(columns = {"Gender":"Purchase Count"})
genderedAnalysisFrame
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Purchase Count</th>
      <th>Avg Price</th>
      <th>Normalized Price</th>
      <th>Total Revenue</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Male</th>
      <td>633</td>
      <td>2.950521</td>
      <td>2.950521</td>
      <td>1867.68</td>
    </tr>
    <tr>
      <th>Female</th>
      <td>136</td>
      <td>2.815515</td>
      <td>2.815515</td>
      <td>382.91</td>
    </tr>
    <tr>
      <th>Other / Non-Disclosed</th>
      <td>11</td>
      <td>3.249091</td>
      <td>3.249091</td>
      <td>35.74</td>
    </tr>
  </tbody>
</table>
</div>



#### Purchase data by age group


```python
agedFile = initFile.groupby('AGEBIN')
agedAnalysisFrame = pd.DataFrame(initFile['AGEBIN'].value_counts())
agedAnalysisFrame['Avg Price'] = agedFile['Price'].mean()
agedAnalysisFrame['Normalized Price']= agedAnalysisFrame['Avg Price'] 
agedAnalysisFrame['Total Revenue'] = agedFile['Price'].sum()
agedAnalysisFrame = agedAnalysisFrame.rename(columns = {"AGEBIN":"Purchase Count"})
agedAnalysisFrame=(agedAnalysisFrame).sort_index()
agedAnalysisFrame
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Purchase Count</th>
      <th>Avg Price</th>
      <th>Normalized Price</th>
      <th>Total Revenue</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>10-14</th>
      <td>35</td>
      <td>2.770000</td>
      <td>2.770000</td>
      <td>96.95</td>
    </tr>
    <tr>
      <th>15-19</th>
      <td>133</td>
      <td>2.905414</td>
      <td>2.905414</td>
      <td>386.42</td>
    </tr>
    <tr>
      <th>20-24</th>
      <td>336</td>
      <td>2.913006</td>
      <td>2.913006</td>
      <td>978.77</td>
    </tr>
    <tr>
      <th>25-29</th>
      <td>125</td>
      <td>2.962640</td>
      <td>2.962640</td>
      <td>370.33</td>
    </tr>
    <tr>
      <th>30-34</th>
      <td>64</td>
      <td>3.082031</td>
      <td>3.082031</td>
      <td>197.25</td>
    </tr>
    <tr>
      <th>35-39</th>
      <td>42</td>
      <td>2.842857</td>
      <td>2.842857</td>
      <td>119.40</td>
    </tr>
    <tr>
      <th>40-44</th>
      <td>16</td>
      <td>3.189375</td>
      <td>3.189375</td>
      <td>51.03</td>
    </tr>
    <tr>
      <th>45-49</th>
      <td>1</td>
      <td>2.720000</td>
      <td>2.720000</td>
      <td>2.72</td>
    </tr>
    <tr>
      <th>5-9</th>
      <td>28</td>
      <td>2.980714</td>
      <td>2.980714</td>
      <td>83.46</td>
    </tr>
  </tbody>
</table>
</div>



#### Highest spending players


```python
whaleFile = initFile.groupby('PLAYER')
whaleAnalysisFrame = pd.DataFrame(initFile['PLAYER'].value_counts())
whaleAnalysisFrame['Avg Price'] = whaleFile['Price'].mean()
whaleAnalysisFrame['Normalized Price']= whaleAnalysisFrame['Avg Price'] 
whaleAnalysisFrame['Total Revenue'] = whaleFile['Price'].sum()
whaleAnalysisFrame['SN'] = whaleFile['SN'].unique().apply(lambda row:row[0])
whaleAnalysisFrame = whaleAnalysisFrame.rename(columns = {"PLAYER":"Purchase Count"})
whaleAnalysisFrame=(whaleAnalysisFrame).sort_values('Total Revenue', ascending = False)
whaleAnalysisFrame.reset_index(inplace=True)
whaleAnalysisFrame.drop(['index','Normalized Price'],axis = 1,inplace=True)
whaleAnalysisFrame.head()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Purchase Count</th>
      <th>Avg Price</th>
      <th>Total Revenue</th>
      <th>SN</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>5</td>
      <td>3.412000</td>
      <td>17.06</td>
      <td>Undirrala66</td>
    </tr>
    <tr>
      <th>1</th>
      <td>4</td>
      <td>3.390000</td>
      <td>13.56</td>
      <td>Saedue76</td>
    </tr>
    <tr>
      <th>2</th>
      <td>4</td>
      <td>3.185000</td>
      <td>12.74</td>
      <td>Mindimnya67</td>
    </tr>
    <tr>
      <th>3</th>
      <td>3</td>
      <td>4.243333</td>
      <td>12.73</td>
      <td>Haellysu29</td>
    </tr>
    <tr>
      <th>4</th>
      <td>3</td>
      <td>3.860000</td>
      <td>11.58</td>
      <td>Eoda93</td>
    </tr>
  </tbody>
</table>
</div>



#### Most purchased items


```python
game_itemFile = initFile.groupby('ITEM')
game_itemAnalysisFrame = pd.DataFrame(initFile['ITEM'].value_counts())
game_itemAnalysisFrame['Price'] = game_itemFile['Price'].mean()
game_itemAnalysisFrame['Total Revenue'] = game_itemFile['Price'].sum()
game_itemAnalysisFrame['Item Name'] = game_itemFile['Item Name'].unique().apply(lambda row:row[0])
game_itemAnalysisFrame['Item ID'] = game_itemFile['Item ID'].unique().apply(lambda row:row[0])
game_itemAnalysisFrame = game_itemAnalysisFrame.rename(columns = {"ITEM":"Purchase Count"})
game_itemAnalysisFrame=(game_itemAnalysisFrame).sort_values('Purchase Count', ascending = False)
game_itemAnalysisFrame.reset_index(inplace=True)
game_itemAnalysisFrame.drop(['index'],axis = 1,inplace=True)
game_itemAnalysisFrame.head()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Purchase Count</th>
      <th>Price</th>
      <th>Total Revenue</th>
      <th>Item Name</th>
      <th>Item ID</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>11</td>
      <td>2.35</td>
      <td>25.85</td>
      <td>Betrayal, Whisper of Grieving Widows</td>
      <td>39</td>
    </tr>
    <tr>
      <th>1</th>
      <td>11</td>
      <td>2.23</td>
      <td>24.53</td>
      <td>Arcane Gem</td>
      <td>84</td>
    </tr>
    <tr>
      <th>2</th>
      <td>9</td>
      <td>1.24</td>
      <td>11.16</td>
      <td>Woeful Adamantite Claymore</td>
      <td>175</td>
    </tr>
    <tr>
      <th>3</th>
      <td>9</td>
      <td>4.14</td>
      <td>37.26</td>
      <td>Retribution Axe</td>
      <td>34</td>
    </tr>
    <tr>
      <th>4</th>
      <td>9</td>
      <td>1.49</td>
      <td>13.41</td>
      <td>Serenity</td>
      <td>13</td>
    </tr>
  </tbody>
</table>
</div>



#### Most profitable items


```python
profit_itemFile = initFile.groupby('ITEM')
profit_itemAnalysisFrame = pd.DataFrame(initFile['ITEM'].value_counts())
profit_itemAnalysisFrame['Price'] = profit_itemFile['Price'].mean()
profit_itemAnalysisFrame['Total Revenue'] = profit_itemFile['Price'].sum()
profit_itemAnalysisFrame['Item Name'] = profit_itemFile['Item Name'].unique().apply(lambda row:row[0])
profit_itemAnalysisFrame['Item ID'] = profit_itemFile['Item ID'].unique().apply(lambda row:row[0])
profit_itemAnalysisFrame = profit_itemAnalysisFrame.rename(columns = {"ITEM":"Purchase Count"})
profit_itemAnalysisFrame=(profit_itemAnalysisFrame).sort_values('Total Revenue', ascending = False)
profit_itemAnalysisFrame.reset_index(inplace=True)
profit_itemAnalysisFrame.drop(['index'],axis = 1,inplace=True)
profit_itemAnalysisFrame.head()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Purchase Count</th>
      <th>Price</th>
      <th>Total Revenue</th>
      <th>Item Name</th>
      <th>Item ID</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>9</td>
      <td>4.14</td>
      <td>37.26</td>
      <td>Retribution Axe</td>
      <td>34</td>
    </tr>
    <tr>
      <th>1</th>
      <td>7</td>
      <td>4.25</td>
      <td>29.75</td>
      <td>Spectral Diamond Doomblade</td>
      <td>115</td>
    </tr>
    <tr>
      <th>2</th>
      <td>6</td>
      <td>4.95</td>
      <td>29.70</td>
      <td>Orenmir</td>
      <td>32</td>
    </tr>
    <tr>
      <th>3</th>
      <td>6</td>
      <td>4.87</td>
      <td>29.22</td>
      <td>Singed Scalpel</td>
      <td>103</td>
    </tr>
    <tr>
      <th>4</th>
      <td>8</td>
      <td>3.61</td>
      <td>28.88</td>
      <td>Splitter, Foe Of Subtlety</td>
      <td>107</td>
    </tr>
  </tbody>
</table>
</div>


