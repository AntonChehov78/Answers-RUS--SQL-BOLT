# -SQL-BOLT
-- CH1 - Выбрать запросы 101

-- НАйти название всех фильмов
SELECT Title
FROM Movies;

-- НАйти режиссера для каждого фильма
SELECT Director
FROM Movies;

-- Найти все фильмы и всех режиссеров
SELECT Title, Director
FROM Movies;

-- НАйти все фильты и год выпуска
SELECT Title, Director
FROM Movies;

-- Показать всю информацию о всех фильмах
SELECT *
FROM Movies;

-- CH2 - Запросы с ограничениями

-- Найти фильм с id 6
SELECT *
FROM Movies
WHERE Id = 6;

-- Найти фильмы выпущенные в период с 2000 по 2010
SELECT *
FROM Movies
WHERE Year BETWEEN 2000 AND 2010;

-- Найти фильмы выпущенные в период отличный от с 2000 по 2010
SELECT *
FROM Movies
WHERE Year NOT BETWEEN 2000 AND 2010;

-- НАйти первые 5 фильмов по годам выпуска
SELECT *
FROM Movies
WHERE Id BETWEEN 1 AND 5;

-- CH3 - Запросы с ограничениями (Ч. 2)

-- Вывести все фильмы "История игрушек"
SELECT *
FROM Movies
WHERE Title LIKE "%Toy Story%";

-- Найдите все фильмы режиссера Джона Лассетера
SELECT *
FROM Movies
WHERE Director = "John Lasseter";

-- Найдите все фильмы (и режиссера), не снятые Джоном Лассетером
SELECT *
FROM Movies
WHERE Director != "John Lasseter";

-- Найдите все фильмы содержащие WALL
SELECT *
FROM Movies
WHERE Title LIKE "%WALL%";

-- CH4 - Фильтрация и сортировка результатов запроса

-- Перечислите всех режиссеров фильмов Pixar (в алфавитном порядке), без дубликатов
FROM Movies
ORDER BY Director;

-- Вывести список последних четырех выпущенных фильмов Pixar (в порядке от самых последних к наименее)
SELECT *
FROM Movies
ORDER BY Year DESC
LIMIT 4;

-- Перечислите первые пять фильмов Pixar, отсортированных в алфавитном порядке
SELECT *
FROM Movies
ORDER BY Title ASC
LIMIT 5;

-- Перечислите следующие пять фильмов Pixar, отсортированных в алфавитном порядке
SELECT *
FROM Movies
ORDER BY Title ASC
LIMIT 5
OFFSET 5;

-- CH5 - Просмотрите простые запросы ВЫБОРА

-- Перечислите все канадские города и их население
SELECT *
FROM North_american_cities
WHERE Country LIKE "Canada";

-- Упорядочите все города в Соединенных Штатах по их широте с севера на юг
SELECT *
FROM North_american_cities
WHERE Country = "United States"
ORDER BY Latitude DESC;

-- Перечислите все города к западу от Чикаго, упорядоченные с запада на восток
SELECT *
FROM North_american_cities
WHERE Longitude < -87.69
ORDER BY Longitude ASC;

-- Вывести два самых населенных города в Мексике
SELECT *
FROM North_american_cities
WHERE Country LIKE "Mexico"
ORDER BY Population DESC
LIMIT 2;

-- Перечислите третий и четвертый по величине города (по численности населения) в Соединенных Штатах и их население
SELECT *
FROM North_american_cities
WHERE Country LIKE "United States"
ORDER BY Population DESC
LIMIT 2
OFFSET 2;

-- CH6 - Запросы с объединением таблиц
-- Найдите внутренние и международные продажи для каждого фильма
SELECT Title, International_sales, Domestic_sales
FROM Movies JOIN Boxoffice
ON Id=Movie_id;

-- Покажите цифры продаж для каждого фильма, который показал лучшие результаты на международном уровне, а не внутри страны
SELECT Title, International_sales, Domestic_sales
FROM Movies JOIN Boxoffice
ON Id=Movie_id
WHERE International_sales > Domestic_sales;

-- Перечислите все фильмы по их рейтингам в порядке убывания
SELECT Title, Rating
FROM Movies JOIN Boxoffice
ON Id=Movie_id
ORDER BY Rating DESC;

-- CH7 - Внешние соединения таблиц

-- Найдите список всех зданий, в которых есть сотрудники
SELECT DISTINCT Building
FROM Employees
LEFT JOIN Buildings ON Building=Building_name
WHERE Years_employed NOT NULL;

-- Найдите список всех зданий и их вместимость
SELECT *
FROM Buildings;

-- List all buildings and the distinct employee roles in each building (including empty buildings)
SELECT DISTINCT Building_name, Role 
FROM Buildings 
LEFT JOIN employees ON building_name = building;

-- CH8 - A short note on NULLs

