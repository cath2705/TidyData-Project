# TidyData-Project
 This repository will contain my Tidy Data Project
 
## Click here to access my Tidy Data Jupyter Notebook! 🧹🧹[Tidy_Data_Project_2008_Olympic_Medals](https://colab.research.google.com/drive/17gWuFKJFb9PEGto3XrJtkQpoLxRQLR0g#scrollTo=_5WaUqfZmB8e))


### 🧹Tidy Data Project-- 2008 Olympic Medal Analysis 🏅
<code><img height="500" src="assets/olympic-fireworks.avif"></code>

### 📌 Project Overview
Welcome to my Tidy Data Project! This repository demonstrates how to clean, reshape, and visualize real-world Olympic medal data using Python — while applying the principles of tidy data, effective data visualization, and reproducible analysis. This project explores and cleans a dataset of Olympic medalists from the 2008 Summer Olympics, focusing on reshaping a wide-format dataset into tidy format to enable meaningful analysis and visualization.

## Goals: 
The goal of this project is to transform a messy dataset on Olympic medals into a clean, tidy format that is easy to analyze, visualize, and interpret. 

## What are Tidy Principles
Tidy data is a standardized way to organize data for easy analysis. A dataset is considered tidy when it follows these three simple rules:
1. Each variable forms a column.
   For Example: "Height", "Weight", and "Age" should each be their own column.
2. Each observation forms a row.
   For Example: A single person’s data (their height, weight, and age) should be one row.
3. Each type of observational unit forms a table.
   For example: If your dataset has different levels of data (like people and their daily measurements), they should be in separate tables.

Therefore, it is important to have Tidy data because it easier to manipulate, visualize, and model data—especially in tools like R and pandas—because it provides consistency and structure.

In this project I will practice Hadley Wickham’s Tidy Data Principles. Meaning that the final (tidy!) dataset will have:
- Each variable is in its own column
- Each observation is in its own row
- Each type of observational unit is in its own table

And then I will create 
-  insightful visualizations that highlight medal distributions across sports, gender, and medal types.

