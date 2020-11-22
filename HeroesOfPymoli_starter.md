### Note
* Instructions have been included for each segment. You do not have to follow them exactly, but they are included to help you think through the steps.


```python
# Dependencies and Setup
import pandas as pd

# File to Load (Remember to Change These)
file_to_load = "Resources/purchase_data.csv"

# Read Purchasing File and store into Pandas data frame
purchase_data = pd.read_csv(file_to_load)
```

## Player Count

* Display the total number of players



```python
#Find unique player count
total_players = purchase_data["SN"].nunique()
total_players
```




    576



## Purchasing Analysis (Total)

* Run basic calculations to obtain number of unique items, average price, etc.


* Create a summary data frame to hold the results


* Optional: give the displayed data cleaner formatting


* Display the summary data frame



```python
#calculate unique items, average price, number of purchases, and total purchase amount

unique_items = purchase_data["Item Name"].nunique()
avg_price = purchase_data["Price"].mean()
purchases = purchase_data["Purchase ID"].count()
rev = purchase_data["Price"].sum()

#create dataframe of variables
totals = pd.DataFrame({
    'Number of Unique Items': [unique_items], 
    'Average Price': [avg_price], 
    'Number of Purchases': [purchases], 
    'Total Revenue': [rev]
})
#update fromat to currency
totals["Total Revenue"] = totals["Total Revenue"].map("${:,.2f}".format)
totals["Average Price"] = totals["Average Price"].map("${:,.2f}".format)

#output in notebook
totals
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
      <td>179</td>
      <td>$3.05</td>
      <td>780</td>
      <td>$2,379.77</td>
    </tr>
  </tbody>
</table>
</div>



## Gender Demographics

* Percentage and Count of Male Players


* Percentage and Count of Female Players


* Percentage and Count of Other / Non-Disclosed



```python
#group by gender
gender_group = purchase_data.groupby("Gender")

#count unique occurrence by gender and total
gender_count = gender_group["SN"].nunique()
total_gender = gender_count.sum()
#percent of total
gen_percent = gender_count/total_gender * 100

#combine into dataframe
gen_demo = pd.DataFrame({
     "Total Count": gender_count,
    "Percentage of Players" : gen_percent
})

#sort and style
sorted_demo = gen_demo.sort_values(['Total Count'], ascending = False).style.format({"Percentage of Players":"{:.2f}%"})

#output in notebook
sorted_demo
```




<style  type="text/css" >
</style><table id="T_96f6520d_2c6b_11eb_96bc_ccd9acc89a91" ><thead>    <tr>        <th class="blank level0" ></th>        <th class="col_heading level0 col0" >Total Count</th>        <th class="col_heading level0 col1" >Percentage of Players</th>    </tr>    <tr>        <th class="index_name level0" >Gender</th>        <th class="blank" ></th>        <th class="blank" ></th>    </tr></thead><tbody>
                <tr>
                        <th id="T_96f6520d_2c6b_11eb_96bc_ccd9acc89a91level0_row0" class="row_heading level0 row0" >Male</th>
                        <td id="T_96f6520d_2c6b_11eb_96bc_ccd9acc89a91row0_col0" class="data row0 col0" >484</td>
                        <td id="T_96f6520d_2c6b_11eb_96bc_ccd9acc89a91row0_col1" class="data row0 col1" >84.03%</td>
            </tr>
            <tr>
                        <th id="T_96f6520d_2c6b_11eb_96bc_ccd9acc89a91level0_row1" class="row_heading level0 row1" >Female</th>
                        <td id="T_96f6520d_2c6b_11eb_96bc_ccd9acc89a91row1_col0" class="data row1 col0" >81</td>
                        <td id="T_96f6520d_2c6b_11eb_96bc_ccd9acc89a91row1_col1" class="data row1 col1" >14.06%</td>
            </tr>
            <tr>
                        <th id="T_96f6520d_2c6b_11eb_96bc_ccd9acc89a91level0_row2" class="row_heading level0 row2" >Other / Non-Disclosed</th>
                        <td id="T_96f6520d_2c6b_11eb_96bc_ccd9acc89a91row2_col0" class="data row2 col0" >11</td>
                        <td id="T_96f6520d_2c6b_11eb_96bc_ccd9acc89a91row2_col1" class="data row2 col1" >1.91%</td>
            </tr>
    </tbody></table>




## Purchasing Analysis (Gender)

* Run basic calculations to obtain purchase count, avg. purchase price, avg. purchase total per person etc. by gender




* Create a summary data frame to hold the results


* Optional: give the displayed data cleaner formatting


* Display the summary data frame


```python
#groupby Gender
gender_group = purchase_data.groupby("Gender")

