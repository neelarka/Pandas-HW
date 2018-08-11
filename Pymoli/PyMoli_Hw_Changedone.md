# Description
-

```python
# Dependencies and Setup
import pandas as pd
import numpy as np

# Raw data file
file_to_load = "Resources/purchase_data.csv"

# Read purchasing file and store into pandas data frame
purchase_data = pd.read_csv(file_to_load)
purchase_data.head()
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
      <th>Purchase ID</th>
      <th>SN</th>
      <th>Age</th>
      <th>Gender</th>
      <th>Item ID</th>
      <th>Item Name</th>
      <th>Price</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>Lisim78</td>
      <td>20</td>
      <td>Male</td>
      <td>108</td>
      <td>Extraction, Quickblade Of Trembling Hands</td>
      <td>3.53</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1</td>
      <td>Lisovynya38</td>
      <td>40</td>
      <td>Male</td>
      <td>143</td>
      <td>Frenzied Scimitar</td>
      <td>1.56</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2</td>
      <td>Ithergue48</td>
      <td>24</td>
      <td>Male</td>
      <td>92</td>
      <td>Final Critic</td>
      <td>4.88</td>
    </tr>
    <tr>
      <th>3</th>
      <td>3</td>
      <td>Chamassasya86</td>
      <td>24</td>
      <td>Male</td>
      <td>100</td>
      <td>Blindscythe</td>
      <td>3.27</td>
    </tr>
    <tr>
      <th>4</th>
      <td>4</td>
      <td>Iskosia90</td>
      <td>23</td>
      <td>Male</td>
      <td>131</td>
      <td>Fury</td>
      <td>1.44</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Player Count (unique)
Total_players = purchase_data["SN"].unique()
Total_players_count = len(Total_players)
summary_Total_Players = pd.DataFrame([{"Total Players": Total_players_count}])   
summary_Total_Players
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
      <td>576</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Purchasing Analysis (Total)

Num_of_Unique_items = len(purchase_data["Item ID"].unique())
# Number of Purchases
purchase_data["Purchase ID"].count()
# Average Price
purchase_data["Price"].mean()
# Total Revenue
purchase_data["Price"].sum()

#Creating a summary data frame to hold the results
summary = pd.DataFrame([
    {"Number of Unique Items": Num_of_Unique_items, "Average Price": purchase_data["Price"].mean(),
     "Number of Purchases": purchase_data["Purchase ID"].count(), "Total Revenue": purchase_data["Price"].sum()}])
organized_summary = summary[["Number of Unique Items","Average Price","Number of Purchases","Total Revenue"]]

# Giving the displayed data cleaner formatting
organized_summary["Average Price"] = organized_summary["Average Price"].map("${:.2f}".format)
organized_summary["Total Revenue"] = organized_summary["Total Revenue"].map("${:.2f}".format)

#Displaying the summary data frame
organized_summary
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
      <th>Number of Unique Items</th>
      <th>Average Price</th>
      <th>Number of Purchases</th>
      <th>Total Revenue</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>183</td>
      <td>$3.05</td>
      <td>780</td>
      <td>$2379.77</td>
    </tr>
  </tbody>
</table>
</div>




```python
## Gender Demographics

gender_group = purchase_data.groupby("Gender")

# Count of Players
unique_members = gender_group["SN"].unique()
females = unique_members["Female"].size
males =  unique_members["Male"].size
non_disclosed = unique_members["Other / Non-Disclosed"].size
unique_members_list = [females, males, non_disclosed]

# Percentage of Players
Percent_of_players_list = []
for elements in unique_members_list :
    Percent_of_players = (elements/Total_players_count)*100
    Percent_of_players_list.append(Percent_of_players)

#Creating a summary data frame to hold the results
summary_Gender_original = pd.DataFrame({
    "Percentage of Players": Percent_of_players_list,
    "Total Count": unique_members_list}, index = ['Female', 'Male', 'Other/Non-Disclosed'] )

# Use Map to format all the Percentage of Players column
summary_Gender_original["Percentage of Players"] = summary_Gender_original["Percentage of Players"].map("{:.2f} %".format)

#Displaying the summary data frame
summary_Gender_original 
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
      <th>Female</th>
      <td>14.06 %</td>
      <td>81</td>
    </tr>
    <tr>
      <th>Male</th>
      <td>84.03 %</td>
      <td>484</td>
    </tr>
    <tr>
      <th>Other/Non-Disclosed</th>
      <td>1.91 %</td>
      <td>11</td>
    </tr>
  </tbody>
</table>
</div>