For more information on this topic read: 
- 📄 [Tidy Data by Hadley Wickham](https://vita.had.co.nz/papers/tidy-data.pdf)
- 📄 [Pandas Cheat Sheet (official site)](https://pandas.pydata.org/Pandas_Cheat_Sheet.pdf)

## About This Notebook 
The notebook performs:
- Data inspection
- Data transformation (tidying)
- Validation and cleaning
- Multiple visualizations and summaries
- 📊 Dataset Description

## 🚀 How to Run This Project

✅ Requirements:
The notebook is designed for Google Colab or any local Jupyter environment.
Before running the notebook, make sure you have the following Python packages installed:
- pip install pandas matplotlib seaborn


📂 Steps to Run
1. Upload Dataset: The notebook prompts for file upload — use olympics_08_medalists.csv provided.
2. Run all cells from top to bottom.


The dataset File is called: olympics_08_medalists.csv
Within the dataset the... 
Rows: Individual athletes
Columns: Medalist name and binary indicators for each gendered sport column (e.g., male_archery, female_swimming, etc.)

### 🔧 Pre-processing Steps
After an initial inspectation of this datset you can tell that it is wide as opposed to tidy (because each sport-gender combination is stored as a column instead of as a row).

Problems: Initially the dataset violates the rule of each variable having its own column, each observation having its own row, and each type of observational unit forming a table" because gender and sport are mixed inside column names instead of them being separate variables. In other words the current dataset combines gender and sport in one string. 

Solution: Therefore, I want to reshape the dataset so that gender and sport become their own columns. I need one column for "sport" and one column for "gender".


In order to tidy the dataset, male_athletics and female_athletics should be converted into two columns:
1. "sport" → The sport name
2.  "gender" → Male or female.
- The Panda's Cheat Sheet explains that you can use pd.melt() to convert multiple columns into a long format.

Used pd.melt() to unpivot wide-format sport columns into a long format
from pd.melt I created 3 new columns
1. medalist_name
2. sport_gender
3. medal_status

Inside the sport_gender column, I want to separate out two kinds of variables:
- Gender (either "male" or "female")
- Sport (e.g., "fencing", "basketball", etc.)

So I did this by...
First to Extract Gender
- I used .apply() with a lambda function to check each value in the sport_gender column:
- If the value contains "female_", it assigns "female" to the new gender column.
- If it contains "male_", it assigns "male".
- If it’s neither (or not a string), it assigns None.

Next to Extract Sport Name
- I then double check any possible prefix typos in Sport Names that might have occured while string slicing or merging earlier
- I used .str.replace() with a regular expression to remove the prefixes "female_" or "male_" from the sport names.
- For example: "female_field_hockey" becomes "field_hockey"
- This cleaned name is stored in a new column called sport.

Next to Drop the original combined column 
- I used df_melted.drop(columns=["sport_gender"], inplace=True) to remove the now-unnecessary sport_gender column.
- inplace=True means the DataFrame is modified directly (no reassignment needed).

Next Double Check 
- then I clean up the orginal column using df.melted.drop to remove the now-unnecessary sport_gender column.
- I applied an extra cleaning step:df_cleaned["sport"] = df_cleaned["sport"].str.replace("^fe", "", regex=True).str.replace("^f", "", regex=True)
- This is a safety net to remove incorrect leftover prefixes, like "fesoftball" → "softball" or "fbasketball" → "basketball".

Last Remove rows with no medal
- Next, because I am only interested in actual medal winners (some athletes may have competed but didn’t win a medal), I have to remove any row where the "medal_status" column is NaN (i.e., no medal was awarded).
 - to do this i used df_melted.dropna(subset=["medal_status"])

In Summary I Extracted:
- ✅ Sport name (cleaned and standardized)
- ✅ Gender (from combined column values)
- ✅ Removed missing values and rows with no medal awarded
- ✅ Cleaned up formatting inconsistencies in sport names (e.g., "ncing" → "fencing")

## 📈 Visualizations
Now, that the data is tidy I am able to make clear and easy visuals. 
Here are some of the key visual outputs from the notebook:

### 🥇 Medal Count by Sport
- Bar chart showing the top 10 sports by total number of medals awarded.
<code><img height="500" src="assets/Top 10 Sports with the Most Medals.png"></code>
I created a bar graph to visualize how many medals each sport got. 
I can see that "track and field" had the most medals by a decently sized margin (this make sense since the sport includes many individual events like sprints, hurdles, relays, jumps, throws, etc), "rowing" had the 2nd greatest number of medals, and swimming had the 3rd greatest number of medals. 

### 👥 Medal Count by Gender
- Grouped bar chart comparing male vs. female medal counts per sport.
<code><img height="500" src="assets/Medals by Gender (Top 10 Sports).png"></code>
I created a bar graph to visualize a comparison of how many medals each gender got per sport
Track and field stands out as the sport with the highest number of medals for both male and female athletes—in part reflecting the wide variety of events offered to both genders. Swimming is also well-balanced, with female athletes slightly edging out their male counterparts, suggesting strong participation and equity in event offerings. In contrast, rowing displays a significant gender gap, with male athletes earning substantially more medals—likely due to a greater number of male events or higher male participation during that Olympic cycle

### 🥉 Medal Type Distribution
- Horizontal bar chart showing the number of bronze, silver, and gold medals.
<code><img height="500" src="assets/Medal Type Distribution.png"></code>
I created a bar graph to visualize how many of each kind of medal (gold, silver, bronze) was won. Bronze medals were the most frequently awarded, followed by silver, with gold being the least common. This outcome aligns with the standard Olympic format, where each event awards one gold, one silver, and typically two bronze medals (for example in certain sports such as judo and wrestling, where both semifinal losers receive bronze). 

### 🧁 Pie Chart: Sports by Medal Share
- Visualizes the percentage breakdown of medals by sport (top 10). 
<code><img height="500" src="assets/Top 10 Sports by Medal Count.png"></code>
I created a pie chart as an alternative way to visualize how many medals each sport got. Track and field clearly dominates the chart, accounting for 16.3% of all medals, which is expected given its wide range of events including sprints, jumps, throws, and relays. Rowing follows closely with 13.8%, reflecting its numerous race categories and team events. Swimming also holds a substantial share at 12.3%, thanks to the variety of strokes and distances contested. 

### 📊 Stacked Bar Chart: Medal Types by Sport
- Shows medal types (gold/silver/bronze) stacked for each sport.
<code><img height="500" src="assets/Medal Distribution by Sport.png"></code>
I am creating "Medal Distribution by Sport" stacked bar chart to show how many of each KIND of medal was won per sport. Track and field once again stands out, with the highest overall medal count and a relatively balanced distribution among all three medal types. Association football follows with a significant number of medals, dominated by bronze and gold, which aligns with the team-based format where a fixed number of medals are awarded per tournament. 

## Important Lessons

This Notebook teaches how to
1. How to identify and convert wide-format data into tidy format using pd.melt().
2. How to extract multiple variables from a single column using str.replace() and regex.
3. How to apply data cleaning best practices like .dropna(), .duplicated(), and .value_counts().
4. How to create clear, effective visualizations for data storytelling using matplotlib and seaborn.

From this we can learn
- The real-world example of the importance of data structure when doing analysis.
- How seemingly small issues (e.g., "male_" in "female_") can affect logic and results.

### Look what we accomplished! Goal medal work for sure :) 
<code><img height="500" src="assets/Michael_Phelps.jpg"></code>


