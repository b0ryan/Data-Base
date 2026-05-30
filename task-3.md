# Задача 3 — Бронирование отелей

## Скрипты создания таблиц (MySQL)

#### Создание таблицы Hotel

```sql
-- Создание таблицы Hotel
CREATE TABLE Hotel (
    ID_hotel INT PRIMARY KEY,
    name VARCHAR(255) NOT NULL,
    location VARCHAR(255) NOT NULL
);
```

#### Создание таблицы Room

```sql
-- Создание таблицы Room
CREATE TABLE Room (
    ID_room INT PRIMARY KEY,
    ID_hotel INT,
    room_type ENUM('Single', 'Double', 'Suite') NOT NULL,
    price DECIMAL(10, 2) NOT NULL,
    capacity INT NOT NULL,
    FOREIGN KEY (ID_hotel) REFERENCES Hotel(ID_hotel)
);
```

#### Создание таблицы Customer

```sql
-- Создание таблицы Customer
CREATE TABLE Customer (
    ID_customer INT PRIMARY KEY,
    name VARCHAR(255) NOT NULL,
    email VARCHAR(255) UNIQUE NOT NULL,
    phone VARCHAR(20) NOT NULL
);
```

#### Создание таблицы Booking

```sql
-- Создание таблицы Booking
CREATE TABLE Booking (
    ID_booking INT PRIMARY KEY,
    ID_room INT,
    ID_customer INT,
    check_in_date DATE NOT NULL,
    check_out_date DATE NOT NULL,
    FOREIGN KEY (ID_room) REFERENCES Room(ID_room),
    FOREIGN KEY (ID_customer) REFERENCES Customer(ID_customer)
);
```

## Скрипт создания таблиц базы PostgreSQL

#### Создание таблицы Hotel

```sql
-- Создание таблицы Hotel
CREATE TABLE Hotel (
    ID_hotel SERIAL PRIMARY KEY,
    name VARCHAR(255) NOT NULL,
    location VARCHAR(255) NOT NULL
);
```

#### Создание таблицы Room

```sql
-- Создание таблицы Room
CREATE TABLE Room (
    ID_room SERIAL PRIMARY KEY,
    ID_hotel INT,
    room_type VARCHAR(20) NOT NULL CHECK (room_type IN ('Single', 'Double', 'Suite')),
    price DECIMAL(10, 2) NOT NULL,
    capacity INT NOT NULL,
    FOREIGN KEY (ID_hotel) REFERENCES Hotel(ID_hotel)
);
```

#### Создание таблицы Customer

```sql
-- Создание таблицы Customer
CREATE TABLE Customer (
    ID_customer SERIAL PRIMARY KEY,
    name VARCHAR(255) NOT NULL,
    email VARCHAR(255) UNIQUE NOT NULL,
    phone VARCHAR(20) NOT NULL
);
```

#### Создание таблицы Booking

```sql
-- Создание таблицы Booking
CREATE TABLE Booking (
    ID_booking SERIAL PRIMARY KEY,
    ID_room INT,
    ID_customer INT,
    check_in_date DATE NOT NULL,
    check_out_date DATE NOT NULL,
    FOREIGN KEY (ID_room) REFERENCES Room(ID_room),
    FOREIGN KEY (ID_customer) REFERENCES Customer(ID_customer)
);
```

## Скрипт заполнения данных

