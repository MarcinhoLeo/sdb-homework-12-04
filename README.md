# Домашнее задание к занятию "`SQL. Часть 2`" - `Гунба Леонардо`

### Задание 1

Одним запросом получите информацию о магазине, в котором обслуживается более 300 покупателей, и выведите в результат следующую информацию:

* фамилия и имя сотрудника из этого магазина;
* город нахождения магазина;
* количество пользователей, закреплённых в этом магазине.

```
SELECT
	s.store_id,
	s2.staff_id,
	CONCAT(s2.first_name,' ',s2.last_name) staff_fio,
	c2.city,
	( SELECT COUNT(1) FROM customer c WHERE c.store_id =s.store_id ) customers_count
FROM 
	store s 
LEFT JOIN staff s2 ON s2.store_id =s.store_id
LEFT JOIN address a ON s.address_id =a.address_id
LEFT JOIN city c2 ON c2.city_id =a.city_id 
WHERE
	( SELECT COUNT(1) FROM customer c WHERE c.store_id =s.store_id ) > 300
;
```

### Задание 2

Получите количество фильмов, продолжительность которых больше средней продолжительности всех фильмов.

```
SELECT
	COUNT(1)
FROM 
	film f
WHERE
	f.`length` > (SELECT AVG(f2.`length`) FROM film f2)
;
```

### Задание 3

Получите информацию, за какой месяц была получена наибольшая сумма платежей и добавьте информацию по количеству аренд за этот месяц.


```sql
SELECT SUM(p.amount) AS 'highest payment',
DATE_FORMAT(p.payment_date, '%Y-%M') AS 'date',
COUNT(r.rental_id) AS 'number of leases'
FROM payment p
LEFT JOIN rental r ON r.rental_id = p.rental_id
GROUP BY DATE_FORMAT(p.payment_date, '%Y-%M')
ORDER BY 1 DESC
LIMIT 1;
```
```
highest payment|date       |number of leases|
---------------+-----------+----------------+
       28368.91|  2005-July|            6709|
```
