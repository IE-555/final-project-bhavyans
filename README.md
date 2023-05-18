
<h1> Geospatial Analysis of Buffalo's Tree Species Distribution </h1>

<a name="br1"></a> <b> Authors:</b>  Bhavyanshu Rajendra Kadu[](mailto:bhavyans@buffalo.edu)[ ](mailto:bhavyans@buffalo.edu)[bhavyans@buffalo.edu
](mailto:bhavyans@buffalo.edu), Shubham Shankar Sathe[](mailto:ssathe@buffalo.edu)[ ](mailto:ssathe@buffalo.edu)[ssathe@buffalo.edu
](mailto:ssathe@buffalo.edu), Rahul Haribhau Shelke[](mailto:rahulhar@buffalo.edu)[ ](mailto:rahulhar@buffalo.edu)[rahulhar@buffalo.edu](mailto:rahulhar@buffalo.edu)

  
 #### YouTube Video: xyz
</b>

<h2>Task List</h2>

| Milestones | Activities | Due Date | Status |
| --- | --- | --- | --- |
| Data collection | Collect Tree Inventory dataset from Pendata Buffalo | 2023-05-07 | DONE |
| Data Cleaning | Explore and Preprocess the data | 2023-05-07 | DONE | 
| Progress Report | Reporting all milestones and status of activities with activities at risk | 2023-05-8 | DONE |
| Data Exploration | Explore the data using python libraries | 2023-05-9 | IN PROGRESS |
| | Plot distribution of tree species | 2023-05-10 | NOT STARTED (Delay expected) |
| Data Analysis | Analyze species abundance by location | 2023-05-13 | NOT STARTED |
| | Perform spatial corelation | 2023-05-13 | NOT STARTED |
| Vizualization | Create interactive maps of analysis | 2023-05-15 | NOT STARTED (Delay expected) |
| Presentation | Creating a YouTube video & uploading | 2023-05-16 | NOT STARTED (Delay expected) |
| Report Writing | Writing the project report as README.md and submitting | 2023-05-16 | NOT STARTED (Delay expected) |

<h2>Introduction</h2>

The "Geospatial Analysis of Buffalo's Tree Species Distribution" project intends to explore and evaluate the Pendata Buffalo tree inventory dataset. The study aims to obtain insights into the distribution patterns and features of tree species in Buffalo by using geospatial tools and statistical analysis.

The collection includes useful information about each tree, such as its Botanical Name, Common Name, Diameter at Breast Height (DBH), and different eco-benefits like as stormwater benefits, CO2 avoidance and sequestration, energy benefits, air quality benefits, and property advantages. It also includes address, street, side, site, council district, park name, latitude, longitude, site ID, and location information.

The major goal of this study is to establish the geographical distribution patterns of several tree species around Buffalo. We can reveal critical insights such as the concentration of various tree species inside specific neighborhoods, parks, or council districts by examining the dataset's geospatial properties. This research can help with urban planning, green infrastructure development, and environmental conservation activities.

The geospatial study will include mapping tree species distribution using latitude and longitude data. We can discover clusters, gaps, or places of high species diversity inside the metropolis by viewing the geographical patterns. Furthermore, statistical analytic techniques will be used to establish any significant connections between the distribution of tree species and the various ecological benefits.

The project's findings can be used to promote evidence-based decision-making for urban forestry efforts, targeted planting campaigns, and resource prioritization in Buffalo to optimize the positive environmental impact of trees. Finally, the initiative aims to encourage sustainable urban growth, improve inhabitants' quality of life, and contribute to Buffalo's greener and healthier future.


<h2>Referances</h2>

