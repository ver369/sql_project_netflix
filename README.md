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

**Objective: Determine the distribution of content types on Netflix.


