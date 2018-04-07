

```python
import pandas as pd

data = "purchase_data.json"

data_df = pd.read_json(data)
data_df.head()
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
      <th>Age</th>
      <th>Gender</th>
      <th>Item ID</th>
      <th>Item Name</th>
      <th>Price</th>
      <th>SN</th>
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
    </tr>
    <tr>
      <th>1</th>
      <td>21</td>
      <td>Male</td>
      <td>119</td>
      <td>Stormbringer, Dark Blade of Ending Misery</td>
      <td>2.32</td>
      <td>Eolo46</td>
    </tr>
    <tr>
      <th>2</th>
      <td>34</td>
      <td>Male</td>
      <td>174</td>
      <td>Primitive Blade</td>
      <td>2.46</td>
      <td>Assastnya25</td>
    </tr>
    <tr>
      <th>3</th>
      <td>21</td>
      <td>Male</td>
      <td>92</td>
      <td>Final Critic</td>
      <td>1.36</td>
      <td>Pheusrical25</td>
    </tr>
    <tr>
      <th>4</th>
      <td>23</td>
      <td>Male</td>
      <td>63</td>
      <td>Stormfury Mace</td>
      <td>1.27</td>
      <td>Aela59</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Player Count
players = data_df["SN"].nunique()

Total_Players = pd.DataFrame({"Total Players": [players]})

Total_Players
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
      <th>Total Players</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>573</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Purchasing Analysis (Total)
PA_Total = data_df
unique_items = PA_Total["Item ID"].nunique()
average_price = PA_Total["Price"].mean()
total_items = PA_Total["Item ID"].count()
revenue = PA_Total["Price"].sum()

PA_Final = pd.DataFrame({"Number of Unique Items": [unique_items], 
                           "Average Price": [average_price],
                           "Number of Purchases": [total_items],
                          "Total Revenue": [revenue]})

PA_Final["Average Price"] = PA_Final["Average Price"].map("${:.2f}".format)
PA_Final["Total Revenue"] = PA_Final["Total Revenue"].map("${:.2f}".format)

PA_Final
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
      <th>Average Price</th>
      <th>Number of Unique Items</th>
      <th>Number of Purchases</th>
      <th>Total Revenue</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>$2.93</td>
      <td>183</td>
      <td>780</td>
      <td>$2286.33</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Gender Demographics
gender_dup = data_df

gender_dup = gender_dup.drop_duplicates(['SN'], keep='first')

Total_Gender = gender_dup["Gender"].value_counts()
gender_percent = ((Total_Gender/players) * 100).round(2)

gender_demographics= pd.DataFrame({"Total Count": Total_Gender, "Percentage of Players": gender_percent})

gender_demographics["Percentage of Players"] = gender_demographics["Percentage of Players"].map("{:.2f}%".format)

gender_demographics
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
      <th>Percentage of Players</th>
      <th>Total Count</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Male</th>
      <td>81.15%</td>
      <td>465</td>
    </tr>
    <tr>
      <th>Female</th>
      <td>17.45%</td>
      <td>100</td>
    </tr>
    <tr>
      <th>Other / Non-Disclosed</th>
      <td>1.40%</td>
      <td>8</td>
    </tr>
  </tbody>
</table>
</div>




```python
data_gender = data_df
gender_value_count = data_gender["Gender"].value_counts()
gender_sum = data_gender.groupby(["Gender"]).sum()
gender_sum_price = gender_sum["Price"]
gender_mean = data_gender.groupby(["Gender"]).mean()
gender_mean_price = gender_mean["Price"]
gender_normalized = gender_sum_price/Total_Gender

gender_spent= pd.DataFrame({"Purchase Count": gender_value_count, "Total Spent": gender_sum_price, 
                            "Average Spent": gender_mean_price, "Normalized Price":gender_normalized})