#calculate variables
gender_count = gender_group["SN"].nunique()
pur_count = gender_group["Purchase ID"].count()
total_pur = gender_group["Price"].sum()
avg_pur = total_pur/pur_count
avg_per = total_pur/gender_count

#create summary dataframe

pur_analysis = pd.DataFrame({
    "Purchase Count": pur_count,
    "Average Purchase Price": avg_pur,
    "Total Purchase Value": total_pur,
    "Avg Total Purchase per Person": avg_per
})

#format
pur_analysis["Average Purchase Price"] = pur_analysis["Average Purchase Price"].map("${:,.2f}".format)
pur_analysis["Total Purchase Value"] = pur_analysis["Total Purchase Value"].map("${:,.2f}".format)
pur_analysis["Avg Total Purchase per Person"] = pur_analysis["Avg Total Purchase per Person"].map("${:,.2f}".format)

#output in notebook
pur_analysis
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
      <th>Avg Total Purchase per Person</th>
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
      <td>$1,967.64</td>
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



## Age Demographics

* Establish bins for ages


* Categorize the existing players using the age bins. Hint: use pd.cut()


* Calculate the numbers and percentages by age group


* Create a summary data frame to hold the results


* Optional: round the percentage column to two decimal points


* Display Age Demographics Table



```python
#create bins
bins = [0, 9, 14, 19, 24, 29, 34, 39, 1000]

#lables
bin_labels =["<10", "10-14", "15-19", "20-24", "25-29", "30-34", "35-39", "40+"]

#bin
pd.cut(purchase_data["Age"], bins, labels= bin_labels)
purchase_data["Age Group"] = pd.cut(purchase_data["Age"], bins, labels= bin_labels)

#group by Age Group
age_group = purchase_data.groupby("Age Group")
person_count = age_group["SN"].nunique()
total_count = person_count.sum()
per_total = person_count/total_count * 100

#combine into dataframe
age_demo = pd.DataFrame({
    "Count": person_count,
    "Percentage of Players" : per_total
})

#style
sorted_age_demo = age_demo.style.format({"Percentage of Players":"{:.2f}%"})

#output in notebook
sorted_age_demo

```




