# SQL--PROJECT-CineStream-Backend-Database
CineStream Backend Database is a relational database management system designed for a movie streaming platform. It manages movies, users, and watch history using SQL, supports CRUD operations, enforces data integrity through primary and foreign keys, and generates analytical reports using JOIN and GROUP BY queries.


mysql> CREATE DATABASE CineStreamDB;
Query OK, 1 row affected (0.04 sec)

mysql> USE CineStreamDB;
Database changed
mysql> CREATE TABLE Movies (
    ->     movie_id INT AUTO_INCREMENT PRIMARY KEY,
    ->     title VARCHAR(100) NOT NULL,
    ->     genre VARCHAR(50) NOT NULL,
    ->     release_year INT,
    ->     rental_rate DECIMAL(5,2)
    -> );
Query OK, 0 rows affected (0.12 sec)

mysql> SELECT * FROM Movies;
Empty set (0.02 sec)

mysql> INSERT INTO Movies(title, genre, release_year, rental_rate)
    -> VALUES
    -> ('Avengers: Endgame', 'Action', 2019, 5.99),
    -> ('The Dark Knight', 'Action', 2008, 4.99),
    -> ('Inception', 'Sci-Fi', 2010, 4.99),
    -> ('Titanic', 'Romance', 1997, 3.99);
Query OK, 4 rows affected (0.04 sec)
Records: 4  Duplicates: 0  Warnings: 0

mysql> SELECT * FROM Movies;
+----------+-------------------+---------+--------------+-------------+
| movie_id | title             | genre   | release_year | rental_rate |
+----------+-------------------+---------+--------------+-------------+
|        1 | Avengers: Endgame | Action  |         2019 |        5.99 |
|        2 | The Dark Knight   | Action  |         2008 |        4.99 |
|        3 | Inception         | Sci-Fi  |         2010 |        4.99 |
|        4 | Titanic           | Romance |         1997 |        3.99 |
+----------+-------------------+---------+--------------+-------------+
4 rows in set (0.00 sec)

mysql> CREATE TABLE Users (
    ->     user_id INT AUTO_INCREMENT PRIMARY KEY,
    ->     username VARCHAR(50) UNIQUE NOT NULL,
    ->     email VARCHAR(100) UNIQUE NOT NULL,
    ->     join_date DATE,
    ->     status VARCHAR(10)
    -> );
Query OK, 0 rows affected (0.07 sec)

mysql> INSERT INTO Users(username, email, join_date, status)
    -> VALUES
    -> ('johndoe123', 'john@gmail.com', '2025-02-15', 'Inactive'),
    -> ('alice01', 'alice@gmail.com', '2025-03-10', 'Active'),
    -> ('bob22', 'bob@gmail.com', '2024-12-05', 'Active');
Query OK, 3 rows affected (0.04 sec)
Records: 3  Duplicates: 0  Warnings: 0

mysql> SELECT * FROM Users;
+---------+------------+-----------------+------------+----------+
| user_id | username   | email           | join_date  | status   |
+---------+------------+-----------------+------------+----------+
|       1 | johndoe123 | john@gmail.com  | 2025-02-15 | Inactive |
|       2 | alice01    | alice@gmail.com | 2025-03-10 | Active   |
|       3 | bob22      | bob@gmail.com   | 2024-12-05 | Active   |
+---------+------------+-----------------+------------+----------+
3 rows in set (0.00 sec)

mysql> CREATE TABLE WatchLog (
    ->     log_id INT AUTO_INCREMENT PRIMARY KEY,
    ->     user_id INT,
    ->     movie_id INT,
    ->     watch_date DATETIME,
    ->     duration_minutes INT,
    ->
    ->     FOREIGN KEY (user_id)
    ->         REFERENCES Users(user_id),
    ->
    ->     FOREIGN KEY (movie_id)
    ->         REFERENCES Movies(movie_id)
    -> );
Query OK, 0 rows affected (0.11 sec)

mysql> INSERT INTO WatchLog(user_id, movie_id, watch_date, duration_minutes)
    -> VALUES
    -> (2, 1, '2026-06-10 18:30:00', 120),
    -> (2, 3, '2026-06-11 19:00:00', 140),
    -> (3, 2, '2026-06-11 20:15:00', 150);
Query OK, 3 rows affected (0.04 sec)
Records: 3  Duplicates: 0  Warnings: 0