```sql
-- Вставка данных в таблицу Hotel
INSERT INTO Hotel (ID_hotel, name, location) VALUES
(1, 'Grand Hotel', 'Paris, France'),
(2, 'Ocean View Resort', 'Miami, USA'),
(3, 'Mountain Retreat', 'Aspen, USA'),
(4, 'City Center Inn', 'New York, USA'),
(5, 'Desert Oasis', 'Las Vegas, USA'),
(6, 'Lakeside Lodge', 'Lake Tahoe, USA'),
(7, 'Historic Castle', 'Edinburgh, Scotland'),
(8, 'Tropical Paradise', 'Bali, Indonesia'),
(9, 'Business Suites', 'Tokyo, Japan'),
(10, 'Eco-Friendly Hotel', 'Copenhagen, Denmark');

-- Вставка данных в таблицу Room
INSERT INTO Room (ID_room, ID_hotel, room_type, price, capacity) VALUES
(1, 1, 'Single', 150.00, 1),
(2, 1, 'Double', 200.00, 2),
(3, 1, 'Suite', 350.00, 4),
(4, 2, 'Single', 120.00, 1),
(5, 2, 'Double', 180.00, 2),
(6, 2, 'Suite', 300.00, 4),
(7, 3, 'Double', 250.00, 2),
(8, 3, 'Suite', 400.00, 4),
(9, 4, 'Single', 100.00, 1),
(10, 4, 'Double', 150.00, 2),
(11, 5, 'Single', 90.00, 1),
(12, 5, 'Double', 140.00, 2),
(13, 6, 'Suite', 280.00, 4),
(14, 7, 'Double', 220.00, 2),
(15, 8, 'Single', 130.00, 1),
(16, 8, 'Double', 190.00, 2),
(17, 9, 'Suite', 360.00, 4),
(18, 10, 'Single', 110.00, 1),
(19, 10, 'Double', 160.00, 2);

-- Вставка данных в таблицу Customer
INSERT INTO Customer (ID_customer, name, email, phone) VALUES
(1, 'John Doe', 'john.doe@example.com', '+1234567890'),
(2, 'Jane Smith', 'jane.smith@example.com', '+0987654321'),
(3, 'Alice Johnson', 'alice.johnson@example.com', '+1122334455'),
(4, 'Bob Brown', 'bob.brown@example.com', '+2233445566'),
(5, 'Charlie White', 'charlie.white@example.com', '+3344556677'),
(6, 'Diana Prince', 'diana.prince@example.com', '+4455667788'),
(7, 'Ethan Hunt', 'ethan.hunt@example.com', '+5566778899'),
(8, 'Fiona Apple', 'fiona.apple@example.com', '+6677889900'),
(9, 'George Washington', 'george.washington@example.com', '+7788990011'),
(10, 'Hannah Montana', 'hannah.montana@example.com', '+8899001122');

-- Вставка данных в таблицу Booking
INSERT INTO Booking (ID_booking, ID_room, ID_customer, check_in_date, check_out_date) VALUES
(1, 1, 1, '2025-05-01', '2025-05-05'),
(2, 2, 2, '2025-05-02', '2025-05-06'),
(3, 3, 3, '2025-05-03', '2025-05-07'),
(4, 4, 4, '2025-05-04', '2025-05-08'),
(5, 5, 5, '2025-05-05', '2025-05-09'),
(6, 6, 6, '2025-05-06', '2025-05-10'),
(7, 7, 7, '2025-05-07', '2025-05-11'),
(8, 8, 8, '2025-05-08', '2025-05-12'),
(9, 9, 9, '2025-05-09', '2025-05-13'),
(10, 10, 10, '2025-05-10', '2025-05-14'),
(11, 1, 2, '2025-05-11', '2025-05-15'),
(12, 2, 3, '2025-05-12', '2025-05-14'),
(13, 3, 4, '2025-05-13', '2025-05-15'),
(14, 4, 5, '2025-05-14', '2025-05-16'),
(15, 5, 6, '2025-05-15', '2025-05-16'),
(16, 6, 7, '2025-05-16', '2025-05-18'),
(17, 7, 8, '2025-05-17', '2025-05-21'),
(18, 8, 9, '2025-05-18', '2025-05-19'),
(19, 9, 10, '2025-05-19', '2025-05-22'),
(20, 10, 1, '2025-05-20', '2025-05-22'),
(21, 1, 2, '2025-05-21', '2025-05-23'),
(22, 2, 3, '2025-05-22', '2025-05-25'),
(23, 3, 4, '2025-05-23', '2025-05-26'),
(24, 4, 5, '2025-05-24', '2025-05-25'),
(25, 5, 6, '2025-05-25', '2025-05-27'),
(26, 6, 7, '2025-05-26', '2025-05-29');
```

## Запросы

## Запрос 1

