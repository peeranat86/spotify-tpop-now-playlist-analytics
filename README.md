# Spotify T-Pop Now Playlist Analytics

This project analyzes the *“T-Pop Now 20251120”* Spotify playlist using the Spotify Web API (via spotipy).  
It pulls track and artist metadata, cleans and enriches it, and then explores:

- Track and artist popularity
- Follower counts
- Genre breakdown and performance
- Release timelines and album age
- Simple correlations between key metrics

The current notebook focuses on a single snapshot of the playlist (around 20 November 2025), so all insights are for that point in time only.


---

## Project Overview

Main goals:

1. *Collect data* from the Spotify Web API for a specific playlist.
2. *Enrich & clean* data:
   - Normalize / simplify Thai-related genres (e.g. thai pop, t-pop, luk thung, thai indie pop).
   - Parse album release dates and derive time-based features (year, month, quarter, album age in days).
3. *Explore & summarize* the dataset:
   - Descriptive statistics for track and artist popularity.
   - Genre distribution and genre-level performance.
   - Time period covered by the playlist.
4. *Visualize* key patterns:
   - Genre distribution (pie chart)
   - Top artists by followers (bar chart)
   - Track popularity distribution (histogram with mean/median)
   - Popularity by genre (boxplot)
   - Artist popularity vs. track popularity (scatter plot)
   - Release timeline by month (bar chart)
   - Correlation matrix for selected metrics

---

## Data Pipeline

### 1. Data Collection

Using spotipy and the *Client Credentials* flow:

- Authenticate with Spotify using:
  - client_id = "YOUR_CLIENT_ID"
  - client_secret = "YOUR_CLIENT_SECRET"
- Target playlist:
  - PLAYLIST_ID = "6JhfoP1VFb1RZFKjrfo2NP"
  - PLAYLIST_NAME = "T-Pop Now 20251120"
- Fetch all tracks from the playlist (handling pagination).
- For each track and artist:
  - Collect track-level information.
  - Collect artist-level metadata in a separate pass (sp.artist()).

### 2. Data Model (Key Columns)

From the notebook, the combined DataFrame includes (at minimum) fields like:

- *Track-level*
  - playlist
  - track_id
  - track_name
  - track_popularity
  - album_name
  - album_release_date
- *Artist-level*
  - artist_id
  - artist_name_playlist
  - followers
  - artist_popularity
  - genres (simplified to one main genre per artist/track)
- *Derived*
  - release_year
  - release_month
  - release_quarter
  - album_age_days

Two main derived DataFrames are used:

- df_tracks – unique tracks (deduplicated on track_id)
- df_artists – artist-track pairs (for follower and popularity analysis)

---

## Analysis & Outputs

### Descriptive Statistics

The notebook prints:

- Track popularity summary (mean, median, standard deviation, range).
- Artist metrics:
  - Average artist popularity
  - Average followers
  - Number of unique artists
- Genre distribution (count and percentage of tracks per simplified genre).
- Time period:
  - Minimum and maximum album_release_date
  - Span in days between the earliest and latest releases.

### Key Insights

Examples of printed insights:

- *Top 5 most popular tracks* in the playlist.
- *Top 5 artists by followers* (with follower counts and popularity).
- *Collaboration stats*:
  - Solo tracks vs. collaborative tracks (2+ artists).
- *Genre performance*:
  - Average track popularity by genre and track counts per genre.

### Visualizations

The notebook generates several plots (using matplotlib + seaborn):

1. *Genre Distribution* – pie chart of simplified genres.
2. *Top 5 Artists by Followers* – horizontal bar chart.
3. *Track Popularity Distribution* – histogram with KDE, mean & median lines.
4. *Track Popularity by Genre* – boxplot.
5. *Artist vs. Track Popularity* – scatter plot with:
   - X: artist_popularity
   - Y: track_popularity
   - Hue: genres
   - Size: followers
6. *Release Timeline* – bar chart of number of tracks by release_month.
7. *Correlation Matrix* – heatmap of correlations between:
   - track_popularity
   - artist_popularity
   - followers
   - album_age_days  
   (Labelled as a *small sample*, so correlations are exploratory, not conclusive.)

---

## Getting Started

### Prerequisites

- Python 3.9+ (any recent version should work)
- A Spotify Developer account and application (for API credentials)

### Installation

Create and activate a virtual environment (recommended), then install the dependencies:

```bash
pip install spotipy pandas streamlit matplotlib seaborn
