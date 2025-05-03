# Netflix_SQL_Project

![netflix_logo](https://github.com/deepak7967/Netflix_SQL_Project/blob/main/n23.jpg)

## Overview
This project involves the analysis of netflix's publicly avilable dataset . The goal is to uncover valuable insights and answer business questions based on the dataset .

## Objectives 

* Analyze the distribution of content types (movies vs TV shows)
* Get content added in last 5 yrs
* To find

### Schema
```sql
create table netflix
(
show_id	varchar(7),
type varchar(10) ,
title varchar(120) ,
director varchar(300) ,
casts varchar(800) ,
country varchar(150),
date_added varchar(100) ,
release_year int,
rating varchar(100) ,
duration varchar(100) ,
listed_in varchar(100) , 
description v
archar(300)
) ;
```

### Business Problems 

#### Task 1 : Count the number of movies vs tv shows
``` sql
select distinct type , count(*) from netflix 
group by type ;
```
#### Task 2 : Find the most common rating for movies and tv shows 
``` sql
select type , rating , count(*),
rank() over(partition by type order by count(*) desc) as ranking
from netflix group by 1 , 2 ;
```
#### Task 3 : List all movies releases in a specific year (e.g , 2021)
```sql
select title , type , release_year from netflix 
where release_year = 2021 and type = 'Movie';
```
#### Task 4 :  Find the top 5 countries with the most content on Netflix 
```sql
select * from netflix ;
select  distinct country , count(*) as country_count
from netflix group by country order by country_count
desc limit 5 ;
```
#### Task 5 :  Identify the longest movie 
```sql
select * from netflix 
where type = 'Movie' and 
duration = (select max(duration) from netflix);
```
#### Task 6 :  Find content added in the last 5 years
```sql
select  * from netflix
where to_date(date_added,'DD-Month-YY')>= current_date - interval '5 years' ;
```
#### Task 7 :  Find all the movies/tv shows by director 'Rajiv Chilaka'
```sql
select * from netflix where director like '%Rajiv Chilaka%';
```
#### Task 8 :  List all Tv Shows with more than 5 seasons
```sql
select * , split_part(duration , ' ' , 1) as season
from netflix where type = 'TV Show' ;
```
#### Task 9 :  Count the number of content items in each genre 
```sql
-- to run this querry we 1st need to split listed_in column into array using string_to_array
select listed_in , show_id , string_to_array(listed_in,',') from netflix
-- now we use unnest function to further split every array items
select listed_in , show_id , 
unnest(string_to_array(listed_in,',')) 
from netflix
-- now we use group by to count number of genre 
select  unnest(string_to_array(listed_in,',')) as genre , 
count(show_id)as total_content from netflix group by 1 ;
```
#### Task 10 :  List all movies that are documentaries
```sql
select * from netflix where listed_in like '%Documentaries%';
```
#### Task 11 : Find how many movies actor 'Salman khan' appeared
```sql
select * from netflix where casts like '%Salman Khan%'
```
#### Task 12 : Find the top 10 actors who appeared in the highest number of movies produced in India .
```sql
select   
unnest(string_to_array(casts,',')) as castss , count(*) as total_content
from netflix where country = 'India' group by 1 order by 2 desc ;
```
