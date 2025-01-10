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

'''
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

'''


