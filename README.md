# Studio Insights

## Project Overview
This project explores the movie industry to provide insights for a company interested in launching a new movie studio. By analyzing data from multiple sources (IMDB, Box Office Mojo, etc.), we aim to identify the types of films that perform best at the box office and recommend strategies to maximize the success of new film productions.

## Project Goals
- **Examine** historical box office performance across various genres, budgets, runtimes, and ratings.
- **Identify** key trends or patterns that contribute to a movie’s commercial success.
- **Recommend** data-driven strategies to guide the creation and marketing of new films.

## Why This Project Matters
- **Competitive Landscape:** Major streaming platforms and traditional studios are heavily investing in original content.
- **High Stakes:** Movie production requires significant capital investment; data-driven decisions can mitigate financial risks.
- **Opportunity:** By analyzing what works (and what doesn’t), we can tailor film production choices to meet audience demand.

## Business Understanding

### Stakeholder
- **Primary Stakeholder:** The Head of the New Movie Studio at our company.
  - **Role:** Oversees film production, marketing, and distribution strategies.
  - **Interest:** Seeks actionable insights on which film projects to greenlight and how to position them for maximum box office returns.

### Business Questions
1. **Which Genres Are Most Profitable?**
   - Do certain genres consistently yield higher box office revenues or returns on investment (ROI)?
   - Which genres have shown growth or decline in recent years?

2. **What Budget Ranges Correlate with Success?**
   - How do production budgets relate to box office returns?
   - Is there an optimal budget range that maximizes ROI?

3. **How Do Runtime and Ratings Affect Box Office Performance?**
   - Are longer or shorter films more successful on average?
   - Does a higher IMDB rating or Rotten Tomatoes score correlate with higher revenue?

4. **Which Time of Year is Best for Movie Releases?** 
   - Are certain months or seasons historically more profitable?

5. **What Additional Factors Could Drive Success?**
   - Star power (cast and director influence)?
   - Sequels vs. original films?

## Data Understanding

### Data Sources

1. **IMDB (SQLite Database)**
   - **Tables:**
     - `movie_basics`: Contains basic info such as title, year, genres, and runtime.
     - `movie_ratings`: Contains the average rating (`avgRating`) and number of votes (`numVotes`).
     

2. **Box Office Mojo (CSV)**
   - **File:** `bom.movie_gross.csv.gz`
   - **Columns (typical):**
     - `title`: Movie title.
     - `studio`: Studio that produced or distributed the film.
     - `domestic_gross`: Revenue from the domestic market (US/Canada).
     - `foreign_gross`: Revenue from international markets.
     - `year`: Release year.

3. **Rotten Tomatoes, TheMovieDB, The Numbers**
   - May include additional information such as critics’ scores, audience scores, production budgets, etc.
   - Use these if time permits to further enhance the analysis.



### Data Descriptions


  - **`movie_basics`**
    - `movie_id`: Unique identifier (often `tconst` in IMDB’s original data).
    - `primary_title`: The main title of the movie.
    - `original_title`: Original title if different from the main one.
    - `start_year`: Year the movie was first released or produced.
    - `runtime_minutes`: Length of the movie in minutes.
    - `genres`: One or more genres (e.g., "Action,Comedy").
  
  - **`movie_ratings`**
    - `movie_id`: Matches the `movie_id` in `movie_basics`.
    - `avg_rating`: Average IMDB rating (scale 1–10).
    - `num_votes`: Number of votes that contributed to the `avg_rating`.
  
  - **Other Tables (e.g., `principals`, `directors`, `writers`)**
    - Provide additional details about cast and crew (optional for a basic analysis).


  - **`bom.movie_gross.csv.gz`**
    - `title`: Movie title.
    - `studio`: Studio that produced or distributed the film.
    - `domestic_gross`: Revenue from the domestic market (US/Canada).
    - `foreign_gross`: Revenue from international markets.
    - `year`: Release year.



## Data Limitations
- **Data Gaps:** Some movies may not have complete information (missing budgets, missing foreign gross, etc.).
- **Merge Complexity:** The IMDB database uses unique IDs, while Box Office Mojo uses movie titles. Matching them may require careful joining on title and year.
- **Outliers:** Certain blockbuster films (e.g., *Avengers*) or extremely low-budget films might skew averages.
- **Time Range:** Ensure the years in each dataset overlap; otherwise, you might be comparing different time periods.

## Data Preparation Strategy

### Merging
- **Merge Approach:**  
  - Merge IMDB data with Box Office Mojo on movie title and release year (or using an approximate matching technique if necessary).
  - Alternatively, keep the datasets separate if merging is too challenging or results in a limited subset of data.

### Cleaning
- **Handling Missing Values:**  
  - Remove or impute missing values where appropriate.
- **Data Type Conversion:**  
  - Convert currency columns to numeric types for accurate analysis.
- **Genre Processing:**  
  - Decide whether to split genres into multiple rows for granular analysis or keep them as a single string.

### Feature Engineering 
- **Profit/ROI Calculation:**  
  - Creating a `profit` or `ROI` column if budget data is available.
- **Primary Genre Extraction:**  
  - Extracting the primary genre when multiple genres are listed (e.g., "Action" from "Action,Comedy").

  
