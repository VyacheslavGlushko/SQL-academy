# 📊 Мои SQL-запросы из SQL Academy

На этой странице представлены мои решения задач из SQL Academy. Демонстрация понимания реляционных баз данных, умение писать SQL-запросы для извлечения, фильтрации и анализа данных.
---

## 🚀 **Базовые SELECT-запросы и фильтрация**

Здесь собраны основные запросы на выборку данных с использованием базовых условий и операторов.

* **Задание 1: Выбрать имена всех пассажиров**
    ```sql
    SELECT name
    FROM passenger;
    ```

* **Задание 2: Выбрать названия всех компаний**
    ```sql
    SELECT name
    FROM company;
    ```

* **Задание 3: Выбрать все данные о рейсах, отправляющихся из "Moscow"**
    ```sql
    SELECT *
    FROM Trip
    WHERE town_from = "Moscow";
    ```

* **Задание 4: Выбрать имена пассажиров, заканчивающиеся на "man"**
    ```sql
    SELECT name
    FROM Passenger
    WHERE name LIKE "%man";
    ```

* **Задание 5: Подсчитать количество рейсов на самолете "TU-134"**
    ```sql
    SELECT COUNT(*) AS COUNT
    FROM Trip
    WHERE plane = "TU-134";
    ```

* **Задание 7: Выбрать уникальные типы самолетов, вылетающих из "Moscow"**
    ```sql
    SELECT DISTINCT plane
    FROM Trip
    WHERE town_from = "Moscow";
    ```

* **Задание 10: Выбрать все данные о рейсах, вылетающих между 10:00 и 14:00 (условно)**
    ```sql
    SELECT *
    FROM Trip
    WHERE time_out BETWEEN '1900-01-01T10:00' AND '1900-01-01T14:00';
    ```

* **Задание 28: Подсчитать количество рейсов из 'Rostov' в 'Moscow'**
    ```sql
    SELECT COUNT(*) AS COUNT
    FROM Trip
    WHERE town_from = 'Rostov' AND town_to = 'Moscow';
    ```

* **Задание 31: Выбрать всех членов семьи, чье имя заканчивается на 'Quincey'**
    ```sql
    SELECT *
    FROM FamilyMembers
    WHERE member_name LIKE '%Quincey';
    ```

* **Задание 34: Подсчитать количество классов, название которых начинается с '10'**
    ```sql
    SELECT COUNT(*) AS count
    FROM class
    WHERE name LIKE '10%';
    ```

* **Задание 36: Выбрать всех студентов, адрес которых начинается с 'ul. Pushkina'**
    ```sql
    SELECT *
    FROM Student
    WHERE address LIKE 'ul. Pushkina%';
    ```

* **Задание 38: Подсчитать количество студентов с именем 'Anna'**
    ```sql
    SELECT COUNT(*) AS count
    FROM Student
    WHERE first_name = 'Anna';
    ```

* **Задание 41: Выбрать время начала 4-й пары**
    ```sql
    SELECT start_pair
    FROM Timepair
    WHERE id = 4;
    ```

* **Задание 59: Выбрать всех пользователей с номером телефона, начинающимся с '+375'**
    ```sql
    SELECT *
    FROM users
    WHERE phone_number LIKE '+375%';
    ```

* **Задание 78: Выбрать всех пользователей с email в домене '@hotmail.com'**
    ```sql
    SELECT *
    FROM Users
    WHERE email LIKE '%@hotmail.com';
    ```

---

## 🔗 **Запросы с JOIN и агрегацией**

В этом разделе представлены запросы, использующие объединение таблиц (`JOIN`) и агрегатные функции (`COUNT`, `SUM`, `MIN`, `MAX`) для получения более сложной информации.

* **Задание 6: Выбрать уникальные названия компаний, у которых есть рейсы на самолете 'Boeing'**
    ```sql
    SELECT DISTINCT name
    FROM Company
    WHERE ID IN (
        SELECT company
        FROM Trip
        WHERE plane = 'Boeing'
    );
    ```

* **Задание 8: Выбрать города прибытия и длительность полета для рейсов из 'Paris'**
    ```sql
    SELECT town_to,
           TIMEDIFF(time_in, time_out) AS flight_time
    FROM Trip
    WHERE town_from = 'Paris';
    ```

* **Задание 9: Выбрать уникальные названия компаний, у которых есть рейсы из "Vladivostok"**
    ```sql
    SELECT DISTINCT name
    FROM Company
    WHERE id IN (
        SELECT company
        FROM Trip
        WHERE town_from = "Vladivostok"
    );
    ```

* **Задание 11: Выбрать имя пассажира с самой длинной длиной имени**
    ```sql
    SELECT name
    FROM Passenger
    WHERE LENGTH(name) = (
        SELECT MAX(LENGTH(name))
        FROM Passenger
    );
    ```

* **Задание 12: Подсчитать количество пассажиров для каждого рейса**
    ```sql
    SELECT trip.id,
           COUNT(passenger) AS COUNT
    FROM Trip
    LEFT JOIN Pass_in_trip ON Pass_in_trip.trip = trip.id
    GROUP BY Trip.id;
    ```

* **Задание 13: Выбрать имена пассажиров, которые встречаются более одного раза (т.е. дубликаты имен)**
    ```sql
    SELECT name
    FROM Passenger
    GROUP BY name
    HAVING COUNT(name) > 1;
    ```