- The code retrieves data from [Pendata Buffalo(Tree Inventory Dataset)](https://data.buffalony.gov/Quality-of-Life/Tree-Inventory/n4ni-uuec)

<h2>Requirements</h2>

- <b>Python packages/ libraries used:</b> pandas, matplotlib, seaborn, numpy, requests, geopandas
- <b>API source(JSON):</b> https://data.buffalony.gov/resource/n4ni-uuec.json?$limit=99999

<h2>Explanation of the Code</h2>

```{python}
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
import numpy as np
import requests
import geopandas as gpd
# Make an HTTP GET request to the API endpoint
response = requests.get('https://data.buffalony.gov/resource/n4ni-uuec.json?$limit=99999')
# Check if the request was successful (status code 200)
if response.status_code == 200:
    # Get the JSON data from the response
    json_data = response.json()

    # Convert JSON data to a DataFrame
    data = pd.DataFrame(json_data)
    print("Shape before NA removal: ",data.shape)

    # Convert relevant columns to numeric data type
    data['latitude'] = pd.to_numeric(data['latitude'])
    data['longitude'] = pd.to_numeric(data['longitude'])

    df = data

    numeric_columns = ['dbh', 'total_yearly_eco_benefits', 'stormwater_benefits',
                       'stormwater_gallons_saved', 'greenhouse_co2_benefits',
                       'co2_avoided_in_lbs', 'co2_sequestered_in_lbs', 'energy_benefits',
                       'kwh_saved', 'therms_saved', 'air_quality_benefits',
                       'pollutants_saved_in_lbs', 'property_benefits', 'leaf_surface_area_in_sq_ft']
    df[numeric_columns] = df[numeric_columns].apply(pd.to_numeric, errors='coerce')


    df.drop(columns=[':@computed_region_fk4y_hpmh',':@computed_region_eziv_p4ck', ':@computed_region_tmcg_v66k',
        ':@computed_region_kwzn_pe6v', ':@computed_region_xbxg_7ifr',
        ':@computed_region_nmyf_6jtp', ':@computed_region_jdfw_hhbp',
        ':@computed_region_h7a8_iwt4', ':@computed_region_ff6v_jbaa',
        ':@computed_region_vsen_jbmg'],inplace=True)
    

    for col in df.columns:
        df = df[df[col] != "VACANT"]

    print("Shape after NA removal:",df.shape)
else:
    print('Request failed with status code:', response.status_code)
 
```

The code begins by importing the essential libraries, including requests, geopandas, requests, pandas, matplotlib.pyplot, seaborn, and numpy.

The requests library is then used to send an HTTP GET request to an API endpoint. The Buffalo Open Data portal's data is retrieved via the API endpoint URL specified in the code.

The code examines the status code to determine whether the request was successful. The code moves on to the data processing if the status code is 200, which denotes a successful request.

Using pd.DataFrame(json_data), the JSON data collected from the API response is transformed into a pandas DataFrame.

The DataFrame's latitude and longitude columns are first changed using pd.to_numeric() to numeric data types before continuing with the processing.

The variable df is given the DataFrame.

The columns that require conversion to numeric data types are listed in a list of numeric columns.

Using df[numeric_columns], the DataFrame's selected numeric columns are transformed into numeric types.apply (pd.to_numeric, coerce errors).

Using df.drop(columns=[...]), a number of columns that include computed areas are removed from the DataFrame.

The rows with the column value "VACANT" are removed by iterating through each column in the DataFrame in a loop.

The code outputs the DataFrame's form both before and after the rows with missing values (NA) have been removed.

An error message is generated if the HTTP request fails (status code is not 200).

------------
HEATMAP

```{python}
# Plot a correlation heatmap of numeric columns
plt.figure(figsize=(12, 10))
sns.heatmap(data[numeric_columns].corr(), annot=True, cmap='coolwarm')
plt.title('Correlation Heatmap of Numeric Columns')
plt.show()
```

- Using plt.figure(figsize=(12, 10)), the code generates a figure with dimensions of 12 inches in width and 10 inches in height. The figure on which the heatmap will be displayed is now set up.
- Only the numeric columns from the data DataFrame that were previously recognized and saved in the numeric_columns list are selected by data[numeric_columns].
- The correlation matrix produced by.corr() is the result of calculating the pairwise correlation between the chosen numeric columns.
- The correlation matrix is visualized as a heatmap using sns.heatmap(). The correlation values are added to the heatmap cells as annotations when the annot=True parameter is set. The heatmap's color map is changed to a cool-to-warm gradient using the cmap='coolwarm' argument.
- 'Correlation Heatmap of Numeric Columns' is the title of the plot as set by plt.title().
- Heatmap plot is displayed by plt.show().

----------
SCATTER PLOT

```{python}
# Create a scatter plot of tree locations
plt.figure(figsize=(10, 8))
sns.scatterplot(data=df, x=df['longitude'], y=df['latitude'], color="green")
plt.xlabel('Longitude')
plt.ylabel('Latitude')
plt.title('Tree Locations')
plt.show()
```

- Using plt.figure(figsize=(10, 8)), the code generates a figure with dimensions of 10 inches in width and 8 inches in height. The figure to which the scatter plot will be attached is now set up.
- A scatter plot is produced using sns.scatterplot(). The DataFrame (df) from which the data will be plotted is specified by the data parameter. The df['longitude'] column, which represents the longitude values, is the column that will be plotted on the x-axis and is specified by the x parameter. The df['latitude'] column, which represents the latitude values, is the column that will be plotted on the y-axis and is specified by the y parameter.
- 'Longitude' is the label that is set for the x-axis by plt.xlabel().'Latitude' is the label that is set for the y-axis by plt.ylabel().
- 'Tree Locations' is the title of the plot as set by plt.title().
- The scatter plot is displayed with plt.show().

------------
NO OF TREES BY SPECIES

```{python}
# Calculate and plot the number of trees by species
species_counts = df['common_name'].value_counts().reset_index()
species_counts.columns = ['Species', 'Count']
plt.figure(figsize=(12, 8))
sns.barplot(data=species_counts, x=species_counts['Species'][:10], y=species_counts['Count'][:10])
plt.xlabel('Species')
plt.ylabel('Count')
plt.title('Number of Trees by Species')
plt.xticks(rotation=90)
plt.show()
```

- Using plt.figure(figsize=(12, 8)), the code generates a figure with dimensions of 12 inches in width and 8 inches in height.
- A bar plot is produced with sns.barplot(). The DataFrame (species_counts) from which the data will be plotted is specified by the data option. The species_counts['Species'][:10] column, which represents the species names of the top 10 species, is the column that will be plotted on the x-axis and is specified by the x parameter. The species_counts['Count'][:10] column, which represents the number of trees for the top 10 species, is the column that will be plotted on the y-axis and is specified by the y parameter.
- 'Species' is the label that is provided for the x-axis by plt.xlabel().
- 'Count' is the label that is set for the y-axis by plt.ylabel().
- 'Number of Trees by Species' is the plot's title, as set by plt.title().
- The x-axis tick labels are rotated by 90 degrees with plt.xticks(rotation=90) to avoid overlapping when the species names are displayed.
- The bar plot is displayed with plt.show().

----------
TOP 10 COMMON TREE SPECIES

```{python}
# Calculate and plot the top 10 common tree species
top_species = df['common_name'].value_counts().head(10)
plt.figure(figsize=(10, 8))
sns.barplot(x=top_species.values, y=top_species.index)
plt.xlabel('Count')
plt.ylabel('Species')
plt.title('Top 10 Common Tree Species')
plt.show()
```
Using df['common_name'], the code determines the number of trees for each common tree species.value_counts(). This produces a Series with the index being a common tree species and the values being the species' relative counts.
- The top 10 species from the Series with the highest counts are chosen using head (10).
- Using plt.figure(figsize=(10, 8)), the code generates a figure with dimensions of 10 inches in width and 8 inches in height.
- A bar plot is produced with sns.barplot(). The top_species.values parameter, which represents the number of trees for each species, is what will be shown on the x-axis. The top_species.index parameter, which represents the names of the most popular tree species, provides the values that will be presented on the y-axis.
- 'Count' is the label that is set for the x-axis by plt.xlabel().
- 'Species' is the label that is set for the y-axis by plt.ylabel().
- 'Top 10 Common Tree Species' is the title of the plot as set by plt.title().
- The bar plot is displayed with plt.show().

-----------
TOP 10 TREE SPECIES BY CO2 AVOIDANCE
```{python}
# Assuming your DataFrame is named df
# Filter the DataFrame for the top 10 tree species based on CO2 avoided
top_10_species = df.groupby('common_name')['co2_avoided_in_lbs'].sum().nlargest(10).index
filtered_df = df[df['common_name'].isin(top_10_species)]

# Create a bar plot to compare CO2 avoided for the top 10 species
plt.figure(figsize=(12, 6))
sns.barplot(data=filtered_df, x='common_name', y='co2_avoided_in_lbs')
plt.xticks(rotation=45)
plt.xlabel('Tree Species')
plt.ylabel('CO2 Avoided (lbs)')
plt.title('Top 10 Tree Species by CO2 Avoided')
plt.show()

```
- The DataFrame 'df' is grouped by the 'common_name' column, and the sum of 'co2_avoided_in_lbs' is calculated for each group.
- Using the 'nlargest' function, this function returns the top ten tree species with the highest sum of CO2 avoided.
- Filters the 'df' DataFrame to include only the rows with the 'common_name' in the top 10 species.
- Using Seaborn, creates a bar plot to compare the CO2 averted by the top ten species.
- The figure's size is set to 12x6 inches.
- Sets the data for the plot to the filtered DataFrame ('filtered_df') and defines the x-axis as 'common_name' and the y-axis as 'co2_avoided_in_lbs'.
- To improve readability, rotate the x-axis labels by 45 degrees.
- Adds labels to the plot's x-axis, y-axis, and title.
- The bar plot is displayed.

------------
TOP 10 TREE SPECIES BY POLLUTANTS SAVED
```{python}
# Assuming your DataFrame is named df
# Filter the DataFrame for the top 10 tree species based on CO2 avoided
top_10_species = df.groupby('common_name')['pollutants_saved_in_lbs'].sum().nlargest(10).index
filtered_df = df[df['common_name'].isin(top_10_species)]

# Create a bar plot to compare CO2 avoided for the top 10 species
plt.figure(figsize=(12, 6))
sns.barplot(data=filtered_df, x='common_name', y='pollutants_saved_in_lbs')
plt.xticks(rotation=45)
plt.xlabel('Tree Species')
plt.ylabel('Pollutants (lbs)')
plt.title('Polluitants Saved (lbs)')
plt.show()
```
- The DataFrame 'df' is divided into groups based on the 'common_name' column, and the sum of 'pollutants_saved_in_lbs' for each group is calculated.
- Using the 'nlargest' function, it returns the top ten tree species with the most pollutants avoided.
- Filters the 'df' DataFrame to include only the rows with the 'common_name' in the top 10 species.
- Using Seaborn, creates a bar plot to compare the pollutants saved (in pounds) for the top ten species.
- The figure's size is set to 12x6 inches.
- Sets the data for the plot as a filtered DataFrame ('filtered_df') and defines the x-axis as 'common_name' and the y-axis as 'pollutants_saved_in_lbs'.
- To improve readability, rotate the x-axis labels by 45 degrees.
- Adds labels to the plot's x-axis, y-axis, and title.
- The bar plot is displayed.

<h2>How to run the code</h2>
We will need ipython enabled in our machine. Then, we just need to change directories to where our code files are run the following command in command prompt:-

```{python}
ipython -c "%run Final.ipynb"
```


If jupyter is installed, we can also use this command to convert the notebook to a Python file.

```{python}
jupyter nbconvert --to python Final.ipynb
```
And then we run the code using this command:


```{python}
python Final.py
```

<h2>Results from your Analysis</h2>

HEAT MAP
![image](https://github.com/IE-555/final-project-bhavyans/assets/124117361/5e699ff1-9a22-49ed-b3c3-52589205df5f)
The correlation heatmap of the numeric columns in the dataset reveals several important associations. Firstly, there is a strong positive correlation between Stormwater gallons saved and Stormwater benefits, indicating that higher levels of stormwater saved result in increased benefits. Similarly, Energy saved shows a positive correlation with both Kwh saved and Therms saved, suggesting that energy reductions occur across multiple units of measurement. Furthermore, a positive correlation exists between Kwh saved and Therms saved, indicating consistent energy savings. The analysis also reveals a positive relationship between Air quality benefits and Pollutants saved in lbs, implying that improvements in air quality coincide with reduced pollutant levels. Additionally, Property benefits show a positive correlation with Leaf surface area in sq.ft, suggesting that greater leaf surface area contributes to increased property benefits. Lastly, CO2 avoided exhibits positive correlations with both Air quality benefits and Pollutants saved in lbs, indicating that reductions in CO2 emissions are accompanied by improved air quality and reduced pollutant levels. These findings provide valuable insights into the relationships among variables, informing decision-making for environmental benefits and resource management.

-------------
SCATTER PLOT

![image](https://github.com/IE-555/final-project-bhavyans/assets/124117361/0eedf021-7123-475e-b058-62f4aa9885f7)
The output of the code is a scatter plot that displays the locations of trees on a two-dimensional coordinate system. The x-axis represents the longitude values of the tree locations, while the y-axis represents the latitude values. Each data point in the plot corresponds to a tree location, with its position determined by its longitude and latitude coordinates. The scatter plot allows us to visualize the distribution and clustering of trees across the given area. In this case, the tree locations are represented by green dots. 

---------
NUMBER OF TREES BY SPECIES
![image](https://github.com/IE-555/final-project-bhavyans/assets/124117361/1c192792-cdf8-4bce-a3c0-8652e6f37b6b)
The visualization generated by the code is a bar plot that depicts the number of trees categorized by species. The x-axis represents the species names, while the y-axis displays the count or frequency of trees for each species. The bars in the plot indicate the number of trees, with varying heights representing the count. Upon analyzing the data, we found that the top 10 most common tree species, ranked by frequency, are Littleleaf Linden with 7,200 trees, followed by Norway Maple with 7,000 trees. Silver Maple has approximately 3,400 trees, Hedge Maple has 1,800 trees, and Stump has 1,700 trees. Additionally, Crimson King Maple comprises 1,600 trees, Red Maple has 1,550 trees, Ivory Silk Lilac accounts for 1,500 trees, Deborah Schwedler has 1,400 trees, and Crabapple has 1,300 trees. These results offer valuable insights into the distribution and prevalence of different tree species within the dataset, aiding in the understanding of the tree population in the area.

----------
TOP 10 COMMON TREE SPECIES
![image](https://github.com/IE-555/final-project-bhavyans/assets/124117361/5b58284b-6cae-420a-a13c-d338dff663dd)
The code generates a bar plot that showcases the top 10 most common tree species based on their frequency. The x-axis represents the count of each tree species, while the y-axis displays the species names. Each bar corresponds to a specific tree species, and its height indicates the frequency or count of trees belonging to that species. The species are listed on the y-axis in descending order of frequency, with the most common species appearing at the top. By analyzing the bar plot, we can identify the predominant tree species in the dataset and compare their frequencies. The frequency values range from 0 to 7000, with the most common species being "Littleleaf, linden" with a frequency of 7000, followed by "Maple, Norway" with a frequency of 6800, "Maple, Silver" with a frequency of 3500, and so on. This information provides insights into the distribution and abundance of tree species in the dataset, aiding in further analysis and decision-making processes.

----------
GEOSPATIAL MAP
![image](https://github.com/IE-555/final-project-bhavyans/assets/124117361/32872244-f014-4ff5-9a93-e2bdb221c213)
- This map shows the distribution of the top ten most prominent tree species across Buffal and it can help to get informed decisions to plant the necessary tree species

- The Color coding for the trees species are as follows: 
'LINDEN, LITTLELEAF': 'red',
 'MAPLE, NORWAY': 'blue',
 'MAPLE, SILVER': 'green',
 'MAPLE, HEDGE': 'orange',
 'STUMP': 'purple',
 'MAPLE, CRIMSON KING': 'yellow',
 'MAPLE, RED': 'pink',
 'LILAC, IVORY SILK': 'gray',
 'MAPLE, DEBORAH SCHWEDLER': 'brown',
 'CRABAPPLE': 'cyan'
 
 - Any other that is not in TOP 10 is black
 
---------
TOP 10 SPECIES BY CO2 AVOIDANCE
![image](https://github.com/IE-555/final-project-bhavyans/assets/124117361/64676755-d56d-4149-abdf-fde5dd01760a)
From this bar chart we can see that London Plane Tree, Maple Silver, Christine Buisman are the top three species with the highest CO2 avoidance rate.

-------
TOP 10 SPECIES BY POLLUTANTS SAVING rate
![image](https://github.com/IE-555/final-project-bhavyans/assets/124117361/9122699a-42ee-4e73-868d-6522750ca7e2)
From this bar chart we can see that London Plane Tree, Maple Silver, Christine Buisman are the top three species with pollutants aving rate.

-----------
Top 10 TREE SPECIES WITH THE HIGHEST PROPERTY BENEFITS
![image](https://github.com/IE-555/final-project-bhavyans/assets/124117361/f12f3f4a-bade-45ac-a197-f6d2b1bb62cc)
The bar plot generated from the code illustrates the top 10 tree species that contribute the highest property benefits. The x-axis represents the monetary value of property benefits, while the y-axis displays the tree species. Each bar in the plot corresponds to a specific tree species and represents the total proprty benefits generated by trees of that species. By analyzing the plot, we can identify the tree species that make the most significant contributions to property benefits. The top 10 species, ranked in descending order based on property benefits, are listed on the y-axis. The C benefits range from $0 to over $400,000. The species with the highest property benefits is Norway Maple with a value of $450,000, followed by Littleleaf Linden with $200,000, and Silver Maple with $175,000. Other notable species include Peer Callery Hedge Maple, Crimpson King Maple, Crimean Linden, Mapple Hedge, London Planetree, Red Maple, and Honeylocust, with varying property benefit amounts. These findings provide valuable insights for decision-making regarding urban tree management and planning, emphasizing the importance of these specific tree species in generating property benefits. 

-----------
CONCLUSION:

- We can see that the trees scpeies which have the highest CO2 avoidance are not the most commonly found trees in BUffalo.
- On the other hand the trees which give the most economical property benefit are found in aboundance and are the most popular trees in Buffalo.
- We can conclude that this is affecing the air quality and from the above analysis the government can take informed decision to plant the necessary tree species in the required area.



















