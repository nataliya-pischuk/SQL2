# Домашнее задание к занятию «SQL. Часть 2»

## Задание 1: Информация о магазине с более чем 300 покупателями
Условие задачи

Одним запросом получите информацию о магазине, в котором обслуживается более 300 покупателей, и выведите в результат следующую информацию:

    Фамилия и имя сотрудника из этого магазина
    Город нахождения магазина
    Количество пользователей, закреплённых в этом магазине
## 
SELECT 
    s.last_name AS staff_last_name,
    s.first_name AS staff_first_name,
    c.city AS store_city,
    COUNT(cust.customer_id) AS customer_count
FROM store st
JOIN staff s ON st.manager_staff_id = s.staff_id
JOIN address a ON st.address_id = a.address_id
JOIN city c ON a.city_id = c.city_id
JOIN customer cust ON st.store_id = cust.store_id
GROUP BY st.store_id, s.staff_id, s.last_name, s.first_name, c.city
HAVING COUNT(cust.customer_id) > 300;

## Задание 2: Количество фильмов длиннее средней продолжительности

SELECT COUNT(*) AS films_above_average_length
FROM film
WHERE length > (
    SELECT AVG(length)
    FROM film
    WHERE length IS NOT NULL
);

## Задание 3: Месяц с наибольшей суммой платежей
SELECT 
    p.payment_date AS payment_month,
    SUM(p.amount) AS total_payment_amount,
    COUNT(DISTINCT r.rental_id) AS rental_count
FROM payment p
JOIN rental r ON p.rental_id = r.rental_id
GROUP BY p.payment_date
ORDER BY total_payment_amount DESC
LIMIT 1;


