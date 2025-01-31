# Spotify SQL Data Analysis
[Dataset Used](https://www.kaggle.com/datasets/sanjanchaudhari/spotify-dataset)

![Spotify Logo](https://github.com/najirh/najirh-Spotify-Data-Analysis-using-SQL/blob/main/spotify_logo.jpg)

## Overview
This project involves analyzing a Spotify dataset with various attributes about tracks, albums, and artists using **SQL**. It covers an end-to-end process of normalizing a denormalized dataset, performing SQL queries of varying complexity (easy, medium, and advanced), and optimizing query performance. The primary goals of the project are to practice advanced SQL skills and generate valuable insights from the dataset.

### Creating Table
```sql
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
```
## Problems Done

### 1. Find the top 5 most-liked tracks for each album type.

```sql
WITH RankedTracks AS (
SELECT
artist,
track,
album,
album_type,
likes,
DENSE_RANK() OVER (PARTITION BY album_type ORDER BY likes DESC) AS rank_position
FROM spotify
SELECT artist, track, album, album_type, likes
FROM RankedTracks
WHERE rank_position <= 5;
```

### 2. Identify no. of artists who have at least two tracks where the valence is higher than 0.75.

```sql

SELECT COUNT(*) FROM (
SELECT artist
FROM spotify
WHERE valence > 0.75
GROUP BY artist
HAVING COUNT(track) >= 2
) as subquery;
```

### 3. Find albums where all tracks have an instrumentalness score greater than 0.8.

```sql

SELECT album FROM spotify
GROUP BY album
HAVING COUNT(*) = COUNT (CASE When instrumentalness > 0.8 Then 1 END);|
```

### 4. Identify albums where the loudest track also has the lowest valence.

```sql

WITH AlbumLoudness AS (
SELECT
album,
track,
loudness,
valence,
RANK() OVER (PARTITION BY album ORDER BY loudness DESC) AS loudest_rank,
RANK() OVER (PARTITION BY album ORDER BY valence ASC) AS lowest valence_rank FROM spotify
SELECT album, track, loudness, valence
FROM Album Loudness
WHERE loudest_rank = 1 AND lowest_valence_rank = 1;
```

### 5. Identify the most-played platform (Spotify or YouTube) for each artist based on total plays.

```sql

WITH platformplay AS
SELECT
artist,
SUM(stream) AS total_spotify_plays, SUM(views) AS total_youtube_views,
CASE
WHEN SUM(stream) > SUM(views) THEN 'Spotify' ELSE 'YouTube'
END AS most_played_platform
FROM spotify
GROUP BY artist)
SELECT artist, most_played_platform
FROM platformplay;
```

### 6. Rank artists based on the total number of tracks they have that exceed the average tempo.

```sql

WITH Avgtempo AS (
SELECT AVG(tempo) AS overall_avg_tempo FROM spotify), Artisttrackcount AS ( SELECT
artist,
COUNT(*) AS high_tempo_tracks
FROM Spotify, Avgtempo
WHERE tempo > overall_avg_tempo
GROUP BY artist)
SELECT
artist,
high tempo_tracks,
DENSE_RANK() OVER (ORDER BY high_tempo_tracks DESC) AS artist_rank
FROM Artisttrackcount;
```

## Contributing

If you would like to contribute to this project, feel free to fork the repository, submit pull requests, or raise issues.

## Author

You can connect with the author on [LinkedIn](https://www.linkedin.com/in/anmol1701/).
