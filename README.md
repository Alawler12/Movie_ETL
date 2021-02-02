
# Building a Data Pipeline
This project aims to illustrate the Extract, Transform, Load process using movie data to create a data pipeline.  The movie data is extracted, cleaned, and loaded into a PostgreSQL database for storage and future analysis.  The data in this case is scraped Wikipedia data stored as a JSON, and Kaggle data in the form of a CSV.  The data is then transformed for storage using Python and Pandas, and then uploaded to an SQL database.  In <b>Movie_data_cleaning.ipyng</b> the ETL process is broken out step by step for both the Wikipedia and Kaggle datasets.  In <b> ETL_clean_kaggle_data.ipynb </b> and <b>ETL_clean_wiki_movies.ipynb</b> this process is refactored into a function. In <b> ETL_create_database</b> all these steps are combined and refactored to the final code for the entire ETL process.

### Extract
The Wikipedia data is a raw JSON file and it is read into the notebook as a list of dictionaries.  The entries are examined to get an idea of what the data looks like. 
The Kaggle data is already flat so it is read in directly to as two dataframes: kaggle_metadata, and ratings.
### Transform
The list of dictionaries is filtered to only include those entries that have a director, an IMDB link, and do not include ‘No. of episodes.’  This will ensure that we end up with a data that only includes movies with an imdb profile.  The list is converted to a dataframe, and then a function is written to loop through each entry and standardize column names and keys.  The list of clean movies is then converted to a dataframe and further cleaned.  Duplicate movies and columns with null values of 90% are deleted, and entries in the box office and budget columns are standardized using regular expressions.  Dates and running times are also standardized.  
The Kaggle metadata is cleaned to drop adult films in the dataset, standardize data types in multiple columns, and convert dates to date time.
The two dataframes are then compared prior to merging them into one.  Redundant columns are dropped, combined, or renamed and the dataframes are merged into movied_df.
The ratings data has too much information for our needs, so it is distilled down to a count of how many times each movie has received a certain rating.  This information is then merged with the cleaned and merged with movies_df to create movies_with_ratings_df.
### Load
A database is created on PGAdmin. A connection is established to this PostgreSQL database using sqlalchemy.  The movie data is imported to the database using the to_sql method.  The ratings data is too large to import in one statement so it is broken into chunks.
 