-- Find the name and role of all employees who have not been assigned to a building
SELECT *
FROM Employees
LEFT JOIN Buildings
ON Building_name = Building
WHERE Building IS NULL;

-- Find the names of the buildings that hold no employees
SELECT *
FROM Buildings
LEFT JOIN Employees
ON Building_name = Building
WHERE Building IS NULL;

-- CH9 - Queries with expressions

-- List all movies and their combined sales in millions of dollars
SELECT Title, (Domestic_sales + International_sales)/1000000 AS Total_Sales_Millions
FROM Movies
LEFT JOIN Boxoffice ON Id=Movie_Id;

-- List all movies and their ratings in percent
SELECT Title, Rating*10 as Percent
FROM Movies
LEFT JOIN Boxoffice ON Id=Movie_Id;

-- List all movies that were released on even number years
SELECT Title, Year
FROM Movies
LEFT JOIN Boxoffice ON Id=Movie_Id
WHERE Year % 2 = 0;

-- CH10 - Queries with aggregates (Pt. 1)

-- Find the longest time that an employee has been at the studio
SELECT MAX(Years_employed)
FROM Employees;

-- For each role, find the average number of years employed by employees in that role
SELECT Role, AVG(Years_Employed) 
FROM Employees
GROUP BY Role;

-- Find the total number of employee years worked in each building
SELECT Building, SUM(Years_Employed) 
FROM Employees
GROUP BY Building;

-- CH11 - Queries with aggregates (Pt. 2)

-- Find the number of Artists in the studio (without a HAVING clause)
SELECT Role, COUNT(*) AS Number_of_Artists
FROM Employees
WHERE Role = "Artist";

-- Find the number of Employees of each role in the studio
SELECT Role, COUNT(*)
FROM Employees
GROUP BY Role;

-- Find the total number of years employed by all Engineers
SELECT Role, SUM(Years_Employed)
FROM Employees
GROUP BY Role
HAVING Role = "Engineer";

-- CH12 - Order of execution of a Query

-- Find the number of movies each director has directed
SELECT *, COUNT(Title)
FROM Movies
GROUP BY Director;

-- Find the total domestic and international sales that can be attributed to each director
SELECT Director, sum(Domestic_sales + International_Sales) as Total_Sales
FROM Movies
LEFT JOIN Boxoffice ON Id = Movie_ID
GROUP BY Director;

-- CH13 - Inserting rows

-- Add the studio's new production, Toy Story 4 to the list of movies (you can use any director)
INSERT INTO Movies,
VALUES (4, "Toy Story 4", "John Lasseter", 2017, 123);

-- Toy Story 4 has been released to critical acclaim! It had a rating of 8.7, and made 340 million domestically and 270 million internationally. Add the record to the  BoxOffice table. 
INSERT INTO Boxoffice
VALUES (4, 8.7, 340000000, 270000000);

-- CH14 - Updating rows

-- The director for A Bug's Life is incorrect, it was actually directed by John Lasseter
UPDATE Movies
SET Director = "John Lasseter"
WHERE Id = 2;

-- The year that Toy Story 2 was released is incorrect, it was actually released in 1999
UPDATE Movies
SET Year = "1999"
WHERE Id = 3;

-- Both the title and directory for Toy Story 8 is incorrect! The title should be "Toy Story 3" and it was directed by Lee Unkrich
UPDATE Movies
SET Title = "Toy Story 3", Director = "Lee Unkrich"
WHERE Id = 11;

-- CH15 - Deleting rows

-- This database is getting too big, lets remove all movies that were released before 2005.
DELETE FROM Movies
WHERE Year < 2005;

-- Andrew Stanton has also left the studio, so please remove all movies directed by him.
DELETE FROM Movies
WHERE Director = "Andrew Stanton";

-- CH16 - Creating Tables

-- Create a new table named Database with the following columns:
-- 1. Name A string (text) describing the name of the database
-- 2. Version A number (floating point) of the latest version of this database
-- 3. Download_count An integer count of the number of times this database was downloaded
-- This table has no constraints.
CREATE TABLE Database (
    Name TEXT,
    Version FLOAT,
    Download_Count INTEGER);
    
-- CH17 - Altering Tables

-- Add a column named Aspect_ratio with a FLOAT data type to store the aspect-ratio each movie was released in.
ALTER TABLE Movies
  ADD COLUMN Aspect_ratio FLOAT DEFAULT 3;
  
-- Add another column named Language with a TEXT data type to store the language that the movie was released in. Ensure that the default for this language is English.
ALTER TABLE Movies
  ADD COLUMN Language TEXT DEFAULT "English";

-- CH18 - Dropping Tables

-- We've sadly reached the end of our lessons, lets clean up by removing the Movies table
DROP TABLE Movies;

-- And drop the BoxOffice table as well
DROP TABLE BoxOffice;