gender_spent["Total Spent"] = gender_spent["Total Spent"].map("${:.2f}".format)
gender_spent["Average Spent"] = gender_spent["Average Spent"].map("${:.2f}".format)
gender_spent["Normalized Price"] = gender_spent["Normalized Price"].map("${:.2f}".format)

gender_spent
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
      <th>Average Spent</th>
      <th>Normalized Price</th>
      <th>Purchase Count</th>
      <th>Total Spent</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Female</th>
      <td>$2.82</td>
      <td>$3.83</td>
      <td>136</td>
      <td>$382.91</td>
    </tr>
    <tr>
      <th>Male</th>
      <td>$2.95</td>
      <td>$4.02</td>
      <td>633</td>
      <td>$1867.68</td>
    </tr>
    <tr>
      <th>Other / Non-Disclosed</th>
      <td>$3.25</td>
      <td>$4.47</td>
      <td>11</td>
      <td>$35.74</td>
    </tr>
  </tbody>
</table>
</div>




```python
Age_breakdown_df = data_df

bins = [0, 10, 15, 20, 25, 30, 35, 40, 120]
group_labels = ["< 10", "10 to 14", "15 to 19", "20 to 24", "25 to 29", "30 to 34",
                "35 to 39", "40 +"]

pd.cut(Age_breakdown_df["Age"], bins, labels=group_labels)

Age_breakdown_df["Age Breakdown"] = pd.cut(Age_breakdown_df["Age"], bins, labels=group_labels)

age_index = Age_breakdown_df.groupby("Age Breakdown")
age_nunique = age_index["SN"].nunique()
age_percent = ((age_nunique/players) * 100).round(2)
age_breakdown_final = pd.DataFrame({"Count": age_nunique, "Percent of Players": age_percent})

age_breakdown_final["Percent of Players"] = age_breakdown_final["Percent of Players"].map("{:.2f}%".format)

age_breakdown_final
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
      <th>Count</th>
      <th>Percent of Players</th>
    </tr>
    <tr>
      <th>Age Breakdown</th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>&lt; 10</th>
      <td>22</td>
      <td>3.84%</td>
    </tr>
    <tr>
      <th>10 to 14</th>
      <td>54</td>
      <td>9.42%</td>
    </tr>
    <tr>
      <th>15 to 19</th>
      <td>139</td>
      <td>24.26%</td>
    </tr>
    <tr>
      <th>20 to 24</th>
      <td>234</td>
      <td>40.84%</td>
    </tr>
    <tr>
      <th>25 to 29</th>
      <td>52</td>
      <td>9.08%</td>
    </tr>
    <tr>
      <th>30 to 34</th>
      <td>44</td>
      <td>7.68%</td>
    </tr>
    <tr>
      <th>35 to 39</th>
      <td>25</td>
      <td>4.36%</td>
    </tr>
    <tr>
      <th>40 +</th>
      <td>3</td>
      <td>0.52%</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Age Spending
Age_Analysis_Spending_df = data_df

bins = [0, 10, 15, 20, 25, 30, 35, 40, 120]
group_labels = ["< 10", "10 to 14", "15 to 19", "20 to 24", "25 to 29", "30 to 34",
                "35 to 39", "40 +"]

pd.cut(Age_Analysis_Spending_df["Age"], bins, labels=group_labels)

Age_Analysis_Spending_df["Age Breakdown"] = pd.cut(Age_Analysis_Spending_df["Age"], bins, labels=group_labels)

age_analysis_index = Age_Analysis_Spending_df.groupby("Age Breakdown")
age_analysis_count = age_analysis_index["Item ID"].count()
age_analysis_sum = age_analysis_index["Price"].sum()
age_analysis_mean = age_analysis_index["Price"].mean()
age_analysis_nunique = age_analysis_index["SN"].nunique()
age_analysis_normalized = age_analysis_sum/age_analysis_nunique

Average_Spending = pd.DataFrame({"Items Bought": age_analysis_count, "Total Purchase Value": age_analysis_sum, 
                                 "Average Amount Spent": age_analysis_mean, "Normalized Value": age_analysis_normalized})

Average_Spending["Average Amount Spent"] = Average_Spending["Average Amount Spent"].map("${:.2f}".format)
Average_Spending["Normalized Value"] = Average_Spending["Normalized Value"].map("${:.2f}".format)
Average_Spending["Total Purchase Value"] = Average_Spending["Total Purchase Value"].map("${:.2f}".format)

Average_Spending
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
      <th>Average Amount Spent</th>
      <th>Items Bought</th>
      <th>Normalized Value</th>
      <th>Total Purchase Value</th>
    </tr>
    <tr>
      <th>Age Breakdown</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>&lt; 10</th>
      <td>$3.02</td>
      <td>32</td>
      <td>$4.39</td>
      <td>$96.62</td>
    </tr>
    <tr>
      <th>10 to 14</th>
      <td>$2.87</td>
      <td>78</td>
      <td>$4.15</td>
      <td>$224.15</td>
    </tr>
    <tr>
      <th>15 to 19</th>
      <td>$2.87</td>
      <td>184</td>
      <td>$3.80</td>
      <td>$528.74</td>
    </tr>
    <tr>
      <th>20 to 24</th>
      <td>$2.96</td>
      <td>305</td>
      <td>$3.86</td>
      <td>$902.61</td>
    </tr>
    <tr>
      <th>25 to 29</th>
      <td>$2.89</td>
      <td>76</td>
      <td>$4.23</td>
      <td>$219.82</td>
    </tr>
    <tr>
      <th>30 to 34</th>
      <td>$3.07</td>
      <td>58</td>
      <td>$4.05</td>
      <td>$178.26</td>
    </tr>
    <tr>
      <th>35 to 39</th>
      <td>$2.90</td>
      <td>44</td>
      <td>$5.10</td>
      <td>$127.49</td>
    </tr>
    <tr>
      <th>40 +</th>
      <td>$2.88</td>
      <td>3</td>
      <td>$2.88</td>
      <td>$8.64</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Top Spender
top_spender_df = data_df[["Item ID", "SN", "Price"]]

top_spender_index = top_spender_df.groupby(["SN"]).count()
top_spender_sum = top_spender_df.groupby(["SN"]).sum()
top_spender_index["Price"] = top_spender_sum["Price"]
top_spender_index["Average Spent"] = (top_spender_index["Price"]/top_spender_index["Item ID"]).round(2)

top_spender_final = top_spender_index.sort_values(["Price"], ascending=False)
top_spender_final = top_spender_final.rename(columns={"Item ID":"Items Purchased","Price":"Total Amount Spent",
                                            "Average Spent":"Average Spent"})

top_spender_final["Total Amount Spent"] = top_spender_final["Total Amount Spent"].map("${:.2f}".format)
top_spender_final["Average Spent"] = top_spender_final["Average Spent"].map("${:.2f}".format)

top_spender_final.head()
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
      <th>Items Purchased</th>
      <th>Total Amount Spent</th>
      <th>Average Spent</th>
    </tr>
    <tr>
      <th>SN</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Undirrala66</th>
      <td>5</td>
      <td>$17.06</td>
      <td>$3.41</td>
    </tr>
    <tr>
      <th>Saedue76</th>
      <td>4</td>
      <td>$13.56</td>
      <td>$3.39</td>
    </tr>
    <tr>
      <th>Mindimnya67</th>
      <td>4</td>
      <td>$12.74</td>
      <td>$3.18</td>
    </tr>
    <tr>
      <th>Haellysu29</th>
      <td>3</td>
      <td>$12.73</td>
      <td>$4.24</td>
    </tr>
    <tr>
      <th>Eoda93</th>
      <td>3</td>
      <td>$11.58</td>
      <td>$3.86</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Most Popular Item - Purchsed
top_item_df = data_df[["Item ID", "SN", "Price", "Item Name"]]

top_item_index = top_item_df.groupby(["Item ID", "Item Name"]).count()
top_item_sum = top_item_df.groupby(["Item ID", "Item Name"]).sum()
top_item_index["Price"] = top_item_sum["Price"]
top_item_index["Average Price"] = top_item_index["Price"]/top_item_index["SN"]

top_item_final = top_item_index.sort_values(["SN"], ascending=False)
top_item_final = top_item_final.rename(columns={"SN":"Items Purchased","Price":"Revenue","Average Price":"Price Per Item"})

top_item_final["Revenue"] = top_item_final["Revenue"].map("${:.2f}".format)
top_item_final["Price Per Item"] = top_item_final["Price Per Item"].map("${:.2f}".format)

top_item_final.head()
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
      <th></th>
      <th>Items Purchased</th>
      <th>Revenue</th>
      <th>Price Per Item</th>
    </tr>
    <tr>
      <th>Item ID</th>
      <th>Item Name</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>39</th>
      <th>Betrayal, Whisper of Grieving Widows</th>
      <td>11</td>
      <td>$25.85</td>
      <td>$2.35</td>
    </tr>
    <tr>
      <th>84</th>
      <th>Arcane Gem</th>
      <td>11</td>
      <td>$24.53</td>
      <td>$2.23</td>
    </tr>
    <tr>
      <th>31</th>
      <th>Trickster</th>
      <td>9</td>
      <td>$18.63</td>
      <td>$2.07</td>
    </tr>
    <tr>
      <th>175</th>
      <th>Woeful Adamantite Claymore</th>
      <td>9</td>
      <td>$11.16</td>
      <td>$1.24</td>
    </tr>
    <tr>
      <th>13</th>
      <th>Serenity</th>
      <td>9</td>
      <td>$13.41</td>
      <td>$1.49</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Most Popular Item - Price
top_item_df = data_df[["Item ID", "SN", "Price", "Item Name"]]

top_item = top_item_df.groupby(["Item ID", "Item Name"]).count()
top_item2 = top_item_df.groupby(["Item ID", "Item Name"]).sum()
top_item["Price"] = top_item2["Price"]
top_item3 = top_item["Price"]/top_item["SN"]
top_item["Average Price"] = top_item3

top_item4 = top_item.sort_values(["Price"], ascending=False)
top_item4 = top_item4.rename(columns={"SN":"Items Purchased","Price":"Total Revenue","Average Price":"Price Per Item"})

top_item4["Total Revenue"] = top_item4["Total Revenue"].map("${:.2f}".format)
top_item4["Price Per Item"] = top_item4["Price Per Item"].map("${:.2f}".format)

top_item4.head()
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
      <th></th>
      <th>Items Purchased</th>
      <th>Total Revenue</th>
      <th>Price Per Item</th>
    </tr>
    <tr>
      <th>Item ID</th>
      <th>Item Name</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>34</th>
      <th>Retribution Axe</th>
      <td>9</td>
      <td>$37.26</td>
      <td>$4.14</td>
    </tr>
    <tr>
      <th>115</th>
      <th>Spectral Diamond Doomblade</th>
      <td>7</td>
      <td>$29.75</td>
      <td>$4.25</td>
    </tr>
    <tr>
      <th>32</th>
      <th>Orenmir</th>
      <td>6</td>
      <td>$29.70</td>
      <td>$4.95</td>
    </tr>
    <tr>
      <th>103</th>
      <th>Singed Scalpel</th>
      <td>6</td>
      <td>$29.22</td>
      <td>$4.87</td>
    </tr>
    <tr>
      <th>107</th>
      <th>Splitter, Foe Of Subtlety</th>
      <td>8</td>
      <td>$28.88</td>
      <td>$3.61</td>
    </tr>
  </tbody>
</table>
</div>