<style  type="text/css" >
</style><table id="T_4385961c_2c6e_11eb_b6a6_ccd9acc89a91" ><thead>    <tr>        <th class="blank level0" ></th>        <th class="col_heading level0 col0" >Count</th>        <th class="col_heading level0 col1" >Percentage of Players</th>    </tr>    <tr>        <th class="index_name level0" >Age Group</th>        <th class="blank" ></th>        <th class="blank" ></th>    </tr></thead><tbody>
                <tr>
                        <th id="T_4385961c_2c6e_11eb_b6a6_ccd9acc89a91level0_row0" class="row_heading level0 row0" ><10</th>
                        <td id="T_4385961c_2c6e_11eb_b6a6_ccd9acc89a91row0_col0" class="data row0 col0" >17</td>
                        <td id="T_4385961c_2c6e_11eb_b6a6_ccd9acc89a91row0_col1" class="data row0 col1" >2.95%</td>
            </tr>
            <tr>
                        <th id="T_4385961c_2c6e_11eb_b6a6_ccd9acc89a91level0_row1" class="row_heading level0 row1" >10-14</th>
                        <td id="T_4385961c_2c6e_11eb_b6a6_ccd9acc89a91row1_col0" class="data row1 col0" >22</td>
                        <td id="T_4385961c_2c6e_11eb_b6a6_ccd9acc89a91row1_col1" class="data row1 col1" >3.82%</td>
            </tr>
            <tr>
                        <th id="T_4385961c_2c6e_11eb_b6a6_ccd9acc89a91level0_row2" class="row_heading level0 row2" >15-19</th>
                        <td id="T_4385961c_2c6e_11eb_b6a6_ccd9acc89a91row2_col0" class="data row2 col0" >107</td>
                        <td id="T_4385961c_2c6e_11eb_b6a6_ccd9acc89a91row2_col1" class="data row2 col1" >18.58%</td>
            </tr>
            <tr>
                        <th id="T_4385961c_2c6e_11eb_b6a6_ccd9acc89a91level0_row3" class="row_heading level0 row3" >20-24</th>
                        <td id="T_4385961c_2c6e_11eb_b6a6_ccd9acc89a91row3_col0" class="data row3 col0" >258</td>
                        <td id="T_4385961c_2c6e_11eb_b6a6_ccd9acc89a91row3_col1" class="data row3 col1" >44.79%</td>
            </tr>
            <tr>
                        <th id="T_4385961c_2c6e_11eb_b6a6_ccd9acc89a91level0_row4" class="row_heading level0 row4" >25-29</th>
                        <td id="T_4385961c_2c6e_11eb_b6a6_ccd9acc89a91row4_col0" class="data row4 col0" >77</td>
                        <td id="T_4385961c_2c6e_11eb_b6a6_ccd9acc89a91row4_col1" class="data row4 col1" >13.37%</td>
            </tr>
            <tr>
                        <th id="T_4385961c_2c6e_11eb_b6a6_ccd9acc89a91level0_row5" class="row_heading level0 row5" >30-34</th>
                        <td id="T_4385961c_2c6e_11eb_b6a6_ccd9acc89a91row5_col0" class="data row5 col0" >52</td>
                        <td id="T_4385961c_2c6e_11eb_b6a6_ccd9acc89a91row5_col1" class="data row5 col1" >9.03%</td>
            </tr>
            <tr>
                        <th id="T_4385961c_2c6e_11eb_b6a6_ccd9acc89a91level0_row6" class="row_heading level0 row6" >35-39</th>
                        <td id="T_4385961c_2c6e_11eb_b6a6_ccd9acc89a91row6_col0" class="data row6 col0" >31</td>
                        <td id="T_4385961c_2c6e_11eb_b6a6_ccd9acc89a91row6_col1" class="data row6 col1" >5.38%</td>
            </tr>
            <tr>
                        <th id="T_4385961c_2c6e_11eb_b6a6_ccd9acc89a91level0_row7" class="row_heading level0 row7" >40+</th>
                        <td id="T_4385961c_2c6e_11eb_b6a6_ccd9acc89a91row7_col0" class="data row7 col0" >12</td>
                        <td id="T_4385961c_2c6e_11eb_b6a6_ccd9acc89a91row7_col1" class="data row7 col1" >2.08%</td>
            </tr>
    </tbody></table>



## Purchasing Analysis (Age)

* Bin the purchase_data data frame by age


* Run basic calculations to obtain purchase count, avg. purchase price, avg. purchase total per person etc. in the table below


* Create a summary data frame to hold the results


* Optional: give the displayed data cleaner formatting


* Display the summary data frame


