# Studio Insights

## Project Overview
We explored the movie industry to provide insights for a company interested in launching a new movie studio. By analyzing data from multiple sources (IMDB, Box Office Mojo, etc.), we identified the types of films that perform best at the box office and recommended strategies to maximize the success of new film productions.

## Project Goals
- **Examined** historical box office performance across various genres, budgets, runtimes, and ratings.  
- **Identified** key trends or patterns that contribute to a movie’s commercial success.  
- **Recommended** data-driven strategies to guide the creation and marketing of new films.

## Why This Project Matters
- **Competitive Landscape:** Major streaming platforms and traditional studios are heavily investing in original content.  
- **High Stakes:** Movie production requires significant capital investment; data-driven decisions can mitigate financial risks.  
- **Opportunity:** By analyzing what works (and what doesn’t), we tailored film production choices to meet audience demand.

## Business Understanding

### Stakeholder
- **Primary Stakeholder:** The Head of the New Movie Studio at our company  
  - **Role:** Oversees film production, marketing, and distribution strategies.  
  - **Interest:** Seeks actionable insights on which film projects to greenlight and how to position them for maximum box office returns.

### Business Questions
1. **Which Genres Are Most Profitable?**  
   - Did certain genres consistently yield higher box office revenues or returns on investment (ROI)?  
   - Which genres showed growth or decline in recent years?

2. **What Budget Ranges Correlate with Success?**  
   - How did production budgets relate to box office returns?  
   - Was there an optimal budget range that maximized ROI?

3. **How Did Runtime and Ratings Affect Box Office Performance?**  
   - Were longer or shorter films more successful on average?  
   - Did a higher IMDB rating or Rotten Tomatoes score correlate with higher revenue?

4. **Which Time of Year Was Best for Movie Releases?**  
   - Were certain months or seasons historically more profitable?

5. **What Additional Factors Could Drive Success?**  
   - Did star power (cast and director influence) play a role?  
   - How did sequels compare to original films?

## Data Understanding

### Data Sources

1. **IMDB (SQLite Database)**
   - **Tables:**  
     - `movie_basics`: Contained basic info such as title, year, genres, and runtime.  
     - `movie_ratings`: Contained the average rating (`avgRating`) and number of votes (`numVotes`).

2. **Box Office Mojo (CSV)**
   - **File:** `bom.movie_gross.csv.gz`  
   - **Columns (typical):**  
     - `title`: Movie title.  
     - `studio`: Studio that produced or distributed the film.  
     - `domestic_gross`: Revenue from the domestic market (US/Canada).  
     - `foreign_gross`: Revenue from international markets.  
     - `year`: Release year.

3. **Rotten Tomatoes, TheMovieDB, The Numbers**  
   - We included additional information such as critics’ scores, audience scores, and production budgets as time permitted to further enhance our analysis.

### Data Descriptions

- **`movie_basics`**  
  - `movie_id`: Unique identifier (often `tconst` in IMDB’s original data).  
  - `primary_title`: The main title of the movie.  
  - `original_title`: Original title if different from the main one.  
  - `start_year`: Year the movie was first released or produced.  
  - `runtime_minutes`: Length of the movie in minutes.  
  - `genres`: One or more genres (e.g., "Action,Comedy").

- **`movie_ratings`**  
  - `movie_id`: Matched the `movie_id` in `movie_basics`.  
  - `avg_rating`: Average IMDB rating (scale 1–10).  
  - `num_votes`: Number of votes that contributed to the `avg_rating`.

- **Other Tables (e.g., `principals`, `directors`, `writers`)**  
  - Provided additional details about cast and crew (optional for a basic analysis).

- **`bom.movie_gross.csv.gz`**  
  - `title`: Movie title.  
  - `studio`: Studio that produced or distributed the film.  
  - `domestic_gross`: Revenue from the domestic market (US/Canada).  
  - `foreign_gross`: Revenue from international markets.  
  - `year`: Release year.

## Data Transformation

### Formatting Worldwide Gross for Readability

To make the revenue figures more interpretable, we converted the `release_date` to a datetime object, extracted the month, grouped the data by month, and transformed the `worldwide_gross` figures from raw numbers into millions. This makes the data easier to understand at a glance (e.g., displaying `$50M` instead of `50000000`).

Below is the code snippet demonstrating this transformation:

```python
# Convert release_date to datetime and extract month
movie_budgets['release_date'] = pd.to_datetime(movie_budgets['release_date'])
movie_budgets['release_month'] = movie_budgets['release_date'].dt.month

# Group by month and calculate mean worldwide gross
monthly_revenue = movie_budgets.groupby('release_month')['worldwide_gross'].mean().reset_index()

# Convert worldwide gross to millions for readability
monthly_revenue['worldwide_gross_millions'] = monthly_revenue['worldwide_gross'] / 1e6

# Display the transformed data
print(monthly_revenue)
