# Задача 2 — Гоночные автомобили

## Скрипты создания таблиц (MySQL)

#### Создание таблицы Classes

```sql
-- Создание таблицы Classes
CREATE TABLE Classes (
    class VARCHAR(100) NOT NULL,
    type ENUM('Racing', 'Street') NOT NULL,
    country VARCHAR(100) NOT NULL,
    numDoors INT NOT NULL,
    engineSize DECIMAL(3, 1) NOT NULL, -- размер двигателя в литрах
    weight INT NOT NULL,                -- вес автомобиля в килограммах
    PRIMARY KEY (class)
);
```

#### Создание таблицы Cars

```sql
-- Создание таблицы Cars
CREATE TABLE Cars (
    name VARCHAR(100) NOT NULL,
    class VARCHAR(100) NOT NULL,
    year INT NOT NULL,
    PRIMARY KEY (name),
    FOREIGN KEY (class) REFERENCES Classes(class)
);
```

#### Создание таблицы Races

```sql
-- Создание таблицы Races
CREATE TABLE Races (
    name VARCHAR(100) NOT NULL,
    date DATE NOT NULL,
    PRIMARY KEY (name)
);
```

#### Создание таблицы Results

```sql
-- Создание таблицы Results
CREATE TABLE Results (
    car VARCHAR(100) NOT NULL,
    race VARCHAR(100) NOT NULL,
    position INT NOT NULL,
    PRIMARY KEY (car, race),
    FOREIGN KEY (car) REFERENCES Cars(name),
    FOREIGN KEY (race) REFERENCES Races(name)
);
```

## Скрипт создания таблиц базы PostgreSQL

#### Создание таблицы Classes

```sql
-- Создание таблицы Classes
CREATE TABLE Classes (
class VARCHAR(100) NOT NULL,
type VARCHAR(20) NOT NULL CHECK (type IN ('Racing', 'Street')), -- тип класса
country VARCHAR(100) NOT NULL,
numDoors INT NOT NULL,
engineSize DECIMAL(3, 1) NOT NULL, -- размер двигателя в литрах
weight INT NOT NULL,            -- вес автомобиля в килограммах
PRIMARY KEY (class)
);
```

#### Создание таблицы Cars

```sql
-- Создание таблицы Cars
CREATE TABLE Cars (
name VARCHAR(100) NOT NULL,
class VARCHAR(100) NOT NULL,
year INT NOT NULL,
PRIMARY KEY (name),
FOREIGN KEY (class) REFERENCES Classes(class)
);
```

#### Создание таблицы Races

```sql
-- Создание таблицы Races
CREATE TABLE Races (
name VARCHAR(100) NOT NULL,
date DATE NOT NULL,
PRIMARY KEY (name)
);
```

#### Создание таблицы Results

```sql
-- Создание таблицы Results
CREATE TABLE Results (
car VARCHAR(100) NOT NULL,
race VARCHAR(100) NOT NULL,
position INT NOT NULL,
PRIMARY KEY (car, race),
FOREIGN KEY (car) REFERENCES Cars(name),
FOREIGN KEY (race) REFERENCES Races(name)
);
```

## Скрипт заполнения данных

```sql
-- Вставка данных в таблицу Classes
INSERT INTO Classes (class, type, country, numDoors, engineSize, weight) VALUES
('SportsCar', 'Racing', 'USA', 2, 3.5, 1500),
('Sedan', 'Street', 'Germany', 4, 2.0, 1200),
('SUV', 'Street', 'Japan', 4, 2.5, 1800),
('Hatchback', 'Street', 'France', 5, 1.6, 1100),
('Convertible', 'Racing', 'Italy', 2, 3.0, 1300),
('Coupe', 'Street', 'USA', 2, 2.5, 1400),
('Luxury Sedan', 'Street', 'Germany', 4, 3.0, 1600),
('Pickup', 'Street', 'USA', 2, 2.8, 2000);

-- Вставка данных в таблицу Cars
INSERT INTO Cars (name, class, year) VALUES
('Ford Mustang', 'SportsCar', 2020),
('BMW 3 Series', 'Sedan', 2019),
('Toyota RAV4', 'SUV', 2021),
('Renault Clio', 'Hatchback', 2020),
('Ferrari 488', 'Convertible', 2019),
('Chevrolet Camaro', 'Coupe', 2021),
('Mercedes-Benz S-Class', 'Luxury Sedan', 2022),
('Ford F-150', 'Pickup', 2021),
('Audi A4', 'Sedan', 2018),
('Nissan Rogue', 'SUV', 2020);

-- Вставка данных в таблицу Races
INSERT INTO Races (name, date) VALUES
('Indy 500', '2023-05-28'),
('Le Mans', '2023-06-10'),
('Monaco Grand Prix', '2023-05-28'),
('Daytona 500', '2023-02-19'),
('Spa 24 Hours', '2023-07-29'),
('Bathurst 1000', '2023-10-08'),
('Nürburgring 24 Hours', '2023-06-17'),
('Pikes Peak International Hill Climb', '2023-06-25');

-- Вставка данных в таблицу Results
INSERT INTO Results (car, race, position) VALUES
('Ford Mustang', 'Indy 500', 1),
('BMW 3 Series', 'Le Mans', 3),
('Toyota RAV4', 'Monaco Grand Prix', 2),
('Renault Clio', 'Daytona 500', 5),
('Ferrari 488', 'Le Mans', 1),
('Chevrolet Camaro', 'Monaco Grand Prix', 4),
('Mercedes-Benz S-Class', 'Spa 24 Hours', 2),
('Ford F-150', 'Bathurst 1000', 6),
('Audi A4', 'Nürburgring 24 Hours', 8),
('Nissan Rogue', 'Pikes Peak International Hill Climb', 3);
```

