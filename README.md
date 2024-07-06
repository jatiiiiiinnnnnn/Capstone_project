This project investigates discrepancies between displayed scores and actual user ratings on Fandango, comparing these to other movie rating platforms.

Key Findings:
Data Exploration and Analysis:
Fandango Ratings vs. Votes:

Scatterplot analysis shows a relationship between movie ratings and vote counts.
Correlation analysis indicates a strong correlation between displayed stars and actual ratings.
Yearly Movie Distribution:

The dataset includes movies from various years, with a notable concentration in recent years.
Top Rated Movies:

Identified top 10 movies with the highest number of votes.
Analyzed the distribution of movies with zero votes.
Reviewed Films Analysis:

KDE plots reveal differences between displayed stars and true user ratings.
The movie with the largest discrepancy between displayed stars and actual ratings was identified.
Comparison with Other Sites:
Rotten Tomatoes Analysis:

Scatterplot comparison of RT critic reviews vs. RT user reviews.
Calculated the mean absolute difference between critic and user ratings.
Identified movies with the largest discrepancies between critic and user ratings.
MetaCritic and IMDB Analysis:

Analyzed scatterplots for Metacritic ratings vs. Metacritic user ratings.
Compared vote counts on Metacritic and IMDB.
Fandango vs. All Sites:
Normalization of Scores:

Scores were normalized to a 0-5 scale for comparison across platforms.
Created a DataFrame of normalized scores for analysis.
Distribution Comparison:

Visualized the distribution of normalized scores across all platforms.
Analyzed KDE plots comparing RT critic ratings vs. Fandango displayed stars.
Worst Movies Analysis:

Identified and visualized ratings for the top 10 lowest-rated movies by Rotten Tomatoes critics.
Cluster map visualization showed ratings across platforms for the worst movies.
Conclusion:
The analysis indicates that Fandango often displays higher ratings than other platforms, possibly to boost ticket sales. This discrepancy is more pronounced for poorly rated films, suggesting a potential bias in displayed scores.