```sql
WITH BookingDetails AS (
    SELECT
        c.ID_customer,
        c.name,
        c.email,
        c.phone,
        b.ID_booking,
        h.ID_hotel,
        h.name AS hotel_name,
        DATEDIFF(b.check_out_date, b.check_in_date) AS stay_days
    FROM Customer c
    JOIN Booking b ON c.ID_customer = b.ID_customer
    JOIN Room r ON b.ID_room = r.ID_room
    JOIN Hotel h ON r.ID_hotel = h.ID_hotel
),
CustomerStats AS (
    SELECT
        name,
        email,
        phone,
        COUNT(ID_booking) AS total_bookings,
        GROUP_CONCAT(DISTINCT hotel_name ORDER BY hotel_name SEPARATOR ', ') AS hotels_list,
        AVG(stay_days) AS avg_stay_days
    FROM BookingDetails
    GROUP BY ID_customer, name, email, phone
    HAVING COUNT(ID_booking) > 2
       AND COUNT(DISTINCT ID_hotel) > 1
)
SELECT
    name,
    email,
    phone,
    total_bookings,
    hotels_list,
    avg_stay_days
FROM CustomerStats
ORDER BY total_bookings DESC;
```

## Запрос 2

```sql
WITH BookingCost AS (
    SELECT
        c.ID_customer,
        c.name,
        b.ID_booking,
        h.ID_hotel,
        r.price * DATEDIFF(b.check_out_date, b.check_in_date) AS booking_cost
    FROM Customer c
    JOIN Booking b ON c.ID_customer = b.ID_customer
    JOIN Room r ON b.ID_room = r.ID_room
    JOIN Hotel h ON r.ID_hotel = h.ID_hotel
),
CustomerAgg AS (
    SELECT
        ID_customer,
        name,
        COUNT(ID_booking) AS total_bookings,
        COUNT(DISTINCT ID_hotel) AS unique_hotels,
        SUM(booking_cost) AS total_spent
    FROM BookingCost
    GROUP BY ID_customer, name
),
MultiHotelCustomers AS (
    SELECT *
    FROM CustomerAgg
    WHERE total_bookings > 2
      AND unique_hotels > 1
),
HighSpenders AS (
    SELECT *
    FROM CustomerAgg
    WHERE total_spent > 500
)
SELECT
    m.ID_customer,
    m.name,
    m.total_bookings,
    m.total_spent,
    m.unique_hotels
FROM MultiHotelCustomers m
JOIN HighSpenders h ON m.ID_customer = h.ID_customer
ORDER BY m.total_spent ASC;
```

## Запрос 3

```sql
WITH HotelCategory AS (
    SELECT
        h.ID_hotel,
        h.name AS hotel_name,
        AVG(r.price) AS avg_price,
        CASE
            WHEN AVG(r.price) < 175 THEN 'дешевый'
            WHEN AVG(r.price) <= 300 THEN 'средний'
            ELSE 'дорогой'
        END AS hotel_category
    FROM Hotel h
    JOIN Room r ON h.ID_hotel = r.ID_hotel
    GROUP BY h.ID_hotel, h.name
),
CustomerHotels AS (
    SELECT DISTINCT
        c.ID_customer,
        c.name,
        hc.hotel_category,
        hc.hotel_name
    FROM Customer c
    JOIN Booking b ON c.ID_customer = b.ID_customer
    JOIN Room r ON b.ID_room = r.ID_room
    JOIN HotelCategory hc ON r.ID_hotel = hc.ID_hotel
),
CustomerPreference AS (
    SELECT
        ID_customer,
        name,
        CASE
            WHEN MAX(CASE WHEN hotel_category = 'дорогой' THEN 1 ELSE 0 END) = 1 THEN 'дорогой'
            WHEN MAX(CASE WHEN hotel_category = 'средний' THEN 1 ELSE 0 END) = 1 THEN 'средний'
            ELSE 'дешевый'
        END AS preferred_hotel_type,
        GROUP_CONCAT(DISTINCT hotel_name ORDER BY hotel_name SEPARATOR ', ') AS visited_hotels
    FROM CustomerHotels
    GROUP BY ID_customer, name
)
SELECT
    ID_customer,
    name,
    preferred_hotel_type,
    visited_hotels
FROM CustomerPreference
ORDER BY
    CASE preferred_hotel_type
        WHEN 'дешевый' THEN 1
        WHEN 'средний' THEN 2
        WHEN 'дорогой' THEN 3
    END,
    name;
```