```python
## Purchasing Analysis (Gender)

purchase_data_gender_group = purchase_data.groupby("Gender")

Total_Purchase_Value = purchase_data_gender_group["Price"].sum()

## Calculation for Average Purchase Total Per Person
Avg_Purchase_Total_per_Person_list = []
for i in range(0, 3):
    Avg_Purchase_Total_per_Person  =  Total_Purchase_Value[i]/unique_members_list[i]
    Avg_Purchase_Total_per_Person_list.append(Avg_Purchase_Total_per_Person)

#Creating a summary data frame to hold the results
summary_Purchase_Analysis_Gender = pd.DataFrame({
    "Purchase Count": purchase_data_gender_group["Purchase ID"].count(),
    "Average Purchase Price": purchase_data_gender_group["Price"].mean(), "Total Purchase Value": purchase_data_gender_group["Price"].sum(), "Avg Purchase Total per Person": Avg_Purchase_Total_per_Person_list})

# Giving the displayed data cleaner formatting
summary_Purchase_Analysis_Gender["Average Purchase Price"] = summary_Purchase_Analysis_Gender["Average Purchase Price"].map("${:.2f}".format)
summary_Purchase_Analysis_Gender["Avg Purchase Total per Person"] =  summary_Purchase_Analysis_Gender["Avg Purchase Total per Person"].map("${:.2f}".format)
summary_Purchase_Analysis_Gender["Total Purchase Value"] =  summary_Purchase_Analysis_Gender["Total Purchase Value"].map("${:.2f}".format)

#Displaying the summary data frame
summary_Purchase_Analysis_Gender
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
      <th>Purchase Count</th>
      <th>Average Purchase Price</th>
      <th>Total Purchase Value</th>
      <th>Avg Purchase Total per Person</th>
    </tr>
    <tr>
      <th>Gender</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Female</th>
      <td>113</td>
      <td>$3.20</td>
      <td>$361.94</td>
      <td>$4.47</td>
    </tr>
    <tr>
      <th>Male</th>
      <td>652</td>
      <td>$3.02</td>
      <td>$1967.64</td>
      <td>$4.07</td>
    </tr>
    <tr>
      <th>Other / Non-Disclosed</th>
      <td>15</td>
      <td>$3.35</td>
      <td>$50.19</td>
      <td>$4.56</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Age Demographics

# Establish bins for ages
age_bins = [0, 9.90, 14.90, 19.90, 24.90, 29.90, 34.90, 39.90, 99999]

group_names = ["<10", "10-14", "15-19", "20-24", "25-29", "30-34", "35-39", "40+"]

# Categorizing the existing players using the age bins.
purchase_data["Age Summary"] = pd.cut(purchase_data["Age"], age_bins, labels=group_names)

purchase_data_group = purchase_data.groupby("Age Summary")

# Calculating the numbers and percentages by age group
Total_count_by_age = purchase_data_group["SN"].unique()
Total_count_list = []
Percentage_of_players_by_age_list = []
for i in range(0, 8):
    Total_count = len(Total_count_by_age[i])
    Percentage_of_players_by_age = (Total_count/Total_players_count)*100
    Total_count_list.append(Total_count)
    Percentage_of_players_by_age_list.append(Percentage_of_players_by_age)

# Creating a summary data frame to hold the results
summary_Age_Demographics = pd.DataFrame({
    "Percentage of Players": Percentage_of_players_by_age_list,
    "Total Count": Total_count_list}, index = group_names )

# Rounding the percentage column to two decimal points
summary_Age_Demographics["Percentage of Players"] = summary_Age_Demographics["Percentage of Players"].map("{:.2f} %".format)

# Displaying Age Demographics Table
summary_Age_Demographics
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
      <th>&lt;10</th>
      <td>2.95 %</td>
      <td>17</td>
    </tr>
    <tr>
      <th>10-14</th>
      <td>3.82 %</td>
      <td>22</td>
    </tr>
    <tr>
      <th>15-19</th>
      <td>18.58 %</td>
      <td>107</td>
    </tr>
    <tr>
      <th>20-24</th>
      <td>44.79 %</td>
      <td>258</td>
    </tr>
    <tr>
      <th>25-29</th>
      <td>13.37 %</td>
      <td>77</td>
    </tr>
    <tr>
      <th>30-34</th>
      <td>9.03 %</td>
      <td>52</td>
    </tr>
    <tr>
      <th>35-39</th>
      <td>5.38 %</td>
      <td>31</td>
    </tr>
    <tr>
      <th>40+</th>
      <td>2.08 %</td>
      <td>12</td>
    </tr>
  </tbody>
</table>
</div>




```python
## Purchasing Analysis (Age)

# Bin the purchase_data data frame by age
age_bins = [0, 9.90, 14.90, 19.90, 24.90, 29.90, 34.90, 39.90, 99999]
group_names = ["<10", "10-14", "15-19", "20-24", "25-29", "30-34", "35-39", "40+"]

# Categorizing the existing players using the age bins.
purchase_data["Age Summary"] = pd.cut(purchase_data["Age"], age_bins, labels=group_names)
purchase_data_group = purchase_data.groupby("Age Summary")