```python
#create bins
bins = [0, 9, 14, 19, 24, 29, 34, 39, 1000]

#lables
bin_labels =["<10", "10-14", "15-19", "20-24", "25-29", "30-34", "35-39", "40+"]

#bin
pd.cut(purchase_data["Age"], bins, labels= bin_labels)
purchase_data["Age Group"] = pd.cut(purchase_data["Age"], bins, labels= bin_labels)


#group by Age Group
age_group = purchase_data.groupby("Age Group")
person_count = age_group["SN"].nunique()
pur_count = age_group["Purchase ID"].count()
total_pur = age_group["Price"].sum()
avg_pur = total_pur/pur_count
avg_per = total_pur/person_count

#create summary dataframe

age_analysis = pd.DataFrame({
    "Purchase Count": pur_count,
    "Average Purchase Price": avg_pur,
    "Total Purchase Value": total_pur,
    "Avg Total Purchase per Person": avg_per
})

#format
age_analysis["Average Purchase Price"] = age_analysis["Average Purchase Price"].map("${:,.2f}".format)
age_analysis["Total Purchase Value"] = age_analysis["Total Purchase Value"].map("${:,.2f}".format)
age_analysis["Avg Total Purchase per Person"] = age_analysis["Avg Total Purchase per Person"].map("${:,.2f}".format)

#output to notebook
age_analysis

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
      <th>Avg Total Purchase per Person</th>
    </tr>
    <tr>
      <th>Age Group</th>
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
      <td>$1,114.06</td>
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



## Top Spenders

* Run basic calculations to obtain the results in the table below


* Create a summary data frame to hold the results


* Sort the total purchase value column in descending order


* Optional: give the displayed data cleaner formatting


* Display a preview of the summary data frame




```python
#group by SN
SN_group = purchase_data.groupby("SN")

#calculate values
purchase_count = SN_group["Purchase ID"].count()
total_purchase = SN_group["Price"].sum()
avg_pur = total_purchase/purchase_count

#create dataframe
top_spenders = pd.DataFrame({
    "Purchase Count": purchase_count,
    "Average Purchase Price": avg_pur,
    "Total Purchase Value": total_purchase
})

#sort,format, and filter to top five
sorted_top_spenders = top_spenders.sort_values(['Total Purchase Value'], ascending = False).head(5).style.format({"Total Purchase Value":"${:.2f}", "Average Purchase Price":"${:.2f}"})

#output to notebook
sorted_top_spenders

```




<style  type="text/css" >
</style><table id="T_73f8fca6_2c74_11eb_b263_ccd9acc89a91" ><thead>    <tr>        <th class="blank level0" ></th>        <th class="col_heading level0 col0" >Purchase Count</th>        <th class="col_heading level0 col1" >Average Purchase Price</th>        <th class="col_heading level0 col2" >Total Purchase Value</th>    </tr>    <tr>        <th class="index_name level0" >SN</th>        <th class="blank" ></th>        <th class="blank" ></th>        <th class="blank" ></th>    </tr></thead><tbody>
                <tr>
                        <th id="T_73f8fca6_2c74_11eb_b263_ccd9acc89a91level0_row0" class="row_heading level0 row0" >Lisosia93</th>
                        <td id="T_73f8fca6_2c74_11eb_b263_ccd9acc89a91row0_col0" class="data row0 col0" >5</td>
                        <td id="T_73f8fca6_2c74_11eb_b263_ccd9acc89a91row0_col1" class="data row0 col1" >$3.79</td>
                        <td id="T_73f8fca6_2c74_11eb_b263_ccd9acc89a91row0_col2" class="data row0 col2" >$18.96</td>
            </tr>
            <tr>
                        <th id="T_73f8fca6_2c74_11eb_b263_ccd9acc89a91level0_row1" class="row_heading level0 row1" >Idastidru52</th>
                        <td id="T_73f8fca6_2c74_11eb_b263_ccd9acc89a91row1_col0" class="data row1 col0" >4</td>
                        <td id="T_73f8fca6_2c74_11eb_b263_ccd9acc89a91row1_col1" class="data row1 col1" >$3.86</td>
                        <td id="T_73f8fca6_2c74_11eb_b263_ccd9acc89a91row1_col2" class="data row1 col2" >$15.45</td>
            </tr>
            <tr>
                        <th id="T_73f8fca6_2c74_11eb_b263_ccd9acc89a91level0_row2" class="row_heading level0 row2" >Chamjask73</th>
                        <td id="T_73f8fca6_2c74_11eb_b263_ccd9acc89a91row2_col0" class="data row2 col0" >3</td>
                        <td id="T_73f8fca6_2c74_11eb_b263_ccd9acc89a91row2_col1" class="data row2 col1" >$4.61</td>
                        <td id="T_73f8fca6_2c74_11eb_b263_ccd9acc89a91row2_col2" class="data row2 col2" >$13.83</td>
            </tr>
            <tr>
                        <th id="T_73f8fca6_2c74_11eb_b263_ccd9acc89a91level0_row3" class="row_heading level0 row3" >Iral74</th>
                        <td id="T_73f8fca6_2c74_11eb_b263_ccd9acc89a91row3_col0" class="data row3 col0" >4</td>
                        <td id="T_73f8fca6_2c74_11eb_b263_ccd9acc89a91row3_col1" class="data row3 col1" >$3.40</td>
                        <td id="T_73f8fca6_2c74_11eb_b263_ccd9acc89a91row3_col2" class="data row3 col2" >$13.62</td>
            </tr>
            <tr>
                        <th id="T_73f8fca6_2c74_11eb_b263_ccd9acc89a91level0_row4" class="row_heading level0 row4" >Iskadarya95</th>
                        <td id="T_73f8fca6_2c74_11eb_b263_ccd9acc89a91row4_col0" class="data row4 col0" >3</td>
                        <td id="T_73f8fca6_2c74_11eb_b263_ccd9acc89a91row4_col1" class="data row4 col1" >$4.37</td>
                        <td id="T_73f8fca6_2c74_11eb_b263_ccd9acc89a91row4_col2" class="data row4 col2" >$13.10</td>
            </tr>
    </tbody></table>



## Most Popular Items

* Retrieve the Item ID, Item Name, and Item Price columns


* Group by Item ID and Item Name. Perform calculations to obtain purchase count, average item price, and total purchase value


* Create a summary data frame to hold the results


* Sort the purchase count column in descending order


* Optional: give the displayed data cleaner formatting


* Display a preview of the summary data frame




```python
#dataframe subset
pop_df = purchase_data[["Item ID", "Item Name", "Price"]]

