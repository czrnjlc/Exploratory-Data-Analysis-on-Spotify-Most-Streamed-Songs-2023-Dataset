# Exploratory Data Analysis on Spotify Most Streamed Songs 2023 Dataset
This repository presents a detailed exploratory data analysis (EDA) of the Most Streamed Spotify Songs 2023 dataset. Through a combination of data analysis and visualization, we explore trends, correlations, and characteristics that contribute to track popularity on Spotify.
___
### Guidelines
1. Start by examining the dataset’s structure, checking for missing values and data types, and familiarizing yourself with the key features.
2. Summarize key metrics such as streams, release dates, and musical attributes like BPM and danceability, using summary statistics.
3. Create well-labeled visualizations (e.g., bar charts, histograms, scatter plots) to identify trends and patterns.
4. Investigate correlations between variables, especially focusing on how streams relate to other musical characteristics like tempo, energy, and playlist presence.
5. Provide insights or recommendations on tracks, artists, or musical trends to help understand factors driving a track’s popularity.
___
### Data Analysis 
### 1. ) Overview of Dataset
This section examines the basic structure of the dataset, including its shape, data types, and any missing values that may affect the analysis.<br>

 **A. Identifying how many rows and columns the dataset contain**
```python
rows, columns = df.shape
rows, columns
```
> This code snippet retrieves the shape of the DataFrame df, which indicates the number of rows and columns. The shape attribute returns a tuple with the first value as the number of rows and the second as the number of columns. The result shows that the dataset contains 953 rows and 24 columns. 

 **B. Identifying the data types of each column**
```python
df_datatype = df.info()
df_datatype
```
> This code snippet checks the data types of each column in the DataFrame `df`. The `info()` method provides a summary of the DataFrame, including the number of entries, the data types of each column. The resulting output is stored in the variable `df_datatype`. <br>
> **Output:** <br>
>
> ![Screenshot 2024-11-05 232326](https://github.com/user-attachments/assets/a9395f89-d543-401b-a80a-48aa09284672)

 **C. Check for any missing values**
```python
missing_values = df.isnull().sum()
missing_values
```
> This code checks for missing values in the DataFrame `df`. The `isnull().sum()` part counts the number of missing entries in each column, and stores the result in `missing`. This shows how many missing values each column has. The result shows that there are no missing values in all the columns except for columns `key` and `in_shazam_charts` which has 95 and 50 missing respectively.<br>
> **Output:** <br>
>
> ![Screenshot 2024-11-05 233110](https://github.com/user-attachments/assets/613bb8b7-69e0-4345-8014-2d5debdb0ad1)

**Given the detected missing values and errors in the dataset, data cleaning is necessary to ensure accurate analysis.**
```
df['streams'] = pd.to_numeric(df['streams'], errors = 'coerce')
df['in_deezer_playlists'] = pd.to_numeric(df['in_deezer_playlists'], errors = 'coerce')
df['in_shazam_charts']= pd.to_numeric(df['in_shazam_charts'], errors = 'coerce')
df
```
> This code snippet cleans the DataFrame df by converting certain columns to numeric format, resolving non-numeric errors by setting them to NaN (missing values). Specifically: <br>
> * The streams, in_deezer_playlists, and in_shazam_charts columns are each converted to numeric format using pd.to_numeric(). <br>
> * The errors='coerce' argument forces any non-numeric values to become NaN, helping handle data errors. <br>
>
> This ensures that the columns are consistently numeric, making them ready for analysis.

**2. ) Basic Descriptive Statistics** <br>
This section provides an overview of the key statistics for the streams column, including mean, median, and standard deviation, as well as an exploration of the distributions of the release years and artist counts.

**A. Solving for the mean, median, and standard deviation of the streams column**
```
mean_streams = df['streams'].mean()
median_streams = df['streams'].median()
std_streams = df['streams'].std()

print("Mean of streams:", mean_streams)
print("Median of streams:", median_streams)
print("Standard deviation of streams:", std_streams)
```
> Using the functions `mean()`, `median()`, and `std()` the results are achieved. <br>
> Mean of streams: 514,137,424.94 <br>
> Median of streams: 290,530,915 <br>
> Standard deviation of streams: 566,856,949.04

 **B. Identifying the distribution of released_year and artist_count and any noticeable trends or outliers**
```
# Plotting the distribution of released_year
plt.figure(figsize=(10, 5))
sns.histplot(df['released_year'], bins=20, color='skyblue')
plt.title('Distribution of Released Year')
plt.xlabel('Released Year')
plt.ylabel('Frequency')
plt.show()

# Plotting the distribution of artist_count
plt.figure(figsize=(10, 5))
sns.histplot(df['artist_count'], bins=10, color='salmon')
plt.title('Distribution of Artist Count')
plt.xlabel('Number of Artists')
plt.ylabel('Frequency')
plt.show()
```
> ![Screenshot 2024-11-06 000820](https://github.com/user-attachments/assets/0d289292-2432-4601-8fd1-b02dd89fbeb7)
> 
> The dataset shows a strong preference for recent music, with most popular tracks released after 2020 and a few classic tracks dating back to 1930. Solo artists dominate, though collaborations are fairly common, with about 25% of songs featuring multiple artists. Overall, popular streaming tracks are mostly contemporary and often performed by solo artists, with occasional collaborations.

**3. ) Top Performers**
In this section, the standout tracks in the dataset are identified based on streaming performance. The most streamed track is highlighted, along with an exploration of the artists who have the highest representation in the dataset.

 **A. Identifying which track has the highest number of streams and Display the top 5 most streamed tracks**
```
top_spotify_streams = df[['track_name', 'artist(s)_name', 'streams']].sort_values(by='streams', ascending=False).head()
top_spotify_streams 
```
> ![Screenshot 2024-11-06 001914](https://github.com/user-attachments/assets/ebcd4245-633a-4ae2-bf5c-bbadabd8624d)
> 
> The result shows that the track with the highest number of streams is "Blinding Lights" with ~3.70 billion streams, and the top 5 most streamed tracks are the following:
> 1. "Blinding Lights" by The Weeknd <br>
> 2. "Shape of You" by Ed Sheeran <br>
> 3. "Someone You Loved" by Lewis Capaldi <br>
> 4. "Dance Monkey" by Tones and I <br>
> 5. "Sunflower - Spider-Man: Into the Spider-Verse" by Post Malone, Swae Lee <br>

**4. ) Temporal Trends** <br>
This section look into the release patterns of tracks over time, including a visualization of the number of tracks released annually and an analysis of monthly trends that may reveal peak periods for new music.
