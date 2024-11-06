# Exploratory Data Analysis on Spotify Most Streamed Songs 2023 Dataset
This repository presents a detailed exploratory data analysis (EDA) of the Most Streamed Spotify Songs 2023 dataset. Through a combination of data analysis and visualization, we explore trends, correlations, and characteristics that contribute to track popularity on Spotify.
___
### Guidelines
1. Start by examining the dataset’s structure, checking for missing values and data types, and familiarizing yourself with the key features.
2. Summarize key metrics such as streams, release dates, and musical attributes like BPM and danceability, using summary statistics.
4. Create well-labeled visualizations (e.g., bar charts, histograms, scatter plots) to identify trends and patterns.
5. Investigate correlations between variables, especially focusing on how streams relate to other musical characteristics like tempo, energy, and playlist presence.
6. Provide insights or recommendations on tracks, artists, or musical trends to help understand factors driving a track’s popularity.
___
### Data Analysis 
### 1. ) Overview of Dataset
This section examines the basic structure of the dataset, including its shape, data types, and any missing values that may affect the analysis.<br>

 **A. Identifying how many rows and columns the dataset contain** <br>
 
 ![image](https://github.com/user-attachments/assets/50fe28d5-ae2f-4545-a75e-5df959f03fd3)

```python
rows, columns = df.shape
rows, columns
```
> This code snippet retrieves the shape of the DataFrame df, which indicates the number of rows and columns. The shape attribute returns a tuple with the first value as the number of rows and the second as the number of columns. The result shows that the dataset contains 953 rows and 24 columns. 

 **B. Identifying the data types of each column** <br>
 
 ![Screenshot 2024-11-05 232326](https://github.com/user-attachments/assets/a9395f89-d543-401b-a80a-48aa09284672)
```python
df_datatype = df.info()
df_datatype
```
> This code snippet checks the data types of each column in the DataFrame `df`. The `info()` method provides a summary of the DataFrame, including the number of entries, the data types of each column. The resulting output is stored in the variable `df_datatype`. It revealed that the data types present in the DataFrame are: int64, float64, and object

 **C. Check for any missing values** <br>

 ![Screenshot 2024-11-05 233110](https://github.com/user-attachments/assets/613bb8b7-69e0-4345-8014-2d5debdb0ad1)
```python
missing_values = df.isnull().sum()
missing_values
```
> This code checks for missing values in the DataFrame `df`. The `isnull().sum()` part counts the number of missing entries in each column, and stores the result in `missing`. This shows how many missing values each column has. The result shows that there are no missing values in all the columns except for columns `key` and `in_shazam_charts` which has 95 and 50 missing respectively.<br>

**Given the detected missing values and errors in the dataset, data cleaning is necessary to ensure accurate analysis.**
```python
df['streams'] = pd.to_numeric(df['streams'], errors = 'coerce')
df
```
> This code snippet cleans the DataFrame df by converting certain columns to numeric format, resolving non-numeric errors by setting them to NaN (missing values). Specifically: <br>
> * The streams, in_deezer_playlists, and in_shazam_charts columns are each converted to numeric format using pd.to_numeric(). <br>
> * The errors='coerce' argument forces any non-numeric values to become NaN, helping handle data errors. <br>
>
> This ensures that the columns are consistently numeric, making them ready for analysis.

### 2. ) Basic Descriptive Statistics <br>
This section provides an overview of the key statistics for the streams column, including mean, median, and standard deviation, as well as an exploration of the distributions of the release years and artist counts.

**A. Solving for the mean, median, and standard deviation of the streams column**

![image](https://github.com/user-attachments/assets/8afd685c-f137-442d-b417-543e2b7f5373)

```python
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

![image](https://github.com/user-attachments/assets/a707b496-6fc1-42c8-a667-fee436ee75ab)
```python
# Count tracks released per year and sort by year
year_counts = df['released_year'].value_counts().sort_index()

# Plot the counts as a line chart
year_counts.plot(kind='line', figsize=(10, 4), marker='o')

# Add title and labels
plt.title("Number of Tracks Released per Year")
plt.xlabel("Year")
plt.ylabel("Number of Tracks")

# Display the plot
plt.show()
```
> The dataset shows a strong preference for recent music, with most popular tracks released after 2020 and a few classic tracks dating back to 1930. Solo artists dominate, though collaborations are fairly common, with about 25% of songs featuring multiple artists. Overall, popular streaming tracks are mostly contemporary and often performed by solo artists, with occasional collaborations.

### 3. ) Top Performers <br>
In this section, the standout tracks in the dataset are identified based on streaming performance. The most streamed track is highlighted, along with an exploration of the artists who have the highest representation in the dataset.

 **A. Identifying which track has the highest number of streams and Display the top 5 most streamed tracks**

 ![Screenshot 2024-11-06 001914](https://github.com/user-attachments/assets/ebcd4245-633a-4ae2-bf5c-bbadabd8624d)
```python
top_spotify_streams = df[['track_name', 'artist(s)_name', 'streams']].sort_values(by='streams', ascending=False).head()
top_spotify_streams 
```
> The result shows that the track with the highest number of streams is "Blinding Lights" with ~3.70 billion streams, and the top 5 most streamed tracks are the following:
> 1. "Blinding Lights" by The Weeknd <br>
> 2. "Shape of You" by Ed Sheeran <br>
> 3. "Someone You Loved" by Lewis Capaldi <br>
> 4. "Dance Monkey" by Tones and I <br>
> 5. "Sunflower - Spider-Man: Into the Spider-Verse" by Post Malone, Swae Lee <br>

 **B. Top 5 most frequent artists based on the number of tracks in the dataset**

![image](https://github.com/user-attachments/assets/e3d09914-08e8-4728-9ef5-38824565e767)
 ```python
# Group by 'artist' and count the number of tracks for each artist
top_artists = df['artist(s)_name'].value_counts().head()

# Convert the Series to a DataFrame and reset the index for tabular display
top_artists_df = top_artists.reset_index()
top_artists_df.columns = ['Artist', 'Track Count']

# Display the result 
top_artists_df
```
> The result show that the top 5 most frequent artists based on number of tracks are:
> 1. Taylor Swift
> 2. The Weeknd
> 3. Bad Bunny
> 4. SZA
> 5. Harry Styles

### 4. ) Temporal Trends <br>
This section look into the release patterns of tracks over time, including a visualization of the number of tracks released annually and an analysis of monthly trends that may reveal peak periods for new music.

**A. Trends in the number of tracks released over time**

 ![Screenshot 2024-11-06 201840](https://github.com/user-attachments/assets/6025d5ef-20a7-4d6d-b548-fd3b8fe7c5dc)
```python
# Count tracks released per year and sort by year
year_counts = df['released_year'].value_counts().sort_index()

# Plot the counts as a line chart
year_counts.plot(kind='line', figsize=(10, 4), marker='o')

# Add title and labels
plt.title("Number of Tracks Released per Year")
plt.xlabel("Year")
plt.ylabel("Number of Tracks")

# Display the plot
plt.show()
```
> The dataset reveals a non-linear increase in the number of tracks released over time, with significant fluctuations between years. Notably, there was a sharp rise in releases starting in 2018, peaking in 2022. This surge can be attributed to the democratization of music production tools and online platforms, which have made it easier for artists to produce and distribute music independently. Additionally, streaming services like Spotify and Apple Music have lowered entry barriers for new artists. This trend suggests a shift towards a more artist-driven market, though it may also lead to increased competition for listener attention due to the higher volume of music being released.

**B. Patterns of number of tracks released per month**

![image](https://github.com/user-attachments/assets/1c9afe93-3e22-45ac-906a-92786ce20c74)
```python
# Count tracks released per month and sort by month order
month_counts = df['released_month'].value_counts().sort_index()

# Plot the counts as a bar chart
month_counts.plot(kind='bar', figsize=(10, 4), color='pink')

# Add title and axis labels
plt.title("Number of Tracks Released per Month")
plt.xlabel("Month")
plt.ylabel("Number of Tracks")

# Set custom x-tick labels for months
plt.xticks(ticks=range(0, 12), labels=['Jan', 'Feb', 'Mar', 'Apr', 'May', 'Jun', 'Jul', 'Aug', 'Sep', 'Oct', 'Nov', 'Dec'])

# Display the plot
plt.show()
```
>The dataset shows a clear seasonal pattern, with January and May having the highest number of track releases, while months like July and August see comparatively fewer. This could be due to industry practices, with January marking a fresh start for artists, and May aligning with the summer season when music festivals and events are more prevalent, prompting releases in time for these occasions. This trend suggests that artists and labels may strategically time their releases to maximize exposure and align with listener habits, while the lower release numbers in summer could reflect a focus on promoting existing tracks during festival season rather than introducing new music.

### Genre and Music Characteristics <br>

**A. Correlation between streams and musical attributes like bpm, danceability_%, and energy_%**

![image](https://github.com/user-attachments/assets/17d3be5e-51b8-4325-b3b8-b7c59b00cb9d)
```python
# Perform correlation analysis on selected columns
correlations = df[['streams','bpm', 'danceability_%', 'energy_%']].corr()

# Create a heatmap to visualize the correlation matrix
plt.figure(figsize=(8, 6))
sns.heatmap(correlations, annot=True)

# Add title to the heatmap
plt.title("Correlation Heatmap of Streams and Musical Attributes")

# Display the heatmap
plt.show()
```
>The heatmap shows low correlations between streams and attributes like danceability, BPM, and energy. Danceability has a slight negative correlation with streams (-0.11), while BPM and energy show almost no correlation. This suggests that factors like marketing, artist popularity, and playlist placements are more influential in driving streams than musical attributes. For artists and labels, focusing on promotion and audience engagement may be more effective than solely refining musical characteristics.

**B. Correlation between danceability_% VS energy_% and valence_% VS acousticness_%**

![image](https://github.com/user-attachments/assets/2ac1884f-6de6-4c91-a9e2-ab5cc0c2e6b9)
```python
# Correlation between selected attributes
selected_corr = df[['danceability_%', 'energy_%', 'valence_%', 'acousticness_%']].corr()

# Create a heatmap to visualize the correlation matrix
plt.figure(figsize=(8, 6))
sns.heatmap(selected_corr, annot=True)

# Add title to the heatmap
plt.title("Correlation Heatmap of Danceability, Energy, Valence, and Acousticness")

# Display the heatmap
plt.show()
```
> Correlation between Danceability (%) and Energy (%): The correlation coefficient is 0.198, indicating a weak positive correlation. This suggests that while there may be a slight tendency for tracks that are more danceable to also be more energetic, the relationship is not strong, meaning many other factors could influence these attributes. <br>
> Correlation between Valence (%) and Acousticness (%):
The correlation coefficient is -0.081, which is very close to zero and indicates almost no correlation. This means there is no significant relationship between the positivity of a song (valence) and its acoustic quality (acousticness). Tracks that are more acoustic do not necessarily have a positive or happy mood, and vice versa. <br>
> In summary, there is a weak correlation between danceability and energy, while there is virtually no correlation between valence and acousticness.

### Platform Popularity <br>

**A. Comparison of numbers of tracks in spotify_playlists, spotify_charts, and apple_playlists**

![image](https://github.com/user-attachments/assets/923a98e1-9376-4512-af1e-8d9542de01df)
```python
# Sum up the tracks across different playlist columns for each platform
platform_counts = {
    'Spotify Playlists': df['in_spotify_playlists'].sum(),
    'Spotify Charts': df['in_spotify_charts'].sum(),
    'Apple Playlists': df['in_apple_playlists'].sum()
}

# Plot the popularity of tracks across different platforms
plt.figure(figsize=(8, 4))

# Create a bar plot with different colors for each platform
plt.bar(platform_counts.keys(), platform_counts.values(), color=['lightpink', 'pink', 'red'])

# Add title and axis labels
plt.title("Platform Popularity of Tracks")
plt.xlabel("Platform")
plt.ylabel("Number of Tracks")

# Display the plot
plt.show()
```
> The results show that tracks in the dataset are predominantly featured in **Spotify Playlists**, with a total of 4,955,719 appearances, followed by a much smaller number of appearances in **Apple Playlists** (64,625). Tracks appear far less frequently in **Spotify Charts** (11,445), indicating that being included in charts may be a rarer event compared to being featured in playlists. This suggests that while playlists drive much of the track visibility, chart placement is less frequent but could signify a higher level of popularity or recognition within the platform.

### Advanced Analysis

**A. Patterns among tracks with the same key or mode (Major vs. Minor) based on streams**

![image](https://github.com/user-attachments/assets/6f70bc1c-bdca-4f6f-95be-eb841729062c)
```python
# Grouping by 'key' and 'mode' to calculate the average streams
key_mode_streams = df.groupby(['key', 'mode'])['streams'].mean().unstack()

# Plotting the bar graph with the specified figure size and colors directly in the plot method
ax = key_mode_streams.plot(kind='bar', stacked=False, color=['lightpink', 'lightblue'], figsize=(10, 5))

# Adding title and labels to the plot
plt.title("Average Streams by Key and Mode (Major vs Minor)")
plt.xlabel("Key")
plt.ylabel("Average Streams")

# Customizing the legend
plt.legend(title="Mode (0 = Minor, 1 = Major)", loc='upper right')

# Rotating the x-axis labels for better readability
plt.xticks(rotation=0)

# Display the plot
plt.show()
```
> The graph indicates a clear preference for tracks in Major keys over Minor keys in terms of streaming performance. While there are some exceptions, such as **A# Minor** and **F# Minor**, which outperform their Major counterparts, the overall trend suggests that Major keys generally attract more streams. This aligns with the common association of Major keys with brighter, more upbeat emotions, making them more appealing to listeners. The findings imply that artists and producers might consider the emotional tone of the key when aiming to maximize a track's popularity and streaming success.

**B. Comparison where artists consistently appear in more playlists or charts and Comparison of the most frequently appearing artists in playlists or charts**

![image](https://github.com/user-attachments/assets/127cb2ed-b158-4b5c-8914-e22ae69cc06c)
```python
# Add more platforms to the playlist_counts by summing across the additional columns
playlist_counts = df.groupby('artist(s)_name')[[
    'in_spotify_playlists', 'in_spotify_charts', 'in_apple_playlists', 
    'in_apple_charts', 'in_deezer_playlists', 'in_deezer_charts'
]].sum()

# Filter the top 5 most featured artists
top_artists = playlist_counts.sum(axis=1).sort_values(ascending=False).head(5)

# Plotting a bar chart for the top 5 featured artists with stacked bars
ax = playlist_counts.loc[top_artists.index].plot(kind='bar', stacked=True, figsize=(12, 7), color=['pink', 'pink', 'pink', 'orange', 'orange', 'orange'])

# Adding title and axis labels to the chart
plt.title("Top 5 Artists in Playlists and Charts", fontsize=16)
plt.xlabel("Artist", fontsize=12)
plt.ylabel("Appearances in Playlists/Charts", fontsize=12)

# Adding a legend to differentiate between playlists and charts
plt.legend(title="Source", labels=['Spotify Playlists', 'Apple Playlists', 'Deezer Playlists', 'Spotify Charts', 'Apple Charts',  'Deezer Charts'], loc='upper right')

# Display the chart
plt.tight_layout()
plt.show()
```
>The analysis reveals that artists like **The Weeknd**, **Taylor Swift**, and **Ed Sheeran** dominate playlist appearances, especially on platforms like **Spotify** and **Apple Music**, with a notable presence in **charts** as well. **Pop** artists lead in playlists, while other genres like **hip-hop** (Eminem) and **indie rock** (Arctic Monkeys) show up in both playlists and charts, albeit less frequently. The higher appearance of these artists in **playlists** suggests the influence of curated content on visibility, while **charts** reflect broader, sustained popularity. This indicates that playlists play a significant role in promoting tracks, while chart appearances reflect more sustained mainstream success.
_________
### References
Elgiriyewithana, N. (2023, August 26). Most streamed Spotify Songs 2023. Kaggle. https://www.kaggle.com/datasets/nelgiriyewithana/top-spotify-songs-2023 
_________
### Profile
**Czarina Julia C. Cueco** <br>
2ECEB <br>

I am currently an engineering student from University of Santo Tomas, working with data analysis and visualization using Pandas, Seabon, and Matplotlib in Python. 





