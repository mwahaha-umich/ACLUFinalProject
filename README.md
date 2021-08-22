# ACLUFinalProject

## Background & Purpose
### The American Civil Liberties Union of Michigan was established in 1959 to defend civil liberties for every person in the United States. Although their advocacy work spans multiple issues, one of their primary focuses is defending the right to vote. The organization educates Michiganders about their voting rights, encourages them to get involved in the democratic process, and ensures an equal and easy right to vote for every citizen. For our capstone project, we produced an “engagement dashboard” for the ACLU-MI to help them build out their advocacy strategy around voting rights. This dashboard tracks the habits of Michigan voters, volunteer engagement with campaigns, success of initiatives, and more. We also mapped out the locations of voters contacted by the ACLU-MI to help the organization understand who they are and aren’t reaching. The audience of the dashboard includes donors and ACLU-MI employees.

## Getting Started
### We will start by creating a parent folder called "ACLU". We then create 3 folder paths:
1. "ACLUData" --> 25 files on 3 outreach campaigns
2. "WorkingFiles" --> after running the code, outputs are saved here
3. "AdditionalData" --> external data sources

### ACLUData
#### We have uploaded a sample of each of the 25 files we have recieved from the ACLU of Michigan which are files on 3 of their Outreach Campaigns in 2020: 
1. Oakland County Prosecutor Race (oakland)
2. Supreme Court of Michigan Race (scomi)
3. Statewide voting rights/voter education race (lpv)

You will need to download all of these .csv files and save them to the folder called "ACLUData". 

### AdditionalData
#### Next, we have a couple of various external data sources that helped us with this analysis such as geojson files of Michigan, and all registered voters in the state of Michigan by county. These will need to be saved to a folder called "AdditionalFiles". 

### WorkingFiles
#### This folder path will be empty until you run the code. 

### Insert Path 
#### You can open the workbooks in Collab, we created a parent folder called "ACLU" to house "WorkingFiles" and "AdditionalFiles" folders. You will need to add your path as shown in the screenshot below: 
<img width="1019" alt="Screen Shot 2021-08-21 at 9 08 32 AM" src="https://user-images.githubusercontent.com/73508641/130322824-8d9519b8-1eac-41f9-b2ab-aa5eace92d97.png">

And then you are good to go to get started!

## Requirements
### Please run the requirements files for each of the notebooks.
Example: 

ACLU Python Part 1: 
'pip install -r part_1requirements.txt'

ACLU Python Part 2: 
'pip install -r part_2requirements.txt'

ACLU Dashbaord 1: 

ACLU Spark: 
'pip install -r spark_requirements.txt'

ACLU Spark Dashboard: 

Clustering: 

## Descriptions of Code Files
We wanted to include some of our earlier renditions of our code and how we got started. We began with using pandas to transform our raw files but quickly realized that it was extremely time consuming due to the size of our files. We started with pandas so that we could at least start building visualizations for the dashboard, we were then able to determine the level of aggregation that would be needed to create our visualizations. We started saving these dataframes as smaller files to increase performance. Lastly, we then took everything to Spark which allowed us to handle our datasets without constantly crashing in Collab. Thus, you will find our first renditions of the dashboard via pandas and then our final version using Spark. 

### Python Part 1
#### In this notebook we are loading in all 25 of the files and using the file names to determine the method of outreach. We have text, mail, phone, call, and postcards. Text is it's own category, but mail and postcards were clubbed into one grouping of "mail". Phone and call were also grouped into one grouping of "call". This was to avoid confusion and create simplicity of three methods of outreach as opposed to 5 in which 2 groupings were pretty much the same. 

The main outputs of this file would be: 
1. dfVoters --> all demographic and location data of the voters with the primary key of the Voter File VANID. 
2. dfOutreach --> all outreach data so communication type, for which campaign, with the primary key of the Voter File VANID. 
3. dfMerged --> all election data with the primary key of Voter File VANID

### Python Part 2
#### This notebook is primarily manipulating our output of dfMerged to a file we called dfReshapedUnique, which is a row-based view of all the election data. It creates the view of Voter File VANID, Election Type, Year, and Participation. The primary key is Voter File VANID but as you can tell, there will be multiple rows for each unique voter depending on election and year they have voted in. In terms of the participation column we have A for Absentee ballot, P for Polls (if there is a D or R, that is specifying which party they voted during the primary), Y for Yes indicating that they voted, but we don’t have data on how they voted (in person vs. absentee), and lastly M this one we tried to get more information on but are not entirely sure, the most wegot was that this probably means they voted in a Municipal election. 

Input: 
dfMerged --> all election data with the primary key of Voter File VANID
Output: 
dfReshapedUnique --> dataframe of how each voter we have reached out to in the 3 campaigns has voted throughout time. same as dfMerged just more user friendly

### Python Dashboard
#### Here we are using our four files (dfVoters, dfOutreach, dfMerged, and dfReshapedUnique) to create various visualizations. Before we create our plotly dash, we have created each dataframe and outputted the data into a csv file to be able to delete our larger input files and conserve memory & RAM. We also leveraged ngrok to create our plotly dash in collab. 

### Spark Combine Files 
#### This notebook is a combination of what was done in Python Part 1 & Python Part 2 but in spark to help with memory issues and crashing in Google Collab. In addition to that, any other larger files that were needed to create visualizations on the dashboard were created here. 

### Spark Dashboard
#### This is our final version of the Dashboard leveraging our Spark pipeline. 

### Clustering
#### Rather than forcing the use of advanced machine learning models to add complexity to our project, we stayed focused on what would be most useful to our client. The ACLU-MI was interested in understanding how registered voters in Michigan are similar and different from each other, which we interpreted as a clustering problem. 

Because all of our data was categorical, we decided to use the K-modes clustering method. We first tried encoding the categorical variables so we could run PCA and K-means, but the results didn’t make much sense because the models assumed an order where there was none. K-modes worked fairly well, but it wasn’t able to handle our large dataset. As a result, we randomly selected two million Michigan voters and narrowed our feature set down to include several demographic characteristics such as sex, race, and zip code in combination with voting behavior in the past four presidential elections. Our analysis showed that there are five distinct groups of voters within the population. 
