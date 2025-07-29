
# Movie Recommendation System – Codebook

This project builds a **simple movie recommendation system** using collaborative filtering and genre-based insights. It walks through data loading, exploration, and visualization, ultimately constructing a model to suggest movies to users based on their ratings and preferences.

## 📁 Contents

### 1. Imports & Setup
Essential libraries are imported:
- `pandas`, `matplotlib`, `seaborn` for data handling and visualization
- `%load_ext autotime` for tracking execution time

### 2. Data Loading
- Movies and ratings data are loaded from the [MovieLens dataset](https://grouplens.org/datasets/movielens/):
  - `movies.csv`: Movie titles and genres
  - `ratings.csv`: User IDs, movie IDs, and ratings

### 3. Genre Distribution Visualization
A horizontal bar plot shows the distribution of movies across different genres.

### 4. Recommendation Algorithm
Based on the approach described by GeeksforGeeks:
- **Collaborative filtering** using user similarity
- Recommendation logic is developed using pandas operations

## 📊 Datasets Used

| Dataset       | Description                         | Source |
|---------------|-------------------------------------|--------|
| `movies.csv`  | Movie titles and genre information | [GfG Tutorial](https://www.geeksforgeeks.org/recommendation-system-in-python/) |
| `ratings.csv` | User-movie rating information      | Same as above |

## 🧠 Recommendation Logic

- Calculates similarity between users based on shared movie ratings.
- Uses this similarity to infer preferences for unseen movies.
- Also includes genre-based frequency analysis.

## ✅ Evaluation & Model Insights

### 📌 Model Testing & Evaluation Steps

The following tests were performed to assess recommendation quality:

#### 1. Per-user Evaluation:
```python
users = ratings['userId'].unique().tolist()
user_use = []
evaluation_scores = []

for user_id in users:
    recommended_movies, top_movie, recommended_id = recommend_movies_for_user(
        user_id, X, user_mapper, movie_mapper, movie_inv_mapper, k=10
    )
    user_df = ratings[ratings['userId'] == user_id]
    seen_movies_ratings = user_df[user_df['movieId'].isin(recommended_id)]['rating'].tolist()

    if seen_movies_ratings:
        above_threshold_count = sum(1 for value in seen_movies_ratings if value > 3)
        total_values_count = len(seen_movies_ratings)

        if total_values_count > 0:
            evaluation_score = above_threshold_count / total_values_count
            user_use.append(user_id)
            evaluation_scores.append(evaluation_score)
```

This gives a proxy measure for the relevance of recommendations, using user feedback on the suggested movies.

#### 2. Clustering Insight:
> "Increasing the number of clusters decreases the overall accuracy. 5 clusters were chosen, balancing variety with precision."

The clustering step ensures personalized yet concise recommendations and aligns with the intended recommendation set size.

#### 3. Manual Recommendation Test:
```python
user_id = 8
recommended_movies, top_movie, recommended_id = recommend_movies_for_user(
    user_id, X, user_mapper, movie_mapper, movie_inv_mapper, k=10
)

if recommended_movies:
    print(f"Recommended movies for user {user_id}:")
    for movie in recommended_movies:
        print(movie)
```
Used to validate output quality for a specific user.

## 📌 Dependencies

```bash
pip install pandas matplotlib seaborn ipython-autotime
```

Use Jupyter Notebook to run the project interactively.
