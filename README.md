Data Collection:

Gather comprehensive data on Uber rides, including timestamps, locations, and trip details.
Ensure data quality and integrity.
Exploratory Data Analysis (EDA):

Conduct EDA to identify patterns, trends, and outliers.
Explore factors such as peak hours, popular routes, and ride durations.
Fare Structure Analysis:

Analyze the fare structure to understand pricing dynamics.
Explore variations in pricing during peak times or specific locations.
Geospatial Visualization:

Utilize geospatial tools to create visualizations of ride data.
Highlight popular pickup/drop-off points and visualize traffic patterns.
Customer Behavior and Ratings:

Analyze customer behavior, preferences, and feedback.
Explore factors influencing customer ratings.
Optimization Strategies:

Develop optimization strategies based on data insights.
Explore ways to improve efficiency, reduce wait times, and enhance customer experience.

CODE:

#!/usr/bin/env python
# coding: utf-8

# In[2]:


import pandas as pd
import numpy as np
import seaborn as sns
import matplotlib.pyplot as plt
import os


# In[3]:


os.listdir(r"/Users/vatsalajain/Desktop/Datasets")


# In[4]:


uber= pd.read_csv(r"/Users/vatsalajain/Desktop/Datasets/uber-raw-data-janjune-15_sample.csv")


# In[5]:


uber.shape


# In[6]:


uber.head(3)


# In[16]:


type(uber)


# In[18]:


uber.duplicated().sum()


# In[8]:


uber.drop_duplicates(inplace = True)


# In[9]:


uber.duplicated().sum()


# In[10]:


uber.shape


# In[11]:


uber.dtypes


# In[12]:


uber.isnull().sum()


# In[13]:


uber["Pickup_date"][2]


# In[14]:


type( uber["Pickup_date"][2])


# In[18]:


uber["Pickup_date"]= pd.to_datetime(uber["Pickup_date"])


# In[19]:


uber["Pickup_date"].dtype 


# In[38]:


uber


# In[20]:


uber["month"]=uber["Pickup_date"].dt.month_name()


# In[21]:


uber["month"]


# In[22]:


uber["month"].value_counts().plot(kind="bar")


# In[23]:


uber["Weekday"]=uber["Pickup_date"].dt.day_name()
uber["Day"]=uber["Pickup_date"].dt.day
uber["Hour"]=uber["Pickup_date"].dt.hour
uber["Minute"]=uber["Pickup_date"].dt.minute


# In[54]:


uber.head(5)


# In[24]:


pivot=pd.crosstab(index=uber["month"],columns=uber["Weekday"])


# In[57]:


pivot


# In[64]:


pivot.plot(kind="bar",figsize=(9,10))


# In[25]:


summary=uber.groupby(["Weekday","Hour"], as_index=False).size()


# In[70]:


summary


# In[26]:


sns.pointplot(x="Hour",y="size",hue="Weekday", data=summary)


# In[74]:


uber.columns


# In[76]:


uber_Foil= pd.read_csv(r"/Users/vatsalajain/Desktop/Datasets/Uber-Jan-Feb-FOIL.csv")


# In[79]:


uber_Foil.head(3)


# In[91]:


fig_box=px.box(uber_Foil,x="dispatching_base_number", y="active_vehicles")


# In[93]:


fig_violin=px.violin(uber_Foil,x="dispatching_base_number", y="active_vehicles")


# In[94]:


fig_box


# In[95]:


fig_violin


# In[27]:


os.listdir(r"/Users/vatsalajain/Desktop/Datasets")


# In[28]:


file_list=['other-Lyft_B02510.csv',
 'other-FHV-services_jan-aug-2015.csv',
 'other-Firstclass_B01536.csv',
 'other-Skyline_B00111.csv',
 'uber-raw-data-janjune-15_sample.csv',
 'uber-raw-data-janjune-15.csv',
 'other-American_B01362.csv',
 'uber-raw-data-apr14.csv',
 'Uber-Jan-Feb-FOIL.csv',
 'other-Highclass_B01717.csv',
 'uber-raw-data-aug14.csv',
 'uber-raw-data-sep14.csv',
 'uber-raw-data-jul14.csv',
 'other-Federal_02216.csv',
 'uber-raw-data-jun14.csv',
 'other-Carmel_B00256.csv',
 'other-Diplo_B01196.csv',
 'other-Dial7_B00887.csv',
 'uber-raw-data-may14.csv',
 'other-Prestige_B01338.csv']


# In[29]:


selected_indices=[4,5,7,10,11,12,14,18]


# In[30]:


selected_files = [file_list[i] for i in selected_indices]


# In[31]:


selected_files.remove('uber-raw-data-janjune-15_sample.csv')


# In[32]:


selected_files.remove('uber-raw-data-janjune-15.csv')


# In[33]:


selected_files


# In[34]:


final= pd.DataFrame()

path = r"/Users/vatsalajain/Desktop/Datasets"

for file in selected_files:
    current_df=pd.read_csv(path+'/'+file)
    final=pd.concat([current_df,final])


# In[35]:


final.shape


# In[36]:


final.duplicated().sum()


# In[37]:


final.drop_duplicates(inplace=True)


# In[38]:


final.shape


# In[137]:





# In[39]:


rush_uber=final.groupby(['Lat','Lon'],as_index=False).size()


# In[40]:


rush_uber.head(3)


# In[143]:


get_ipython().system('pip install folium')


# In[144]:


import folium


# In[145]:


basemap=folium.Map()


# In[146]:


basemap


# In[147]:


from folium.plugins import HeatMap


# In[148]:


HeatMap(rush_uber).add_to(basemap)


# In[150]:


basemap


# In[152]:


final.columns


# In[153]:


final.head(3)


# In[154]:


final.dtypes


# In[45]:


final["Date/Time"]= pd.to_datetime(final['Date/Time'], format="%m/%d/%Y %H:%M:%S")


# In[46]:


final['Date/Time'][0]


# In[47]:


final['Date/Time'].dtype


# In[55]:


final["day"]=final["Date/Time"].dt.day
final["hour"]=final["Date/Time"].dt.hour


# In[56]:


final.head(3)


# In[59]:


pivot_table=final.groupby(['day','hour']).size().unstack()


# In[60]:


pivot_table


# In[66]:


pivot_table.style.background_gradient()


# In[67]:


def gen_pivot_table(df,col1,col2):
    pivot= final.groupby([col1,col2]).size().unstack()
    return pivot_table.style.background_gradient()


# In[74]:


gen_pivot_table(final,'day','hour')