mysql> SELECT * FROM WatchLog;
+--------+---------+----------+---------------------+------------------+
| log_id | user_id | movie_id | watch_date          | duration_minutes |
+--------+---------+----------+---------------------+------------------+
|      1 |       2 |        1 | 2026-06-10 18:30:00 |              120 |
|      2 |       2 |        3 | 2026-06-11 19:00:00 |              140 |
|      3 |       3 |        2 | 2026-06-11 20:15:00 |              150 |
+--------+---------+----------+---------------------+------------------+
3 rows in set (0.00 sec)

mysql> INSERT INTO Movies
    -> (title, genre, release_year, rental_rate)
    -> VALUES
    -> ('Interstellar', 'Sci-Fi', 2014, 4.99);
Query OK, 1 row affected (0.04 sec)

mysql> SELECT * FROM Movies;
+----------+-------------------+---------+--------------+-------------+
| movie_id | title             | genre   | release_year | rental_rate |
+----------+-------------------+---------+--------------+-------------+
|        1 | Avengers: Endgame | Action  |         2019 |        5.99 |
|        2 | The Dark Knight   | Action  |         2008 |        4.99 |
|        3 | Inception         | Sci-Fi  |         2010 |        4.99 |
|        4 | Titanic           | Romance |         1997 |        3.99 |
|        5 | Interstellar      | Sci-Fi  |         2014 |        4.99 |
+----------+-------------------+---------+--------------+-------------+
5 rows in set (0.00 sec)

mysql> SELECT *
    -> FROM Users
    -> WHERE join_date > '2025-01-01'
    -> AND status = 'Active';
+---------+----------+-----------------+------------+--------+
| user_id | username | email           | join_date  | status |
+---------+----------+-----------------+------------+--------+
|       2 | alice01  | alice@gmail.com | 2025-03-10 | Active |
+---------+----------+-----------------+------------+--------+
1 row in set (0.03 sec)

mysql> UPDATE Users
    -> SET status = 'Active'
    -> WHERE username = 'johndoe123';
Query OK, 1 row affected (0.04 sec)
Rows matched: 1  Changed: 1  Warnings: 0

mysql> SELECT * FROM Users;
+---------+------------+-----------------+------------+--------+
| user_id | username   | email           | join_date  | status |
+---------+------------+-----------------+------------+--------+
|       1 | johndoe123 | john@gmail.com  | 2025-02-15 | Active |
|       2 | alice01    | alice@gmail.com | 2025-03-10 | Active |
|       3 | bob22      | bob@gmail.com   | 2024-12-05 | Active |
+---------+------------+-----------------+------------+--------+
3 rows in set (0.00 sec)

mysql> DELETE FROM Users
    -> WHERE status = 'Inactive'
    -> AND user_id NOT IN
    -> (
    ->     SELECT user_id
    ->     FROM WatchLog
    -> );
Query OK, 0 rows affected (0.03 sec)

mysql> SELECT * FROM Users;
+---------+------------+-----------------+------------+--------+
| user_id | username   | email           | join_date  | status |
+---------+------------+-----------------+------------+--------+
|       1 | johndoe123 | john@gmail.com  | 2025-02-15 | Active |
|       2 | alice01    | alice@gmail.com | 2025-03-10 | Active |
|       3 | bob22      | bob@gmail.com   | 2024-12-05 | Active |
+---------+------------+-----------------+------------+--------+
3 rows in set (0.00 sec)

mysql> SELECT
    -> u.username,
    -> m.title,
    -> w.watch_date,
    -> w.duration_minutes
    -> FROM WatchLog w
    -> INNER JOIN Users u
    -> ON w.user_id = u.user_id
    -> INNER JOIN Movies m
    -> ON w.movie_id = m.movie_id;
+----------+-------------------+---------------------+------------------+
| username | title             | watch_date          | duration_minutes |
+----------+-------------------+---------------------+------------------+
| alice01  | Avengers: Endgame | 2026-06-10 18:30:00 |              120 |
| alice01  | Inception         | 2026-06-11 19:00:00 |              140 |
| bob22    | The Dark Knight   | 2026-06-11 20:15:00 |              150 |
+----------+-------------------+---------------------+------------------+
3 rows in set (0.03 sec)

mysql> SELECT
    -> m.genre AS Genre_Name,
    -> COUNT(w.log_id) AS Total_Watch_Count
    -> FROM Movies m
    -> INNER JOIN WatchLog w
    -> ON m.movie_id = w.movie_id
    -> GROUP BY m.genre
    -> ORDER BY Total_Watch_Count DESC;
+------------+-------------------+
| Genre_Name | Total_Watch_Count |
+------------+-------------------+
| Action     |                 2 |
| Sci-Fi     |                 1 |
+------------+-------------------+
2 rows in set (0.04 sec)