## Запросы

## Запрос 1

```sql
WITH CarStats AS (
    SELECT 
        c.class,
        c.name,
        AVG(r.position) AS avg_position,
        COUNT(r.race)   AS race_count
    FROM Cars c
    JOIN Results r ON c.name = r.car
    GROUP BY c.class, c.name
),
MinAvgPerClass AS (
    SELECT 
        class,
        MIN(avg_position) AS min_avg_position
    FROM CarStats
    GROUP BY class
)
SELECT 
  cs.name,
    cs.class,
    cs.avg_position,
    cs.race_count
FROM CarStats cs
JOIN MinAvgPerClass m ON cs.class = m.class AND cs.avg_position = m.min_avg_position
ORDER BY cs.avg_position, cs.name;
```

## Запрос 2

```sql
WITH CarStats AS (
    SELECT 
        c.name,
        c.class,
        cl.country,
        AVG(r.position) AS avg_position,
        COUNT(r.race)   AS race_count
    FROM Cars c
    JOIN Results r ON c.name = r.car
    JOIN Classes cl ON c.class = cl.class
    GROUP BY c.name, c.class, cl.country
)
SELECT 
    name,
    class,
    avg_position,
    race_count,
    country
FROM CarStats
ORDER BY avg_position, name
LIMIT 1;
```

## Запрос 3

```sql
WITH CarStats AS (
    SELECT 
        c.name,
        c.class,
        cl.country,
        AVG(r.position) AS car_avg_position,
        COUNT(r.race)   AS car_race_count
    FROM Cars c
    JOIN Results r ON c.name = r.car
    JOIN Classes cl ON c.class = cl.class
    GROUP BY c.name, c.class, cl.country
),
ClassAvg AS (
    SELECT 
        c.class,
        AVG(r.position) AS class_avg_position
    FROM Cars c
    JOIN Results r ON c.name = r.car
    GROUP BY c.class
),
MinClassAvg AS (
    SELECT MIN(class_avg_position) AS min_avg
    FROM ClassAvg
),
TargetClasses AS (
    SELECT class
    FROM ClassAvg
    WHERE class_avg_position = (SELECT min_avg FROM MinClassAvg)
),
ClassTotalRaces AS (
    SELECT 
        c.class,
        COUNT(DISTINCT r.race) AS total_races
    FROM Cars c
    JOIN Results r ON c.name = r.car
    WHERE c.class IN (SELECT class FROM TargetClasses)
    GROUP BY c.class
)
SELECT 
    cs.name,
    cs.class,
    cs.car_avg_position AS avg_position,
    cs.car_race_count AS race_count,
    cs.country,
    ctr.total_races
FROM CarStats cs
JOIN TargetClasses tc ON cs.class = tc.class
JOIN ClassTotalRaces ctr ON cs.class = ctr.class
ORDER BY cs.class, cs.name;
```

## Запрос 4

```sql
WITH CarStats AS (
    SELECT 
        c.name,
        c.class,
        cl.country,
        AVG(r.position) AS car_avg_position,
        COUNT(r.race)   AS car_race_count
    FROM Cars c
    JOIN Results r ON c.name = r.car
    JOIN Classes cl ON c.class = cl.class
    GROUP BY c.name, c.class, cl.country
),
ClassStats AS (
    SELECT 
        class,
        AVG(car_avg_position) AS class_avg_position,
        COUNT(*) AS car_count_in_class
    FROM CarStats
    GROUP BY class
    HAVING COUNT(*) >= 2  
)
SELECT 
    cs.name,
    cs.class,
    cs.car_avg_position AS avg_position,
    cs.car_race_count AS race_count,
    cs.country
FROM CarStats cs
JOIN ClassStats cls ON cs.class = cls.class
WHERE cs.car_avg_position < cls.class_avg_position
ORDER BY cs.class, cs.car_avg_position;
```

## Запрос 5

```sql
WITH CarStats AS (
    SELECT 
        c.name,
        c.class,
        cl.country,
        AVG(r.position) AS car_avg_position,
        COUNT(r.race)   AS car_race_count
    FROM Cars c
    JOIN Results r ON c.name = r.car
    JOIN Classes cl ON c.class = cl.class
    GROUP BY c.name, c.class, cl.country
),
LowPositionCars AS (
    SELECT 
        name,
        class,
        country,
        car_avg_position,
        car_race_count
    FROM CarStats
    WHERE car_avg_position > 3.0
),
ClassLowCount AS (
    SELECT 
        class,
        COUNT(*) AS low_position_car_count
    FROM LowPositionCars
    GROUP BY class
),
MaxLowCount AS (
    SELECT MAX(low_position_car_count) AS max_low_count
    FROM ClassLowCount
),
TargetClasses AS (
    SELECT class
    FROM ClassLowCount
    WHERE low_position_car_count = (SELECT max_low_count FROM MaxLowCount)
),
ClassTotalRaces AS (
    SELECT 
        c.class,
        COUNT(DISTINCT r.race) AS total_races_in_class
    FROM Cars c
    JOIN Results r ON c.name = r.car
    WHERE c.class IN (SELECT class FROM TargetClasses)
    GROUP BY c.class
)
SELECT 
    lpc.name,
    lpc.class,
    lpc.car_avg_position AS avg_position,
    lpc.car_race_count AS race_count,
    lpc.country,
    ctr.total_races_in_class
FROM LowPositionCars lpc
JOIN TargetClasses tc ON lpc.class = tc.class
JOIN ClassTotalRaces ctr ON lpc.class = ctr.class
ORDER BY 
    lpc.car_avg_position DESC;
```
