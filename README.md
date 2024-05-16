# Football League Data Analysis Project

## Overview
This project involves analyzing detailed football league data to extract insights and drive decisions based on game statistics, team performance, and player contributions. It begins by initializing an SQL database using the premiership.SQL file, setting up the necessary schema and data environment.

## Introduction
The objective of this project is to demonstrate the application of SQL to handle complex datasets, perform detailed queries, and derive insights from sports data. This includes analyzing team strategies, player performances, managerial impacts, and overall league dynamics, with a focus on goal statistics and match outcomes.

## Data Preparation

### Initializing the Database
Run the premiership.SQL file to set up the database schema and populate it with initial data. This file includes commands for creating tables such as teams, players, matches, goals, and managers, and for inserting preliminary data for the league.

### Understanding the Schema
Once the SQL file is executed, the database contains multiple interconnected tables that reflect real-world relationships in football, such as matches between teams, goals scored by players, and managerial tenures.

### Data Integrity and Relationships
Data integrity is ensured through the establishment of primary and foreign keys that maintain relationships between entities like teams, players, and matches. This setup facilitates complex SQL queries that combine data across various dimensions.

## SQL Queries for Analysis
Following the data preparation, several SQL queries are executed to analyze the data from different perspectives. Below are the queries and their descriptions:

### Question 1: Query the names of the teams with the most own goals. Tip: use the WITH TIES keyword.
```sql
SELECT TOP (1) WITH TIES
t.team_id, t.team_name, SUM(po.own_goal) AS Totalowngoals 
FROM team AS t
JOIN player as p
ON t.team_id = p.team_id
JOIN goal AS po
ON p.player_id = po.player_id
GROUP BY t.team_id, t.team_name
ORDER BY Totalowngoals DESC;
```

### Question 2: Query the name of the top scorer (player who scores the most goals). 
```sql
SELECT TOP (1) WITH TIES
p.player_id, p.player_name, COUNT(g.goal_id) AS totalgoal
FROM player AS P JOIN goal AS g
ON p.player_id=g.player_id
GROUP BY p.player_id, p.player_name
ORDER BY totalgoal DESC;
```
### Question 3: Query the number of drawn matches with goals. 
```sql
SELECT COUNT(match_id) AS [sotranhoacobanthang]
FROM match
WHERE home_score=away_score AND home_score= 0
```
### Question 4: Query the name of the team that scores the least away goals.
```sql
SELECT TOP (1) WITH TIES t.team_name, sum(away_score) AS [tongsobansanhkach]
FROM match m 
join team t ON m.away_team_id = t.team_id
GROUP BY t.team_name
ORDER BY [tongsobansanhkach]
```

### Question 5: Query the name of the team that concedes the most goals. 
```sql
WITH away_home_score AS(
SELECT away_team_id, SUM(home_score) AS [tongsobanbithuaosankhach]
FROM match
GROUP BY away_team_id),

home_away_Score AS (
SELECT home_team_id, SUM(away_score) AS [tongsobanbithuaosannha]
FROM match
GROUP BY home_team_id)

SELECT TOP 1 WITH TIES t.team_id, t.team_name,
t1.tongsobanbithuaosannha + t2.tongsobanbithuaosankhach AS [tongsobanbithua]
FROM home_away_Score AS t1 JOIN away_home_score AS t2
ON t1.home_team_id= t2.away_team_id
JOIN team AS t
ON t.team_id=t1.home_team_id 
ORDER BY [tongsobanbithua] DESC;
```

### Question 6: Query the average number of goals scored. 
```sql
SELECT AVG(goal_order)
FROM goal
SELECT TOP 1 WITH TIES
team_name, COUNT(DISTINCT nation_id) AS number_nation
FROM team t join player p ON t.team_id = p.team_id
GROUP BY team_name 
ORDER BY number_nation DESC;
```
### Question 7: Query the name of the country with the most players currently playing in the league.
```sql
SELECT p.nation_id, count(p.player_id) AS [socauthu], n.nation_name
FROM player as P JOIN nation AS n
ON p.nation_id = n.nation_id
GROUP BY n.nation_name, p.nation_id
ORDER BY count(player_id) DESC;
```
### Question 8: Query the name of the football team that has changed coaches the most.
```sql
SELECT TOP 1 t.team_id, t.team_name, COUNT(DISTINCT tm.manager_id) AS Manager_Changes
FROM team AS t JOIN team_manager AS tm
ON t.team_id = tm.team_id
GROUP BY t.team_id, t.team_name
ORDER BY COUNT(DISTINCT tm.manager_id) DESC;
```

### Question 9: Query the name of the coach who has led for the longest and the corresponding duration of leadership. 
--Note: if end_date is NULL, then take end_date as the maximum value present in the team_manager table. 
```sql
SELECT 
    m.manager_name, 
    m.manager_id, 
    SUM(DATEDIFF(day, t.start_date, t.end_date)) AS TotalDays
FROM 
    team_manger2 AS t 
JOIN 
    manager AS m 
ON 
    t.manager_id = m.manager_id
GROUP BY 
    m.manager_name, 
    m.manager_id
ORDER BY 
    TotalDays DESC;
```

### Question 10: Query the names of the matches with the most goals scored and the corresponding number of goals. 
--Note: match names should be in the format of home team v. away team, for example, Chelsea v. Arsenal.
```sql
SELECT TOP 1
    CONCAT(t1.team_name, ' v. ', t2.team_name) AS Match_Name,
    (m.home_score + m.away_score) AS Goals_Scored
FROM
    match m
INNER JOIN
    team t1 ON m.home_team_id = t1.team_id
INNER JOIN
    team t2 ON m.away_team_id = t2.team_id
ORDER BY
    Goals_Scored DESC;
```
## Contact Information

For further information, collaboration, or inquiries, feel free to contact [Phil Dinh](mailto:dinhthanhtrung2011@gmail.com).