# Calculating the unique members count
Total_Purchase_value = purchase_data_group["Price"].sum()
Total_count = summary_Age_Demographics["Total Count"]
Avg_Purchase_Total_per_Person  =  purchase_data_group["Price"].sum()/summary_Age_Demographics["Total Count"]
    
# creating a summary data frame to hold the results for  purchase count, avg. purchase price, avg. purchase total per person etc.
summary_Purchase_Analysis_Age = pd.DataFrame({
    "Purchase Count": purchase_data_group["Purchase ID"].count(),
    "Average Purchase Price": purchase_data_group["Price"].mean(), "Total Purchase Value": purchase_data_group["Price"].sum(), "Average Purchase Total per Person"
:Avg_Purchase_Total_per_Person})

#Rounding the Average Purchase Price and Average Purchase Total per Person column to two decimal points
summary_Purchase_Analysis_Age["Average Purchase Price"] = summary_Purchase_Analysis_Age["Average Purchase Price"].map("${:.2f}".format)
summary_Purchase_Analysis_Age["Average Purchase Total per Person"] = summary_Purchase_Analysis_Age["Average Purchase Total per Person"].map("${:.2f}".format)
summary_Purchase_Analysis_Age["Total Purchase Value"] = summary_Purchase_Analysis_Age["Total Purchase Value"].map("${:.2f}".format)
# Displaying Purchasing Analysis (Age) table
summary_Purchase_Analysis_Age
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
      <th>Purchase Count</th>
      <th>Average Purchase Price</th>
      <th>Total Purchase Value</th>
      <th>Average Purchase Total per Person</th>
    </tr>
    <tr>
      <th>Age Summary</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>&lt;10</th>
      <td>23</td>
      <td>$3.35</td>
      <td>$77.13</td>
      <td>$4.54</td>
    </tr>
    <tr>
      <th>10-14</th>
      <td>28</td>
      <td>$2.96</td>
      <td>$82.78</td>
      <td>$3.76</td>
    </tr>
    <tr>
      <th>15-19</th>
      <td>136</td>
      <td>$3.04</td>
      <td>$412.89</td>
      <td>$3.86</td>
    </tr>
    <tr>
      <th>20-24</th>
      <td>365</td>
      <td>$3.05</td>
      <td>$1114.06</td>
      <td>$4.32</td>
    </tr>
    <tr>
      <th>25-29</th>
      <td>101</td>
      <td>$2.90</td>
      <td>$293.00</td>
      <td>$3.81</td>
    </tr>
    <tr>
      <th>30-34</th>
      <td>73</td>
      <td>$2.93</td>
      <td>$214.00</td>
      <td>$4.12</td>
    </tr>
    <tr>
      <th>35-39</th>
      <td>41</td>
      <td>$3.60</td>
      <td>$147.67</td>
      <td>$4.76</td>
    </tr>
    <tr>
      <th>40+</th>
      <td>13</td>
      <td>$2.94</td>
      <td>$38.24</td>
      <td>$3.19</td>
    </tr>
  </tbody>
</table>
</div>




```python
## Top Spenders

purchase_data_SN_group = purchase_data.groupby("SN")

# Creating a summary data frame to hold the results
summary_Purchase_Analysis_SN = pd.DataFrame({
    "Purchase Count": purchase_data_SN_group["Purchase ID"].count(),
    "Average Purchase Price": purchase_data_SN_group["Price"].mean(), "Total Purchase Value": purchase_data_SN_group["Price"].sum(), 
 })
    
# Sorting the total purchase value column in descending order
summary_Purchase_Analysis_SN_sorted = summary_Purchase_Analysis_SN.sort_values("Total Purchase Value", ascending=False)

# Giving the displayed data cleaner formatting
summary_Purchase_Analysis_SN_sorted["Average Purchase Price"] = summary_Purchase_Analysis_SN_sorted["Average Purchase Price"].map("${:.2f}".format)
summary_Purchase_Analysis_SN_sorted["Total Purchase Value"] = summary_Purchase_Analysis_SN_sorted["Total Purchase Value"].map("${:.2f}".format)
# Displaying the Top Spenders table
summary_Purchase_Analysis_SN_sorted.head()
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
      <th>Purchase Count</th>
      <th>Average Purchase Price</th>
      <th>Total Purchase Value</th>
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
      <th>Lisosia93</th>
      <td>5</td>
      <td>$3.79</td>
      <td>$18.96</td>
    </tr>
    <tr>
      <th>Idastidru52</th>
      <td>4</td>
      <td>$3.86</td>
      <td>$15.45</td>
    </tr>
    <tr>
      <th>Chamjask73</th>
      <td>3</td>
      <td>$4.61</td>
      <td>$13.83</td>
    </tr>
    <tr>
      <th>Iral74</th>
      <td>4</td>
      <td>$3.40</td>
      <td>$13.62</td>
    </tr>
    <tr>
      <th>Iskadarya95</th>
      <td>3</td>
      <td>$4.37</td>
      <td>$13.10</td>
    </tr>
  </tbody>
</table>
</div>




```python
## Most Popular Items

