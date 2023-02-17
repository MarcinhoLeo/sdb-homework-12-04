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


```
SELECT 
	summs.mon monn, summs.summ, 
	count(r.rental_id)
FROM
	(SELECT
		MONTH(p.payment_date) mon,
		SUM(p.amount) summ
	FROM payment p 
	GROUP BY MONTH(p.payment_date)
	ORDER BY summ DESC
	LIMIT 1
	) summs
left join rental r on MONTH(r.rental_date)=summs.mon
GROUP BY monn
;
```
