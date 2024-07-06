# Capstone_project

Exploring Fandango Displayed Scores versus True User Ratings
This project explores the Fandango movie ratings dataset to analyze discrepancies between displayed scores and actual user ratings. The analysis includes data exploration, visualization, and comparison with other movie rating platforms.

Prerequisites
Ensure you have the following Python packages installed:

numpy
pandas
matplotlib
seaborn
You can install these packages using pip:

sh
Copy code
pip install numpy pandas matplotlib seaborn
Data Files
fandango_scrape.csv
all_sites_scores.csv
Part One: Exploring Fandango Displayed Scores versus True User Ratings
Load the Fandango dataset:

python
Copy code
fandango = pd.read_csv("fandango_scrape.csv")
Explore the DataFrame Properties and Head:

python
Copy code
fandango.head()
fandango.info()
fandango.describe()
Scatterplot showing the relationship between rating and votes:

python
Copy code
plt.figure(figsize=(10,4), dpi=150)
sns.scatterplot(data=fandango, x='RATING', y='VOTES')
plt.show()
Calculate the correlation between the columns:

python
Copy code
fandango.corr()
Create a new column for the year extracted from the film title:

python
Copy code
fandango['YEAR'] = fandango['FILM'].apply(lambda title: title.split('(')[-1])
Count of movies per year:

python
Copy code
fandango['YEAR'].value_counts()
Visualize the count of movies per year:

python
Copy code
sns.countplot(data=fandango, x='YEAR')
plt.show()
Top 10 movies with the highest number of votes:

python
Copy code
fandango.nlargest(10, 'VOTES')
Count of movies with zero votes:

python
Copy code
no_votes = fandango['VOTES'] == 0 
no_votes.sum()
Create DataFrame of only reviewed films:

python
Copy code
fan_reviewed = fandango[fandango['VOTES'] > 0]
KDE plot for distribution of ratings:

python
Copy code
plt.figure(figsize=(10,4), dpi=150)
sns.kdeplot(data=fan_reviewed, x='RATING', clip=[0,5], fill=True, label='True Rating')
sns.kdeplot(data=fan_reviewed, x='STARS', clip=[0,5], fill=True, label='Stars Displayed')
plt.legend(loc=(1.05, 0.5))
plt.show()
Create a new column for the difference between STARS and RATING:

python
Copy code
fan_reviewed["STARS_DIFF"] = fan_reviewed['STARS'] - fan_reviewed['RATING']
fan_reviewed['STARS_DIFF'] = fan_reviewed['STARS_DIFF'].round(2)
Count plot for the differences:

python
Copy code
plt.figure(figsize=(12,4), dpi=150)
sns.countplot(data=fan_reviewed, x='STARS_DIFF', palette='magma')
plt.show()
Identify the movie with the largest discrepancy:

python
Copy code
fan_reviewed[fan_reviewed['STARS_DIFF'] == 1]
Part Three: Comparison of Fandango Ratings to Other Sites
Load the all_sites_scores.csv dataset:

python
Copy code
all_sites = pd.read_csv("all_sites_scores.csv")
Explore the DataFrame columns, info, and description:

python
Copy code
all_sites.head()
all_sites.info()
all_sites.describe()
Rotten Tomatoes Analysis
Scatterplot for RT Critic reviews vs. RT User reviews:

python
Copy code
plt.figure(figsize=(10,4), dpi=150)
sns.scatterplot(data=all_sites, x='RottenTomatoes', y='RottenTomatoes_User')
plt.xlim(0, 100)
plt.ylim(0, 100)
plt.show()
Calculate the difference between critics and user ratings:

python
Copy code
all_sites['Rotten_Diff'] = all_sites['RottenTomatoes'] - all_sites['RottenTomatoes_User']
Mean Absolute Difference:

python
Copy code
all_sites['Rotten_Diff'].apply(abs).mean()
Distribution of the differences:

python
Copy code
plt.figure(figsize=(10,4), dpi=200)
sns.histplot(data=all_sites, x='Rotten_Diff', kde=True, bins=25)
plt.title("RT Critics Score minus RT User Score")
plt.show()
Distribution of the absolute value differences:

python
Copy code
plt.figure(figsize=(10,4), dpi=200)
sns.histplot(x=all_sites['Rotten_Diff'].apply(abs), bins=25, kde=True)
plt.title("Abs Difference between RT Critics Score and RT User Score")
plt.show()
Top 5 movies users rated higher than critics:

python
Copy code
print("Users Love but Critics Hate")
all_sites.nsmallest(5, 'Rotten_Diff')[['FILM', 'Rotten_Diff']]
Top 5 movies critics rated higher than users:

python
Copy code
print("Critics love, but Users Hate")
all_sites.nlargest(5, 'Rotten_Diff')[['FILM', 'Rotten_Diff']]
MetaCritic Analysis
Scatterplot for Metacritic Rating vs. Metacritic User rating:

python
Copy code
plt.figure(figsize=(10,4), dpi=150)
sns.scatterplot(data=all_sites, x='Metacritic', y='Metacritic_User')
plt.xlim(0, 100)
plt.ylim(0, 10)
plt.show()
IMDB Analysis
Scatterplot for vote counts on MetaCritic vs. vote counts on IMDB:

python
Copy code
plt.figure(figsize=(10,4), dpi=150)
sns.scatterplot(data=all_sites, x='Metacritic_user_vote_count', y='IMDB_user_vote_count')
plt.show()
Movie with the highest IMDB user vote count:

python
Copy code
all_sites.nlargest(1, 'IMDB_user_vote_count')
Movie with the highest Metacritic User Vote count:

python
Copy code
all_sites.nlargest(1, 'Metacritic_user_vote_count')
Fandango Scores vs. All Sites
Combine the Fandango table with the All Sites table using an inner merge:

python
Copy code
df = pd.merge(fandango, all_sites, on='FILM', how='inner')
df.info()
df.head()
Normalize columns to Fandango STARS and RATINGS (0-5 scale):

python
Copy code
df['RT_Norm'] = np.round(df['RottenTomatoes'] / 20, 1)
df['RTU_Norm'] = np.round(df['RottenTomatoes_User'] / 20, 1)
df['Meta_Norm'] = np.round(df['Metacritic'] / 20, 1)
df['Meta_U_Norm'] = np.round(df['Metacritic_User'] / 2, 1)
df['IMDB_Norm'] = np.round(df['IMDB'] / 2, 1)
Create a norm_scores DataFrame:

python
Copy code
norm_scores = df[['STARS', 'RATING', 'RT_Norm', 'RTU_Norm', 'Meta_Norm', 'Meta_U_Norm', 'IMDB_Norm']]
norm_scores.head()
Comparing distribution of scores across sites:

python
Copy code
fig, ax = plt.subplots(figsize=(15,6), dpi=150)
sns.kdeplot(data=norm_scores, clip=[0,5], shade=True, palette='Set1', ax=ax)
move_legend(ax, "upper left")
plt.show()
KDE plot for RT critic ratings vs. Fandango displayed STARS:

python
Copy code
fig, ax = plt.subplots(figsize=(15,6), dpi=150)
sns.kdeplot(data=norm_scores[['RT_Norm', 'STARS']], clip=[0,5], shade=True, palette='Set1', ax=ax)
move_legend(ax, "upper left")
plt.show()
Optional histplot comparing all normalized scores:

python
Copy code
plt.subplots(figsize=(15,6), dpi=150)
sns.histplot(norm_scores, bins=50)
plt.show()
How are the worst movies rated across all platforms?
Clustermap visualization of all normalized scores:

python
Copy code
sns.clustermap(norm_scores, cmap='magma', col_cluster=False)
plt.show()
Top 10 lowest rated movies by Rotten Tomatoes Critic Ratings:

python
Copy code
norm_films = df[['STARS', 'RATING', 'RT_Norm', 'RTU_Norm', 'Meta_Norm', 'Meta_U_Norm', 'IMDB_Norm', 'FILM']]
norm_films.nsmallest(10, 'RT_Norm')
Visualize the distribution of ratings across all sites for the top 10 worst movies:

python
Copy code
plt.figure(figsize=(15,6), dpi=150)
worst_films = norm_films.nsmallest(10, 'RT_Norm').drop('FILM', axis=1)
sns.kdeplot(data=worst_films, clip=[0,5], shade=True, palette='Set1')
plt.title("Ratings for RT Critic's 10 Worst Reviewed Films")
plt.show()
Final Thoughts
The analysis indicates that Fandango often displays higher ratings than other platforms, potentially to boost ticket sales. This discrepancy is especially pronounced for poorly rated films.