#group by Item ID & Item Name
group_item = pop_df.groupby(['Item ID', 'Item Name'])
purchase_count = group_item["Item ID"].count()
total_purchase = group_item["Price"].sum()
avg_price = total_purchase/purchase_count

pop_items = pd.DataFrame({
    "Purchase Count": purchase_count,
    "Average Item Price": avg_price,
    "Total Purchase Value": total_purchase
})

#sort,format, and filter to top five
sorted_pop_items = pop_items.sort_values(['Purchase Count'], ascending = False).head(5).style.format({"Total Purchase Value":"${:.2f}", "Average Item Price":"${:.2f}"})

#output to notebook
sorted_pop_items

```




<style  type="text/css" >
</style><table id="T_9abeebc3_2c79_11eb_93ee_ccd9acc89a91" ><thead>    <tr>        <th class="blank" ></th>        <th class="blank level0" ></th>        <th class="col_heading level0 col0" >Purchase Count</th>        <th class="col_heading level0 col1" >Average Item Price</th>        <th class="col_heading level0 col2" >Total Purchase Value</th>    </tr>    <tr>        <th class="index_name level0" >Item ID</th>        <th class="index_name level1" >Item Name</th>        <th class="blank" ></th>        <th class="blank" ></th>        <th class="blank" ></th>    </tr></thead><tbody>
                <tr>
                        <th id="T_9abeebc3_2c79_11eb_93ee_ccd9acc89a91level0_row0" class="row_heading level0 row0" >92</th>
                        <th id="T_9abeebc3_2c79_11eb_93ee_ccd9acc89a91level1_row0" class="row_heading level1 row0" >Final Critic</th>
                        <td id="T_9abeebc3_2c79_11eb_93ee_ccd9acc89a91row0_col0" class="data row0 col0" >13</td>
                        <td id="T_9abeebc3_2c79_11eb_93ee_ccd9acc89a91row0_col1" class="data row0 col1" >$4.61</td>
                        <td id="T_9abeebc3_2c79_11eb_93ee_ccd9acc89a91row0_col2" class="data row0 col2" >$59.99</td>
            </tr>
            <tr>
                        <th id="T_9abeebc3_2c79_11eb_93ee_ccd9acc89a91level0_row1" class="row_heading level0 row1" >178</th>
                        <th id="T_9abeebc3_2c79_11eb_93ee_ccd9acc89a91level1_row1" class="row_heading level1 row1" >Oathbreaker, Last Hope of the Breaking Storm</th>
                        <td id="T_9abeebc3_2c79_11eb_93ee_ccd9acc89a91row1_col0" class="data row1 col0" >12</td>
                        <td id="T_9abeebc3_2c79_11eb_93ee_ccd9acc89a91row1_col1" class="data row1 col1" >$4.23</td>
                        <td id="T_9abeebc3_2c79_11eb_93ee_ccd9acc89a91row1_col2" class="data row1 col2" >$50.76</td>
            </tr>
            <tr>
                        <th id="T_9abeebc3_2c79_11eb_93ee_ccd9acc89a91level0_row2" class="row_heading level0 row2" >145</th>
                        <th id="T_9abeebc3_2c79_11eb_93ee_ccd9acc89a91level1_row2" class="row_heading level1 row2" >Fiery Glass Crusader</th>
                        <td id="T_9abeebc3_2c79_11eb_93ee_ccd9acc89a91row2_col0" class="data row2 col0" >9</td>
                        <td id="T_9abeebc3_2c79_11eb_93ee_ccd9acc89a91row2_col1" class="data row2 col1" >$4.58</td>
                        <td id="T_9abeebc3_2c79_11eb_93ee_ccd9acc89a91row2_col2" class="data row2 col2" >$41.22</td>
            </tr>
            <tr>
                        <th id="T_9abeebc3_2c79_11eb_93ee_ccd9acc89a91level0_row3" class="row_heading level0 row3" >132</th>
                        <th id="T_9abeebc3_2c79_11eb_93ee_ccd9acc89a91level1_row3" class="row_heading level1 row3" >Persuasion</th>
                        <td id="T_9abeebc3_2c79_11eb_93ee_ccd9acc89a91row3_col0" class="data row3 col0" >9</td>
                        <td id="T_9abeebc3_2c79_11eb_93ee_ccd9acc89a91row3_col1" class="data row3 col1" >$3.22</td>
                        <td id="T_9abeebc3_2c79_11eb_93ee_ccd9acc89a91row3_col2" class="data row3 col2" >$28.99</td>
            </tr>
            <tr>
                        <th id="T_9abeebc3_2c79_11eb_93ee_ccd9acc89a91level0_row4" class="row_heading level0 row4" >108</th>
                        <th id="T_9abeebc3_2c79_11eb_93ee_ccd9acc89a91level1_row4" class="row_heading level1 row4" >Extraction, Quickblade Of Trembling Hands</th>
                        <td id="T_9abeebc3_2c79_11eb_93ee_ccd9acc89a91row4_col0" class="data row4 col0" >9</td>
                        <td id="T_9abeebc3_2c79_11eb_93ee_ccd9acc89a91row4_col1" class="data row4 col1" >$3.53</td>
                        <td id="T_9abeebc3_2c79_11eb_93ee_ccd9acc89a91row4_col2" class="data row4 col2" >$31.77</td>
            </tr>
    </tbody></table>



## Most Profitable Items

* Sort the above table by total purchase value in descending order


* Optional: give the displayed data cleaner formatting


* Display a preview of the data frame




```python
#sort,format, and filter to top five
sorted_top_items = pop_items.sort_values(['Total Purchase Value'], ascending = False).head(5).style.format({"Total Purchase Value":"${:.2f}", "Average Item Price":"${:.2f}"})

