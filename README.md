# Analyze Kickstarter
![](https://i.imgur.com/RTFJQVt.jpg)
https://datastudio.google.com/reporting/1GkxC8-xmfhHOnpA58fR_165q7FF3z0PU/page/IIcw


###    I. Introduction:
What is Kickstater?
Kickstarter is a global online crowdfunding platform that the creators can share and gather interest on their creative projects, along with specs for their finished product, potential risks and progress updates. Other people can donate money towards these projects. 
How does it work?
The creators set a monetary goal and a deadline. The supporters – backers pledge money towards these projects. If the goal is not met by the deadline, the creator receives no money. 
	Main functions of Kickstater
Kickstarter has 15 different project categories including music, food, technology and design. Kickstarter encourages the creators display all the details of the projects such as text, video and photos to everyone. 
Many projects on Kickstarter have incentives for people who make donations above a certain amount for e.g: donate more than $30 – have a copy of the book/ product of the project that you donate into. It’s like a different level of rewards that backers can receive after pledging specific amounts (the larger the amount, the bigger the reward)
If the amount of money satisfies the creator’s goal by the deadline, the project will be implemented and carried out. 
We, as the potential backers, who are extremely interested in Kickstarter’s projects, decide to have a research before investing our money into any of these. Before doing that, we did a data analysis using Kickstarter data, which includes all details about the projects from 2009 to 2017. And we would like to present our data analysis to all of you. It includes our perspectives as backers and how these data present the successful rate of the projects.

###    II. Body:
####    1. Cleaning data:
We download Kickstarter dataset csv file and start our data cleaning by using Jupyter notebook. Import Python’s libraries (numpy, pandas, seaborn and matplotlib) 🡪 use panda to read our dataset csv file. The raw data has 15 columns, which includes all details about the projects: 
ID number
Name – Name of the project 
Category – sub category 
Main category – includes sub category column
Currency – currency used to support 
Deadline – for crowdfunding 
Goal – fundraising goal
Launch – Date launched 
Pledged – the amount that pledged by backers (in different currency) 
State – Current condition of the project (successful, failed, canceled, suspended, live)
Backers – number of backers 
Country- country pledged money from 
USD pledged – amount of money pledged in USD
USD pledged real – real amount of money pledged in USD
USD goal real – real goal amount of the project in USD
Removing null values, drop column and inconsistency dataset:
There are 378661 rows. We want to make sure these data is non-null so we use kicks.info() to check. By looking at the name  and USD pledged column the number of rows are different from the rest. We take a deeper look to name column first to see the null values kicks[kicks['name'].isnull()] As we could not use these data because those are null values, we decide to delete it from the dataset kicks[kicks['name'].notnull()].count(). Similarly, by looking at the USD pledge column, the data is not only duplicate with USD pledge real column but also incorrect. As a result, we also drop this column kicks.drop(columns=['usd pledged']). 
We also find out that there are wrong data in country colum N,0, which is unknown/ unreadable. Therefore, we replace N,0 by changing it into Others.  kicks['country'].replace('N,0"', 'Others', inplace=True)
Moreover, if you can look at the deadline and launched column, the date format is slightly different. Launched column has time while deadline doesn’t. So we decide to create more columns to calculate the duration of the project by separating it into days, months, quarters, year of each project in order to assist our analysis better. There are 7 more columns deadline_y, deadline_m, launched_y, launched_m, deadline_q, launched_q and duration.
kicks['duration'] = round((pd.to_datetime(kicks['deadline_m']).dt.date       pd.to_datetime(kicks['launched_m']).dt.date) / np.timedelta64(1, 'D'))
	
#### Exploratory Data Analysis (EDA)
##### 1st graph
Our goal is focusing on the successful rate of the project. In state column, we only look at successful and fail data, and ignore the rest (suspended, undefined, canceled, live)
kicks['state'].value_counts().plot(kind="pie")
As you can see, the number of failed projects takes most of the pie chart than successful rate. 


##### 2nd graph
We want to know how the duration (number of days between launched and deadline) will affect the successful rate of the projects. 
We called successfulrate = kicks[kicks['state'] == 'successful'] in order to get all the successful projects. Then, we called data_duration = successfulrate['duration'] to extract the number of days that fundraising takes for all the successful projects
fig = plt.figure(figsize=(16,10)) #size of the graph
duration_range = np.arange(0,110,10) #set range for number of days
plt.hist(data_duration, duration_range, label="State Data") #function for histogram 
plt.xlabel('days') #label x values as days
plt.ylabel('Successful rate') #label y values as successful rate 
plt.xticks(np.arange(0,100,10)) #set range for x values 
plt.legend(loc='upper left') #set legend for the dataset on the upper left of the graph
plt.show()
The duration between 30 – 40 days has the highest successful rate based on the number of projects 🡺 We will invest in the projects if the project’s duration is between 30-40 days. 





##### 3rd graph 
We would like to compare the successful rate of each main category, which would help us narrow down the potential fields that has the highest probability of successful rate that being launched in each month.

Theater and dance category have the highest successful rate compared to the rest of them. The data suggest we should invest into Theater  and Dance where the launched months are around from March to July. 
If we are interested in Food category, we must stay alert and avoid investing into any project that launched in July. 

Line Graph version of 3rd graph: 


##### 4th graph
We would like to compare the successful rate of each main category, which would help us to have insights about social trends towards the projects from 2009 to 2017. 
Analyzing the past trends is one of the most important parts. This data helps us to forecast the performance of each category in the future and decide which factors are the most important in defining success for a project.

We can easily observe the trendline of each category. Publishing, Film &Video, Music, Food, Crafts, Fashion, Art, Photography, Technology, Dance and Journalism have significant deceased trendlines, meanwhile Product Design, Tabletop Games, Theater and Comics have steadily increased trendlines in recent years. 
We will choose the top 3 categories having positive trendlines for further analysis and it will be shown in the 5th graph below.
    
    
##### 5th graph
We will zoom into the top 3 categories with highest successful rate compared in different months. We can see the successful rate of Product Design, Tabletop Games and Theater reach its peak, respectively in October, August and June. This data would help us which category and what time of the year that we should invest into. 


##### Charts from Google Data Studio
![](https://i.imgur.com/Bz8MYym.png)
<p align="center">
<i>
  Overview dashboard
</i>
</p>


![](https://i.imgur.com/VfkZ7j8.png)
<p align="center">
<i>
  EDA by countries
</i>
</p>


![](https://i.imgur.com/GgcTceH.png)
<p align="center">
<i>
  EDA by categories
</i>
</p>