* **Задание 14: Выбрать города прибытия для рейсов, в которых участвовал 'Bruce Willis'**
    ```sql
    SELECT town_to
    FROM Trip
    WHERE id IN (
        SELECT trip
        FROM Pass_in_trip
        WHERE passenger IN (
            SELECT id
            FROM Passenger
            WHERE name = 'Bruce Willis'
        )
    );
    ```

* **Задание 15: Выбрать ID пассажира 'Steve Martin' и время прибытия рейсов в 'London', в которых он участвовал**
    ```sql
    SELECT Passenger.id,
           Trip.time_in
    FROM Passenger
    JOIN Pass_in_trip ON Passenger.id = Pass_in_trip.passenger
    JOIN Trip ON Pass_in_trip.trip = Trip.id
    WHERE Passenger.name = 'Steve Martin' AND Trip.town_to = 'London';
    ```

* **Задание 16: Выбрать имя пассажира и количество его полетов, отсортированных по убыванию количества полетов и имени**
    ```sql
    SELECT p.name,
           COUNT(pit.passenger) AS count
    FROM Passenger AS p
    JOIN Pass_in_trip AS pit ON pit.passenger = p.id
    GROUP BY passenger
    HAVING COUNT(trip) > 0
    ORDER BY COUNT(trip) DESC, name;
    ```

* **Задание 17: Выбрать имя члена семьи, статус и общую стоимость покупок за 2005 год**
    ```sql
    SELECT member_name,
           status,
           SUM(amount * unit_price) AS costs
    FROM FamilyMembers AS fm
    JOIN Payments AS p ON p.family_member = fm.member_id
    WHERE YEAR(date) = 2005
    GROUP BY member_name, status;
    ```

* **Задание 18: Выбрать имя самого старшего члена семьи (по дате рождения)**
    ```sql
    SELECT member_name
    FROM FamilyMembers
    WHERE birthday = (
        SELECT MIN(birthday)
        FROM FamilyMembers
    );
    ```

* **Задание 19: Выбрать уникальные статусы членов семьи, которые покупали 'potato'**
    ```sql
    SELECT DISTINCT FamilyMembers.status
    FROM FamilyMembers
    JOIN Payments ON FamilyMembers.member_id = Payments.family_member
    JOIN Goods ON Payments.good = Goods.good_id
    WHERE Goods.good_name = 'potato';
    ```

* **Задание 21: Выбрать названия товаров, которые покупались более одного раза**
    ```sql
    SELECT good_name
    FROM Goods AS g
    JOIN Payments AS p ON p.good = g.good_id
    GROUP BY good
    HAVING COUNT(*) > 1;
    ```

* **Задание 22: Выбрать уникальные имена членов семьи со статусом 'mother'**
    ```sql
    SELECT DISTINCT member_name
    FROM FamilyMembers
    WHERE status = 'mother'
    GROUP BY member_name;
    ```

* **Задание 37: Выбрать минимальный возраст студентов (в годах)**
    ```sql
    SELECT MIN(TIMESTAMPDIFF(YEAR, birthday, NOW())) AS year
    FROM Student;
    ```

* **Задание 39: Подсчитать количество студентов в классе '10 B'**
    ```sql
    SELECT COUNT(*) AS COUNT
    FROM class
    JOIN Student_in_class ON Student_in_class.class = Class.id
    WHERE Class.name = '10 B';
    ```

* **Задание 46: Выбрать уникальные названия классов, в которых преподает учитель 'Krauze'**
    ```sql
    SELECT DISTINCT c.name
    FROM Schedule AS s
    JOIN Teacher t ON t.id = s.teacher
    JOIN Class c ON c.id = s.class
    WHERE t.last_name = 'Krauze';
    ```

* **Задание 48: Выбрать название класса и количество студентов в нем, отсортированные по убыванию количества студентов**
    ```sql
    SELECT name,
           COUNT(student) AS count
    FROM Class AS c
    JOIN Student_in_class AS sic ON sic.class = c.id
    GROUP BY name
    ORDER BY count DESC;
    ```

---

## 🛠️ **Операции изменения данных (DML)**

Этот раздел демонстрирует навыки обновления и удаления данных.

* **Задание 53: Обновить имя члена семьи с "Andie Quincey" на "Andie Anthony"**
    ```sql
    UPDATE FamilyMembers
    SET member_name = "Andie Anthony"
    WHERE member_name = "Andie Quincey";
    ```

* **Задание 54: Удалить всех членов семьи, чье имя заканчивается на 'Quincey'**
    ```sql
    DELETE FROM FamilyMembers
    WHERE member_name LIKE '%Quincey';
    ```

* **Задание 56: Удалить все рейсы, вылетающие из 'Moscow'**
    ```sql
    DELETE FROM Trip
    WHERE town_from = 'Moscow';
    ```

---

## 📊 **Дополнительные и специфичные запросы**

Запросы, которые демонстрируют специфические функции или подходы.

* **Задание 74: Выбрать ID комнат и преобразовать булево значение has_internet в 'YES'/'NO'**
    ```sql
    SELECT id,
           IF(has_internet = 1, 'YES', 'NO') AS has_internet
    FROM Rooms;
    ```