#output to notebook
sorted_top_items
```




<style  type="text/css" >
</style><table id="T_a8882645_2c7a_11eb_96d6_ccd9acc89a91" ><thead>    <tr>        <th class="blank" ></th>        <th class="blank level0" ></th>        <th class="col_heading level0 col0" >Purchase Count</th>        <th class="col_heading level0 col1" >Average Item Price</th>        <th class="col_heading level0 col2" >Total Purchase Value</th>    </tr>    <tr>        <th class="index_name level0" >Item ID</th>        <th class="index_name level1" >Item Name</th>        <th class="blank" ></th>        <th class="blank" ></th>        <th class="blank" ></th>    </tr></thead><tbody>
                <tr>
                        <th id="T_a8882645_2c7a_11eb_96d6_ccd9acc89a91level0_row0" class="row_heading level0 row0" >92</th>
                        <th id="T_a8882645_2c7a_11eb_96d6_ccd9acc89a91level1_row0" class="row_heading level1 row0" >Final Critic</th>
                        <td id="T_a8882645_2c7a_11eb_96d6_ccd9acc89a91row0_col0" class="data row0 col0" >13</td>
                        <td id="T_a8882645_2c7a_11eb_96d6_ccd9acc89a91row0_col1" class="data row0 col1" >$4.61</td>
                        <td id="T_a8882645_2c7a_11eb_96d6_ccd9acc89a91row0_col2" class="data row0 col2" >$59.99</td>
            </tr>
            <tr>
                        <th id="T_a8882645_2c7a_11eb_96d6_ccd9acc89a91level0_row1" class="row_heading level0 row1" >178</th>
                        <th id="T_a8882645_2c7a_11eb_96d6_ccd9acc89a91level1_row1" class="row_heading level1 row1" >Oathbreaker, Last Hope of the Breaking Storm</th>
                        <td id="T_a8882645_2c7a_11eb_96d6_ccd9acc89a91row1_col0" class="data row1 col0" >12</td>
                        <td id="T_a8882645_2c7a_11eb_96d6_ccd9acc89a91row1_col1" class="data row1 col1" >$4.23</td>
                        <td id="T_a8882645_2c7a_11eb_96d6_ccd9acc89a91row1_col2" class="data row1 col2" >$50.76</td>
            </tr>
            <tr>
                        <th id="T_a8882645_2c7a_11eb_96d6_ccd9acc89a91level0_row2" class="row_heading level0 row2" >82</th>
                        <th id="T_a8882645_2c7a_11eb_96d6_ccd9acc89a91level1_row2" class="row_heading level1 row2" >Nirvana</th>
                        <td id="T_a8882645_2c7a_11eb_96d6_ccd9acc89a91row2_col0" class="data row2 col0" >9</td>
                        <td id="T_a8882645_2c7a_11eb_96d6_ccd9acc89a91row2_col1" class="data row2 col1" >$4.90</td>
                        <td id="T_a8882645_2c7a_11eb_96d6_ccd9acc89a91row2_col2" class="data row2 col2" >$44.10</td>
            </tr>
            <tr>
                        <th id="T_a8882645_2c7a_11eb_96d6_ccd9acc89a91level0_row3" class="row_heading level0 row3" >145</th>
                        <th id="T_a8882645_2c7a_11eb_96d6_ccd9acc89a91level1_row3" class="row_heading level1 row3" >Fiery Glass Crusader</th>
                        <td id="T_a8882645_2c7a_11eb_96d6_ccd9acc89a91row3_col0" class="data row3 col0" >9</td>
                        <td id="T_a8882645_2c7a_11eb_96d6_ccd9acc89a91row3_col1" class="data row3 col1" >$4.58</td>
                        <td id="T_a8882645_2c7a_11eb_96d6_ccd9acc89a91row3_col2" class="data row3 col2" >$41.22</td>
            </tr>
            <tr>
                        <th id="T_a8882645_2c7a_11eb_96d6_ccd9acc89a91level0_row4" class="row_heading level0 row4" >103</th>
                        <th id="T_a8882645_2c7a_11eb_96d6_ccd9acc89a91level1_row4" class="row_heading level1 row4" >Singed Scalpel</th>
                        <td id="T_a8882645_2c7a_11eb_96d6_ccd9acc89a91row4_col0" class="data row4 col0" >8</td>
                        <td id="T_a8882645_2c7a_11eb_96d6_ccd9acc89a91row4_col1" class="data row4 col1" >$4.35</td>
                        <td id="T_a8882645_2c7a_11eb_96d6_ccd9acc89a91row4_col2" class="data row4 col2" >$34.80</td>
            </tr>
    </tbody></table>



## Observations

* Men are far more likely to make in-app purchases, accounting for 84% of purchases
* While 20-24 year old players spend the most, 35-39 year old players are the most valuable 
* The most popular item in the game is also the most profitable item
