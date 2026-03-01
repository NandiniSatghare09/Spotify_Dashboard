🎵Project Spotify Songs Using Power BI

📊This project analyzes the most-streamed Spotify songs, aiming to uncover key trends in music consumption, genre popularity, and artist performance. The study also examines factors contributing to streaming success, offering insights into the evolving dynamics of the music industry.

🚀Data Cleaning & Preparation in Power BI
Before visualizing, use Power Query to address common issues found in this specific dataset:
Data Types: Convert streams, in_deezer_playlists, and in_shazam_charts to Whole Number. Date Consolidation: Use the released_year, released_month, and released_day columns to create a single Release Date column using the DAX formula:
Release Date = DATE([released_year], [released_month], [released_day]).
Unpivoting Artists: For songs with multiple artists (artist_count > 1), consider splitting the artist(s)_name column by a comma delimiter to accurately count individual artist appearances. 

1) Key Metrics & KPI Cards
-Total Tracks: COUNTROWS('Spotify Data')
-Total Streams: SUM('Spotify Data'[streams])
-Total Artists: DISTINCTCOUNT('Spotify Data'[artist(s)_name])
-Avg bpm: AVERAGE('Spotify Data'[bpm])

2) Mode Category Distribution (Donut Chart)
The dashboard classifies songs into custom moods (Chill, Happy Hit, Aggressive, Sad). You can create a Calculated Column named Mode_Category using this logic based on valence_% (positivity) and energy_%.

3)  by Decade (Column Chart)
To group songs by decade as seen in the bottom-left visual, create another Calculated Column: 
Decade: FLOOR('Spotify Data'[released_year], 10) & "s"

4) Engagement Score vs. Streams (Scatter Plot)
The dashboard calculates a custom Engagement Score. A common way to derive this from your columns is to sum the song's presence across all platform playlists: 
Engagement Score Measure:
Engagement Score = SUM('Spotify Data'[in_spotify_playlists]) + SUM('Spotify Data'[in_apple_playlists]) + SUM('Spotify Data'[in_deezer_playlists])
Visual Setup: Place streams on the X-axis, Engagement Score on the Y-axis, and use Mode_Category for the legend.

5) Top 10 Songs by Streams (Bar Chart)
To ensure this visual always shows exactly 10 items regardless of other filters, use a Top N filter on the visual level or this DAX measure: 
Top 10 Streams:
CALCULATE(SUM('Spotify Data'[streams]), TOPN(10, ALL('Spotify Data'[track_name]), SUM('Spotify Data'[streams]), DESC))