# Creating a summary data frame to hold the results
purchase_data_item_id_group = purchase_data.groupby(["Item ID", "Item Name"])
summary_Purchase_Analysis_item_id = pd.DataFrame({
    "Purchase Count": purchase_data_item_id_group["Purchase ID"].count(),
    "Average Purchase Price": purchase_data_item_id_group["Price"].mean(), "Total Purchase Value": purchase_data_item_id_group["Price"].sum() 
 })

# Sorting the purchase count column in descending order
summary_Purchase_Analysis_item_id_sorted = summary_Purchase_Analysis_item_id.sort_values("Purchase Count", ascending=False)

# Giving the displayed data cleaner formatting
summary_Purchase_Analysis_item_id_sorted["Average Purchase Price"] = summary_Purchase_Analysis_item_id_sorted["Average Purchase Price"].map("${:.2f}".format)
summary_Purchase_Analysis_item_id_sorted["Total Purchase Value"] = summary_Purchase_Analysis_item_id_sorted["Total Purchase Value"].map("${:.2f}".format)

# Displaying the Most Popular Items table
summary_Purchase_Analysis_item_id_sorted.head()
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
      <th>Purchase Count</th>
      <th>Average Purchase Price</th>
      <th>Total Purchase Value</th>
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
      <th>178</th>
      <th>Oathbreaker, Last Hope of the Breaking Storm</th>
      <td>12</td>
      <td>$4.23</td>
      <td>$50.76</td>
    </tr>
    <tr>
      <th>145</th>
      <th>Fiery Glass Crusader</th>
      <td>9</td>
      <td>$4.58</td>
      <td>$41.22</td>
    </tr>
    <tr>
      <th>108</th>
      <th>Extraction, Quickblade Of Trembling Hands</th>
      <td>9</td>
      <td>$3.53</td>
      <td>$31.77</td>
    </tr>
    <tr>
      <th>82</th>
      <th>Nirvana</th>
      <td>9</td>
      <td>$4.90</td>
      <td>$44.10</td>
    </tr>
    <tr>
      <th>19</th>
      <th>Pursuit, Cudgel of Necromancy</th>
      <td>8</td>
      <td>$1.02</td>
      <td>$8.16</td>
    </tr>
  </tbody>
</table>
</div>




```python
## Most Profitable Items

purchase_data_item_id_group = purchase_data.groupby(["Item ID", "Item Name"])

# Creating a summary data frame to hold the results
summary_Purchase_Analysis_item_id = pd.DataFrame({
    "Purchase Count": purchase_data_item_id_group["Purchase ID"].count(),
    "Average Purchase Price": purchase_data_item_id_group["Price"].mean(), "Total Purchase Value": purchase_data_item_id_group["Price"].sum() 
 })

# Sorting the  total purchase value column in descending order
summary_Purchase_Analysis_item_sorted = summary_Purchase_Analysis_item_id.sort_values("Total Purchase Value", ascending=False)

# Giving the displayed data cleaner formatting
summary_Purchase_Analysis_item_sorted["Average Purchase Price"] = summary_Purchase_Analysis_item_sorted["Average Purchase Price"].map("${:.2f}".format)
summary_Purchase_Analysis_item_sorted["Total Purchase Value"] = summary_Purchase_Analysis_item_sorted["Total Purchase Value"].map("${:.2f}".format)

# Displaying the Most Profitable Items table
summary_Purchase_Analysis_item_sorted.head()

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
      <th>Purchase Count</th>
      <th>Average Purchase Price</th>
      <th>Total Purchase Value</th>
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
      <th>178</th>
      <th>Oathbreaker, Last Hope of the Breaking Storm</th>
      <td>12</td>
      <td>$4.23</td>
      <td>$50.76</td>
    </tr>
    <tr>
      <th>82</th>
      <th>Nirvana</th>
      <td>9</td>
      <td>$4.90</td>
      <td>$44.10</td>
    </tr>
    <tr>
      <th>145</th>
      <th>Fiery Glass Crusader</th>
      <td>9</td>
      <td>$4.58</td>
      <td>$41.22</td>
    </tr>
    <tr>
      <th>92</th>
      <th>Final Critic</th>
      <td>8</td>
      <td>$4.88</td>
      <td>$39.04</td>
    </tr>
    <tr>
      <th>103</th>
      <th>Singed Scalpel</th>
      <td>8</td>
      <td>$4.35</td>
      <td>$34.80</td>
    </tr>
  </tbody>
</table>
</div>


