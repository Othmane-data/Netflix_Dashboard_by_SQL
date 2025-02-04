# Netflix Dashboard Movies and TV Shows Data Analysis by SQL

![](netflix_image.jpg)
--

## Introduction

This project involves a comprehensive analysis of Netflix's movies and TV shows data using SQL. The goal is to extract valuable insights and answer various business questions based on the dataset. The following README provides a detailed account of the project's objectives, business problems, solutions, findings, and conclusions.

## Objectives and Problem statement

- Analyze the distribution of content types (movies vs TV shows).
- Identify the most common ratings for movies and TV shows.
- List and analyze content based on release years, countries, and durations.
- Explore and categorize content based on specific criteria and keywords.

## File

[Netflix_Table](https://github.com/Othmane-data/Netflix_Dashboard_by_SQL/blob/main/netflix_table.csv)

## Schema

```sql
drop table if exists netflix;
CREATE TABLE NETFLIX (
show_id varchar(15),
type varchar(10),
title varchar(150), 	
director varchar(208),
casts varchar(1000),
country varchar(150),
date_added varchar(50),
release_year varchar(20),
rating	varchar(10),
duration varchar(15),
listed_in varchar(100),
description varchar(250)
);

select * from netflix

```


## 15 Business Problems & Solutions

-1. Count the number of Movies vs TV Shows

```sql
select type,
count (show_id) as numbers
from netflix
group by type
```
__Objective:__ Determine the distribution of content types on Netflix.

2. Find the most common rating for movies and TV shows

```sql
select type,
rating from
(select type,
rating,
count (* ),
rank () over (partition by type order by count (*) desc) as ranking
from netflix
group by 1,2
) as T1
where ranking=1
```
__Objective:__ Identify the most frequently occurring rating for each type of content.


3. List all movies released in a specific year (e.g., 2020)

```sql
select *
from netflix
where type = 'Movie'
and
release_year = '2020'
```
__Objective:__ Retrieve all movies released in a specific year.

4. Find the top 5 countries with the most content on Netflix

```sql
select
unnest(string_to_array(country, ',')) as new_country,
count(*) as count_of_content
from netflix
group by new_country
order by count_of_content desc
limit 5;
```
__Objective:__ Identify the top 5 countries with the highest number of content items.


5. Identify the longest movie

```sql
select *
from netflix
where type ='Movie'
and duration is not null
and duration = (select max (duration) from netflix)
```
__Objective:__ Find the movie with the longest duration.

6. Find content added in the last 5 years

```sql
select * from netflix
where to_date(date_added, 'month,DD,YYYY') >=current_date - interval '5 years'
```
__Objective:__ Retrieve content added to Netflix in the last 5 years.


7. Find all the movies/TV shows by director 'Rajiv Chilaka'!

```sql
select *
from netflix
where director ilike '%Rajiv Chilaka%'
```
__Objective:__ List all content directed by 'Rajiv Chilaka'.


8. List all TV shows with more than 5 seasons

```sql
select * 
from netflix
where type = 'TV Show'
and split_part (duration, ' ', 1)::numeric > 5
```
__Objective:__ Identify TV shows with more than 5 seasons.


9. Count the number of content items in each genre

```sql
select
unnest(string_to_array(listed_in, ',')) as new_listed_in,
count(*)
from netflix
group by new_listed_in
order by count desc
```
__Objective:__ Count the number of content items in each genre.


10.Find each year and the average numbers of content release in India on netflix. 
return top 5 year with highest avg content release!

```sql
select 
EXTRACT(year from to_date(date_added, 'month DD,YYY')) as years,
count (*) as yearly_content,
round(
count(*)::numeric/(select count(*)from netflix where country like '%India%')::numeric * 100
,2) as avg_content_per_year
from netflix
where country like '%India%'
group by 1

```
__Objective:__ Calculate and rank years by the average number of content releases by India.


11. List all movies that are documentaries

```sql
select type,
listed_in
from netflix
where type = 'Movie'
and listed_in like '%Documentaries%'
```
__Objective:__ Retrieve all movies classified as documentaries.


12. Find all content without a director

```sql
select
* from netflix
where Director is null
```
__Objective:__ List content that does not have a director.


13. Find how many movies actor 'Salman Khan' appeared in last 10 years!

```sql
select *
from netflix
where casts ilike '%Salman Khan%'
and release_year::numeric > extract(year from current_date)::numeric - 10
```
__Objective:__ Count the number of movies featuring 'Salman Khan' in the last 10 years.


14. Find the top 10 actors who have appeared in the highest number of movies produced in India.

```sql
select
unnest(string_to_array(casts, ',')) as new_casts,
count (*) as count_movie
from netflix
where type ='Movie'
and country ilike '%India%'
group by new_casts
order by count_movie desc
limit 10
```
__Objective:__ Identify the top 10 actors with the most appearances in Indian-produced movies.


15.Categorize the content based on the presence of the keywords 'kill' and 'violence' in 
the description field. Label content containing these keywords as 'Bad' and all other 
content as 'Good'. Count how many items fall into each category.

```sql
with new_table as
(select 
*,
case 
when description ilike '%kill%' then 'Bad'
when description ilike '%violence%' then'Bad'
else 'Good'
end content_category
from netflix)

select content_category,
count (*) as count_category
from new_table
group by 1

```
__Objective:__ Categorize content as 'Bad' if it contains 'kill' or 'violence' and 'Good' otherwise. Count the number of items in each category.

### Conclusion and Recommendations :

__- Content Distribution:__ The dataset contains a diverse range of movies and TV shows with varying ratings and genres.
__- Common Ratings:__ Insights into the most common ratings provide an understanding of the content's target audience.
__- Geographical Insights:__ The top countries and the average content releases by India highlight regional content distribution.
__- Content Categorization:__ Categorizing content based on specific keywords helps in understanding the nature of content available on Netflix.
This analysis provides a comprehensive view of Netflix's content and can help inform content strategy and decision-making.
