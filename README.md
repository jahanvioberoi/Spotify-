# Spotify Advanced SQL Project and Query Optimization P-6
Project Category: Advanced
[Click Here to get Dataset](https://www.kaggle.com/datasets/sanjanchaudhari/spotify-dataset)

![Spotify Logo](https://github.com/najirh/najirh-Spotify-Data-Analysis-using-SQL/blob/main/spotify_logo.jpg)

## Overview
This project focuses on analyzing a Spotify music dataset using SQL to extract useful business and musical insights. The dataset contains details about tracks such as artist name, genre, popularity, danceability, energy, and more.



```sql
-- create table
DROP TABLE IF EXISTS spotify;
CREATE TABLE spotify (
    artist VARCHAR(255),
    track VARCHAR(255),
    album VARCHAR(255),
    album_type VARCHAR(50),
    danceability FLOAT,
    energy FLOAT,
    loudness FLOAT,
    speechiness FLOAT,
    acousticness FLOAT,
    instrumentalness FLOAT,
    liveness FLOAT,
    valence FLOAT,
    tempo FLOAT,
    duration_min FLOAT,
    title VARCHAR(255),
    channel VARCHAR(255),
    views FLOAT,
    likes BIGINT,
    comments BIGINT,
    licensed BOOLEAN,
    official_video BOOLEAN,
    stream BIGINT,
    energy_liveness FLOAT,
    most_played_on VARCHAR(50)
);




##  Data analysis 

#  1) Retrieve the names of all tracks that have more than 1 billion streams.
   ```sql
   SELECT Track FROM spotify
   WHERE Stream > 1000000000;
#  2)List all albums along with their respective artists.
     ```sql
      Select DISTINCT Album,Artist from spotify
	  ORDER BY 1;
# 3) Get the total number of comments for tracks where licensed = TRUE.
    ```sql
     SELECT SUM(Comments) as Total_comments
	 FROM spotify
	 WHERE Licensed = 'true'
	 ;
# 4) Find all tracks that belong to the album type single.
     ```sql
     Select track from spotify
	 WHERE album_type ILIKE'single'
	 ;
	
# 5) Count the total number of tracks by each artist.
    ```sql
       SELECT
	    artist,
		count(*) as total_no_songs
		FROM spotify
		GROUP BY artist



#  6) Calculate the average danceability of tracks in each album.
   ```sql
    SELECT 
	  album,
	  avg(danceability) as avg_danceability
      FROM spotify
	  GROUP BY 1
	  ORDER BY 2 DESC
     
#    7) Find the top 5 tracks with the highest energy values.
      ```sql
      SELECT * FROM spotify;
	  SELECT 
	  track,
	  MAX(energy)
      FROM spotify
	  GROUP BY 1
	  ORDER BY 2 DESC
	  LIMIT 5
	  
# 8) List all tracks along with their views and likes where official_video = TRUE.
     ```sql
         SELECT 
		 track,
		 sum(views) as total_views ,
	     sum(likes) as total_likes
		 FROM spotify 
	      where official_video = TRUE
		 GROUP BY 1
		 ORDER BY 2 DESC
		 
#   9) For each album, calculate the total views of all associated tracks.
   ```sql
      SELECT 
	   album,
	   track,
	   SUM(views) as total_views
	   FROM spotify
	   GROUP BY 1,2
	   ORDER BY 3 DESC
# 10) Retrieve the track names that have been streamed on Spotify more than YouTube.
    ```sql
     SELECT * FROM (
  SELECT 
    track,
    --most_playedon,
    COALESCE(SUM(CASE WHEN LOWER(most_playedon) = 'youtube' THEN stream END), 0) AS streamed_on_youtube,
    COALESCE(SUM(CASE WHEN LOWER(most_playedon) = 'spotify' THEN stream END), 0) AS streamed_on_spotify
  FROM spotify
  GROUP BY 1
) AS t1
WHERE streamed_on_spotify > streamed_on_youtube
  AND streamed_on_youtube <> 0;

  # 11) Find the top 3 most-viewed tracks for each artist using window functions.
       ```sql
      WITH ranking_artist
	  AS
	  (SELECT 
	  artist,
	  track,
	  SUM(views) as total_views,
	  DENSE_RANK() OVER(PARTITION BY artist ORDER BY SUM(views) DESC) as rank
	  FROM spotify
	  GROUP BY 1,2
	  ORDER BY 1,3 DESC
	  )
	  SELECT * FROM ranking_artist
	  WHERE rank<=3
	  
# 12) Write a query to find tracks where the liveness score is above the average.
     ```sql
       SELECT
	   track,
	   artist
	   
	   FROM spotify
	   WHERE liveness > (SELECT AVG (liveness) FROM spotify)
	   
	  
#  13) Use a WITH clause to calculate the difference between the highest and lowest energy values for tracks in each album.
     ```sql
       WITH cte
	   AS
	   (SELECT
	   album,
	   MAX(energy) as highest_energy,
	   MIN(energy) as lowest_energy
	   FROM spotify
	   GROUP BY 1
	   )
	   SELECT 
	   album,
	   highest_energy-lowest_energy as energy_diff
	   FROM cte
	   ORDER BY 2 DESC


   ## FINDINGS AND CONCLUSIONS
   # 1) Tracks with more than 1 billion streams are the most popular globally.

   # 2) Most songs are released as singles, not part of full albums.

   # 3) Every artist has 3 main songs that get the most views.

  # 4) Songs with official videos get more views and likes.

 # 5) Some albums have much higher total views than others.

# 6) Many songs are streamed more on Spotify than on YouTube.

# 7) Top songs and albums have high danceability and energy.

# 8) Some songs feel more live, based on their liveness score.

# 9) Albums with a big energy range offer different moods.

# 10) Some artists have released a lot more songs than others.












  

	  


```
