
--ЗАДАНИЕ №1
--Выведите для каждого покупателя его адрес проживания, 
--город и страну проживания.
```sql
select
	concat(last_name, ' ', first_name) "Customer name",
	a.address, c2.city, c3.country
from customer c
join address a on a.address_id = c.address_id
join city c2 on a.city_id = c2.city_id
join country c3 on c2.country_id = c3.country_id
```

--ЗАДАНИЕ №2
--С помощью SQL-запроса посчитайте для каждого магазина количество его покупателей.
```sql
select
	s.store_id "ID магазина",
	count(customer_id) "Количество покупателей" 
from store s 
join customer c on s.store_id = c.store_id
group by 1
```
--Доработайте запрос и выведите только те магазины, 
--у которых количество покупателей больше 300-от.
--Для решения используйте фильтрацию по сгруппированным строкам 
--с использованием функции агрегации.
```sql
select
	s.store_id "ID магазина",
	count(customer_id) "Количество покупателей" 
from store s 
join customer c on s.store_id = c.store_id
group by 1
having count(customer_id) > 300
```



-- Доработайте запрос, добавив в него информацию о городе магазина, 
--а также фамилию и имя продавца, который работает в этом магазине.
```sql
select
	s.store_id "ID магазина",
	count(customer_id) "Количество покупателей" ,
	c2.city "Город",
	s2.last_name || ' ' || s2.first_name "Имя сотрудника"
	from store s 
join customer c on s.store_id = c.store_id
join staff s2 on s.store_id = s2.store_id
join address a on s.address_id = a.address_id
join city c2 on c2.city_id = a.city_id
group by 1, 3, 4
having count(customer_id) > 300
```



--ЗАДАНИЕ №3
--Выведите ТОП-5 покупателей, 
--которые взяли в аренду за всё время наибольшее количество фильмов
```sql
select
	last_name || ' ' || first_name "Фамилия и имя покупателя",
	count(r.rental_id) "Количество фильмов"
from customer c 
join rental r on c.customer_id = r.customer_id
group by 1
order by 2 desc
limit 5
```


--ЗАДАНИЕ №4
--Посчитайте для каждого покупателя 4 аналитических показателя:
--  1. количество фильмов, которые он взял в аренду
--  2. общую стоимость платежей за аренду всех фильмов (значение округлите до целого числа)
--  3. минимальное значение платежа за аренду фильма
--  4. максимальное значение платежа за аренду фильма
```sql
select 
	last_name || ' ' || first_name "Фамилия и имя покупателя",
	count(r.rental_id) "Количество фильмов",
	round(sum(amount)) "Общая стоимость платежей",
	min(amount) "Минимальная стоимость платежа",
	max(amount) "Максимальная стоимость платежа"
from customer c
join rental r on c.customer_id = r.customer_id
join payment p on p.rental_id = r.rental_id
group by 1
```

--ЗАДАНИЕ №5
--Используя данные из таблицы городов составьте одним запросом всевозможные пары городов таким образом,
 --чтобы в результате не было пар с одинаковыми названиями городов. 
 --Для решения необходимо использовать декартово произведение.
 ```sql
select c1.city "Город 1", c2.city "Город 2"
from city c1, city c2
where c1.city != c2.city
```


--ЗАДАНИЕ №6
--Используя данные из таблицы rental о дате выдачи фильма в аренду (поле rental_date)
--и дате возврата фильма (поле return_date), 
--вычислите для каждого покупателя среднее количество дней, за которые покупатель возвращает фильмы.
```sql
select
	customer_id "ID покупателя",
	round(avg(return_date::date - rental_date::date), 2) "Среднее количество дней на возврат"
from rental r 
group by 1
order by 1
```
