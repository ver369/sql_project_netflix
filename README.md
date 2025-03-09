# Netflix Movies and TV Shows Data Analysis using SQL

![Netflix Logo](https://github.com/ver369/sql_project_netflix/blob/main/logo.png)

## Overview
This project involves a comprehensive analysis of Netflix's movies and TV shows data using SQL. The goal is to extract valuable insights and answer various business questions based on the dataset. The following README provides a detailed account of the project's objectives, business problems, solutions, findings, and conclusions.

## Objectives
- Analyze the distribution of content types (movies vs TV shows).
- Identify the most common ratings for movies and TV shows.
- List and analyze content based on release years, countries, and durations.
- Explore and categorize content based on specific criteria and keywords.

## Dataset

The data for this project is sourced from the Kaggle dataset:

- **Dataset Link** [Movies Dataset](https://www.kaggle.com/datasets/shivamb/netflix-shows?resource=download)

## Schema

```sql
CREATE TABLE netflix
(
	show_id	VARCHAR(15),
	type VARCHAR(10),	
	title VARCHAR(150),	
	director VARCHAR(208),	
	casts VARCHAR(1000),	
	country	VARCHAR(150),
	date_added VARCHAR(50),
	release_year INT,	
	rating VARCHAR(10),	
	duration VARCHAR(10),
	listed_in VARCHAR(150),	
	description VARCHAR(250)
);
```

## Business Problems and Solutions
### 1. Count the Number of Movies vs TV Shows
```sql
SELECT 
	type,
	COUNT(*) as total_content
FROM netflix
GROUP BY type;
```

**Objective:** Determine the distribution of content types on Netflix.

### 2. Find the Most Common Rating for Movies and TV Shows
```sql
SELECT 
	type,
	rating
FROM
(
	SELECT 
		type,
		rating,
		COUNT(*),
		RANK() OVER(PARTITION BY type ORDER BY COUNT(*) DESC) as ranking
	FROM netflix
	GROUP BY 1,2
	) AS t1
WHERE 
	ranking = 1;
```

**Objective:** Identify the most frequently occurring rating for each type of content.

### 3. List All Movies Released in a Specific Year (e.g., 2020)

```sql
SELECT 
	title
FROM netflix
WHERE 
	release_year = 2020 
	AND 
	type = 'Movie';
```

**Objective:** Retrieve all movies released in a specific year.

### 4. Find the Top 5 Countries with the Most Content on Netflix

```sql
SELECT
	UNNEST(STRING_TO_ARRAY(country, ',')) as new_country,
	COUNT(show_id) as total_content
FROM netflix
GROUP BY 1
ORDER BY 2 DESC
LIMIT 5;
```

**Objective:** Identify the top 5 countries with the highest number of content items.

### 5. Identify the Longest Movies

```sql
SELECT * FROM netflix
WHERE 
	type = 'Movie'
	AND
	duration = (SELECT MAX(duration) FROM netflix);
```
**Objective:** Find movies with the longest duration.

### 6. Find Content Added in the Last 5 Years

```sql
SELECT 
	*
FROM netflix
WHERE
	TO_DATE(date_added, 'Month DD, YYYY') >= CURRENT_DATE - INTERVAL '5 years';
```
**Objective:** Retrieve content added to Netflix in the last 5 years.

### 7. Find All Movies/TV Shows by Director Bruno Garotti

```sql
SELECT * FROM netflix
WHERE director ILIKE '%Bruno Garotti%';
```
**Objective:** List all content directed by Bruno Garotti.

### 8. List All TV Shows with More Than 5 Seasons

```sql
SELECT 
	*
FROM netflix
WHERE 
	type = 'TV Show'
	AND
	SPLIT_PART(duration, ' ', 1)::numeric > 5 ;
```

**Objective:** Identify TV shows with more than 5 seasons.

### 9. Count the Number of Content Items in Each Genre

```sql
SELECT 
	UNNEST(STRING_TO_ARRAY(listed_in, ',')),
	COUNT(show_id) as total_content
FROM netflix
GROUP BY 1;
```

**Objective:** Count the number of content items in each genre.

### 10.Find each year and the average numbers of content release in South Korea on netflix.
return top 5 year with highest avg content release!

```sql
SELECT   
	EXTRACT(YEAR FROM TO_DATE(date_added, 'Month DD, YYYY')) AS date,
	COUNT(*),
	ROUND(COUNT(*)::numeric/(SELECT COUNT(*) FROM netflix WHERE country = 'South Korea') * 100, 2) as avg_content_of_year
FROM netflix
WHERE country = 'South Korea'
GROUP BY 1;
```

**Objective:** Calculate and rank years by the average number of content releases by South Korea.

### 11. List All Movies that are Documentaries

```sql
SELECT * FROM netflix
WHERE
	listed_in ILIKE '%documentaries%';
```
**Objective:** Retrieve all movies classified as documentaries.

### 12. Find All Content Without a Director

```sql
SELECT * FROM netflix
WHERE
	director IS NULL;
```
**Objective:** List content that does not have a director.

### 13. Find How Many Movies Actor Ryan Reynolds Appeared in the Last 10 Years

```sql
SELECT * FROM netflix
WHERE
	casts ILIKE '%Ryan Reynolds%'
	AND 
	release_year > EXTRACT(YEAR FROM CURRENT_DATE) - 10;
```

**Objective:** Count the number of movies featuring Ryan Reynolds in the last 10 years.

### 14. Find the Top 10 Actors Who Have Appeared in the Highest Number of Movies Produced in South Korea

```sql
SELECT 
    UNNEST(STRING_TO_ARRAY(casts, ',')) AS actor,
    COUNT(*) as total_content
FROM netflix
WHERE country = 'South Korea'
GROUP BY 1
ORDER BY 2 DESC
LIMIT 10;
```
**Objective:** Identify the top 10 actors with the most appearances in Korean-produced movies.

### 15. Categorize Content Based on the Presence of 'Kill' and 'Violence' Keywords

```sql
SELECT 
    category,
    COUNT(*) AS content_count
FROM (
    SELECT 
        CASE 
            WHEN description ILIKE '%kill%' OR description ILIKE '%violence%' THEN 'Bad'
            ELSE 'Good'
        END AS category
    FROM netflix
) AS categorized_content
GROUP BY category;
```

**Objective:** Categorize content as 'Bad' if it contains 'kill' or 'violence' and 'Good' otherwise. Count the number of items in each category.



## Findings and Conclusion
- **Content Distribution:** The dataset contains a diverse range of movies and TV shows with varying ratings and genres.
- **Common Ratings:** Insights into the most common ratings provide an understanding of the content's target audience.
- **Geographical Insights:** The top countries and the average content releases by South Korea highlight regional content distribution.
- **Content Categorization:** Categorizing content based on specific keywords helps in understanding the nature of content available on Netflix.

This analysis provides a comprehensive view of Netflix's content and can help inform content strategy and decision-making.
