## **Problem Solving Case - BookMyShow**

I have designed four individual tables Theatre, Screen, Movie and Show
which individually storing the respective data for movies with
respective attributes:

## 

## **🎭 1. Theatre Table**

  ----------------------------------------------------------------------
  **Attribute**   **Type**        **Description**
  --------------- --------------- --------------------------------------
  theatre_id      INT (PK)        Unique identifier for each theatre.
                                  Primary Key.

  theatre_name    VARCHAR(100)    Name of the theatre (e.g., PVR
                                  Cinemas, INOX).

  location        VARCHAR(100)    City/area where the theatre is
                                  located.
  ----------------------------------------------------------------------

🔹 **Purpose:** Stores basic information about each theatre location.

## **🎥 2. Screen Table**

  ------------------------------------------------------------------------
  **Attribute**   **Type**      **Description**
  --------------- ------------- ------------------------------------------
  screen_id       INT (PK)      Unique identifier for each screen inside a
                                theatre.

  theatre_id      INT (FK)      Foreign Key referencing
                                Theatre(theatre_id). Defines which theatre
                                this screen belongs to.

  screen_name     VARCHAR(50)   Name/label for the screen (e.g., Screen 1,
                                IMAX, Gold Class).
  ------------------------------------------------------------------------

🔹 **Purpose:** Represents different **auditorium halls** inside a
theatre where shows are scheduled.

## **🎬 3. Movie Table**

  ------------------------------------------------------------------------
  **Attribute**   **Type**       **Description**
  --------------- -------------- -----------------------------------------
  movie_id        INT (PK)       Unique identifier for each movie.

  title           VARCHAR(100)   Name of the movie (e.g., Inception,
                                 Avatar).

  language        VARCHAR(50)    Primary language of the movie (e.g.,
                                 Hindi, English, Tamil).

  genre           VARCHAR(50)    Category of the movie (e.g., Action,
                                 Comedy, Sci-Fi).

  duration        INT            Duration of the movie in minutes.

  dimension       VARCHAR(10)    Format type of the movie, either 2D or
                                 3D. Constraint applied.
  ------------------------------------------------------------------------

🔹 **Purpose:** Stores metadata about movies that can be scheduled in
theatres.

## **🕒 4. Show Table**

  ------------------------------------------------------------------------
  **Attribute**   **Type**   **Description**
  --------------- ---------- ---------------------------------------------
  show_id         INT (PK)   Unique identifier for each show entry.

  screen_id       INT (FK)   Foreign Key referencing Screen(screen_id).
                             Defines which screen is hosting the show.

  movie_id        INT (FK)   Foreign Key referencing Movie(movie_id).
                             Defines which movie is playing in the show.

  show_date       DATE       The calendar date on which the show runs.

  show_time       TIME       The start time of the show.
  ------------------------------------------------------------------------

🔹 **Purpose:** Represents an **instance of a movie screening** at a
particular screen, date, and time.

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

**List of DDL queries for each table**

\-- 1. Theatre Table

CREATE TABLE Theatre (

theatre_id INT PRIMARY KEY,

theatre_name VARCHAR(100) NOT NULL,

location VARCHAR(100) NOT NULL

);

\-- 2. Screen Table

CREATE TABLE Screen (

screen_id INT PRIMARY KEY,

theatre_id INT NOT NULL,

screen_name VARCHAR(50),

FOREIGN KEY (theatre_id) REFERENCES Theatre(theatre_id)

);

\-- 3. Movie Table

CREATE TABLE Movie (

movie_id INT PRIMARY KEY,

title VARCHAR(100) NOT NULL,

language VARCHAR(50) NOT NULL,

genre VARCHAR(50),

duration INT, \-- in minutes

dimension VARCHAR(10) CHECK (dimension IN (\'2D\',\'3D\'))

);

\-- 4. Show Table

CREATE TABLE Show (

show_id INT PRIMARY KEY,

screen_id INT NOT NULL,

movie_id INT NOT NULL,

show_date DATE NOT NULL,

show_time TIME NOT NULL,

FOREIGN KEY (screen_id) REFERENCES Screen(screen_id),

FOREIGN KEY (movie_id) REFERENCES Movie(movie_id)

);

**List of DML queries for each table**

\-- Insert Theatre

INSERT INTO Theatre VALUES (1, \'PVR Cinemas\', \'Bandra, Mumbai\');

INSERT INTO Theatre VALUES (2, \'INOX\', \'Vile Parle, Mumbai\');

\-- Insert Screens

INSERT INTO Screen VALUES (1, 1, \'Screen 1\');

INSERT INTO Screen VALUES (2, 1, \'Screen 2\');

INSERT INTO Screen VALUES (3, 2, \'Screen 1\');

\-- Insert Movies

INSERT INTO Movie VALUES

(1, \'Inception\', \'English\', \'Sci-Fi\', 148, \'2D\'),

(2, \'Avatar: The Way of Water\', \'English\', \'Sci-Fi/Adventure\',
192, \'3D\'),

(3, \'3 Idiots\', \'Hindi\', \'Comedy/Drama\', 170, \'2D\'),

(4, \'KGF 2\', \'Kannada\', \'Action\', 168, \'2D\');

\-- Insert Shows

INSERT INTO Show VALUES (1, 1, 1, \'2025-08-25\', \'18:00:00\');

INSERT INTO Show VALUES (2, 1, 2, \'2025-08-25\', \'21:30:00\');

INSERT INTO Show VALUES (3, 2, 3, \'2025-08-25\', \'19:00:00\');

INSERT INTO Show VALUES (4, 3, 2, \'2025-08-25\', \'20:00:00\');

**P2**

Query to select the list of shows on particular day -

SELECT

t.theatre_name,

m.title AS movie_name,

m.language,

m.dimension,

s.screen_name,

sh.show_date,

sh.show_time

FROM Show sh

JOIN Screen s ON sh.screen_id = s.screen_id

JOIN Theatre t ON s.theatre_id = t.theatre_id

JOIN Movie m ON sh.movie_id = m.movie_id

WHERE t.theatre_name = \'PVR Cinemas\' \-- given theatre

AND sh.show_date = \'2025-08-25\' \-- given date

ORDER BY sh.show_time;
