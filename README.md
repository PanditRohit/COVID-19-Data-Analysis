# Import Libraries
import pandas as pd
import numpy as np
import seaborn as sns
import matplotlib.pyplot as plt

#Import Covid-19 Dataset
corona_dataset_csv = pd.read_csv("C:/Users/pandit/Downloads/covid19_Confirmed_dataset.csv")
corona_dataset_csv.head()
corona_dataset_csv.shape

#Delete the useless columns after Exploratory Data Analysis
corona_dataset_csv.drop(["Lat","Long"],axis=1,inplace=True)
corona_dataset_csv.head(10)

#Aggregating the rows by the country
corona_dataset_aggregated=corona_dataset_csv.groupby("Country/Region").sum()
corona_dataset_aggregated.head()
corona_dataset_aggregated.shape

#Visualizing Data related to Countries
corona_dataset_aggregated.loc["China"].plot()
corona_dataset_aggregated.loc["India"].plot()
corona_dataset_aggregated.loc["Italy"].plot()
plt.legend()

#Calculating a good measure
corona_dataset_aggregated.loc['China'][:3].plot()
corona_dataset_aggregated.loc['China'].diff().plot()

#Finding maximum infection rates for Selected countries
corona_dataset_aggregated.loc['China'].diff().max()
corona_dataset_aggregated.loc['India'].diff().max()
corona_dataset_aggregated.loc['Italy'].diff().max()

#Find maximum infection rate for all the countries
countries = list(corona_dataset_aggregated.index)
max_infection_rates=[]
for c in countries :
    max_infection_rates.append(corona_dataset_aggregated.loc[c].diff().max())
corona_dataset_aggregated["max_infection_rates"] = max_infection_rates
corona_dataset_aggregated.head()

#Create a new dataframe with only needed column
corona_data = pd.DataFrame(corona_dataset_aggregated["max_infection_rates"])
corona_data.head()

#Now import world happiness report dataset
happiness_report = pd.read_csv("C:/Users/pandit/Downloads/worldwide_happiness_report.csv")
happiness_report.head()

#Make a list of useless columns and drop the same
useless_cols = ["Overall rank","Score","Generosity","Perceptions of corruption"]
happiness_report.drop(useless_cols,axis=1,inplace=True)
happiness_report.head()

#Change the indices of the dataframe
happiness_report.set_index("Country or region",inplace=True)
happiness_report.head()

#Check the shapes of both the dataframes
corona_data.head()
corona_data.shape
happiness_report.shape

#Join the two datasets using inner join as the shape of the datasets vary
data=corona_data.join(happiness_report,how="inner")
data.head()

#Create a correlation matrix
data.corr()

#Visualization of the results
data.head()

#Plotting GDP vs maximum infection rates
x = data["GDP per capita"]
y = data["max_infection_rates"]
sns.scatterplot(x,np.log(y))
sns.regplot(x,np.log(y))

#Plotting social support vs maximum infection rates
x = data["Social support"]
y = data["max_infection_rates"]
sns.scatterplot(x,np.log(y))
sns.regplot(x,np.log(y))

#Plotting Healthy life expectancy vs maximum infection rates
x = data["Healthy life expectancy"]
y = data["max_infection_rates"]
sns.scatterplot(x,np.log(y))
sns.regplot(x,np.log(y))