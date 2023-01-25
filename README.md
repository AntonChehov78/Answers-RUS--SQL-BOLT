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

-- Перечислите все здания и должности сотрудников в каждом здании (включая пустующие здания)
SELECT DISTINCT Building_name, Role 
FROM Buildings 
LEFT JOIN employees ON building_name = building;

-- CH8 - A short note on NULLs

-- Найдите имена и должности всех сотрудников, которые не были назначены в здание
SELECT *
FROM Employees
LEFT JOIN Buildings
ON Building_name = Building
WHERE Building IS NULL;

-- В каких зданиях нет сотрудников
SELECT *
FROM Buildings
LEFT JOIN Employees
ON Building_name = Building
WHERE Building IS NULL;

-- CH9 - Запросы с выражениями

-- Перечислите все фильмы и их совокупные продажи в миллионах долларов
SELECT Title, (Domestic_sales + International_sales)/1000000 AS Total_Sales_Millions
FROM Movies
LEFT JOIN Boxoffice ON Id=Movie_Id;

--Перечислите все фильмы и их рейтинги в процентах
SELECT Title, Rating*10 as Percent
FROM Movies
LEFT JOIN Boxoffice ON Id=Movie_Id;

-- Перечислите все фильмы, которые были выпущены за четные годы
SELECT Title, Year
FROM Movies
LEFT JOIN Boxoffice ON Id=Movie_Id
WHERE Year % 2 = 0;

-- CH10 - Запросы с агрегатами (Ч. 1)

-- Найдите самое продолжительное время, в течение которого сотрудник находился в студии
SELECT MAX(Years_employed)
FROM Employees;

-- Для каждой должности найдите среднее количество лет, отработанных сотрудниками в этой должности
SELECT Role, AVG(Years_Employed) 
FROM Employees
GROUP BY Role;

-- Найдите общее количество лет работы сотрудников в каждом здании
SELECT Building, SUM(Years_Employed) 
FROM Employees
GROUP BY Building;

-- CH11 - Запросы с агрегатами (Ч. 2)

-- Найдите количество художников в студии (без указания НАЛИЧИЯ)
SELECT Role, COUNT(*) AS Number_of_Artists
FROM Employees
WHERE Role = "Artist";

-- Найдите количество сотрудников каждой роли в студии
SELECT Role, COUNT(*)
FROM Employees
GROUP BY Role;

-- Найдите общее количество лет, отработанных всеми инженерами
SELECT Role, SUM(Years_Employed)
FROM Employees
GROUP BY Role
HAVING Role = "Engineer";

-- CH12 - Порядок выполнения запроса

-- Найдите количество фильмов, снятых каждым режиссером
SELECT *, COUNT(Title)
FROM Movies
GROUP BY Director;

-- Найдите общий объем внутренних и международных продаж, каждого директора
SELECT Director, sum(Domestic_sales + International_Sales) as Total_Sales
FROM Movies
LEFT JOIN Boxoffice ON Id = Movie_ID
GROUP BY Director;

-- CH13 - Вставка строк
-- Найдите общий объем внутренних и международных продаж, который можно отнести к каждому директору
INSERT INTO Movies,
VALUES (4, "Toy Story 4", "John Lasseter", 2017, 123);

-- История игрушек 4 была выпущена с одобрением критиков! У него был рейтинг 8,7, и он заработал 340 миллионов долларов внутри страны и 270 миллионов долларов на международном уровне. Добавьте запись в таблицу BoxOffice
INSERT INTO Boxoffice
VALUES (4, 8.7, 340000000, 270000000);

-- CH14 - Обновление строк

-- Режиссер фильма "Жизнь жука" указан неверно, на самом деле режиссером был Джон Лассетер
UPDATE Movies
SET Director = "John Lasseter"
WHERE Id = 2;

-- Год выхода "Истории игрушек 2" указан неверно, на самом деле она была выпущена в 1999 году
UPDATE Movies
SET Year = "1999"
WHERE Id = 3;

-- И название, и каталог для Toy Story 8 неверны! Название должно было быть "История игрушек 3", и режиссером фильма был Ли Ункрич
UPDATE Movies
SET Title = "Toy Story 3", Director = "Lee Unkrich"
WHERE Id = 11;

-- CH15 - Удаление строк

-- Эта база данных становится слишком большой, давайте удалим все фильмы, которые были выпущены до 2005 года.
DELETE FROM Movies
WHERE Year < 2005;

--Эндрю Стэнтон также покинул студию, поэтому, пожалуйста, удалите все фильмы, снятые его режиссером.
DELETE FROM Movies
WHERE Director = "Andrew Stanton";

-- CH16 - Создание таблиц

-- Создайте новую таблицу с именем Database со следующими столбцами:
-- 1. Name A string (text) describing the name of the database
-- 2. Version A number (floating point) of the latest version of this database
-- 3. Download_count An integer count of the number of times this database was downloaded
-- This table has no constraints.
CREATE TABLE Database (
    Name TEXT,
    Version FLOAT,
    Download_Count INTEGER);
    
-- CH17 - Изменения таблиц

-- Добавьте столбец с именем Aspect_ratio с типом данных FLOAT для хранения соотношения сторон, в котором был выпущен каждый фильм
ALTER TABLE Movies
  ADD COLUMN Aspect_ratio FLOAT DEFAULT 3;
  
-- Добавьте еще один столбец с именем Language и типом текстовых данных, чтобы сохранить язык, на котором был выпущен фильм. Убедитесь, что по умолчанию для этого языка используется английский.
ALTER TABLE Movies
  ADD COLUMN Language TEXT DEFAULT "English";

-- CH18 - Удаление таблиц

-- К сожалению, мы подошли к концу наших уроков, давайте наведем порядок, убрав таблицу фильмов.
DROP TABLE Movies;

-- И также удалите таблицу BoxOffice
DROP TABLE BoxOffice;
