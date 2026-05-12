[TALLER02_DianaZetabobo.sql](https://github.com/user-attachments/files/27619306/TALLER02_DianaZetabobo.sql)
-- Seleccionar Base de Datos Sakila
USE sakila;
--------------------------------
-- PARTE 1 - SELECT Y WHERE

-- Mostrar nombre y apellido de todos los clientes

SELECT first_name, last_name
FROM customer;

-- Mostrar peliculas con duracion mayor a 120 minutos
SELECT title, length
FROM film
WHERE length > 120;

-- PARTE 2 - ORDER BY
-- Ordenar clientes por apellido de la A a la Z

SELECT first_name, last_name
FROM customer
ORDER BY last_name ASC;

-- Mostrar las 5 peliculas mas largas
SELECT title, length
FROM film
ORDER BY length DESC
LIMIT 5;

-- PARTE 3 - INNER JOIN
-- Mostrar cantidad pagada y fecha del pago
-- junto con el nombre y apellido del cliente
SELECT customer.first_name,
       customer.last_name,
       payment.amount,
       payment.payment_date
FROM payment
INNER JOIN customer
ON payment.customer_id = customer.customer_id;

-- Mostrar peliculas alquiladas y fecha de alquiler
SELECT film.title,
       rental.rental_date
FROM rental
INNER JOIN inventory
ON rental.inventory_id = inventory.inventory_id
INNER JOIN film
ON inventory.film_id = film.film_id;

-- PARTE 4 - LEFT JOIN
-- Mostrar clientes que no tienen pagos
SELECT customer.first_name,
       customer.last_name
FROM customer
LEFT JOIN payment
ON customer.customer_id = payment.customer_id
WHERE payment.payment_id IS NULL;

-- Mostrar peliculas que no tienen actores relacionados
SELECT film.title,
       film.length
FROM film
LEFT JOIN film_actor
ON film.film_id = film_actor.film_id
WHERE film_actor.actor_id IS NULL;

-- PARTE 5 - INSERT, UPDATE Y DELETE
-- Insertar actor temporal

INSERT INTO actor (first_name, last_name)
VALUES ('DIANA', 'ZETA');

-- Actualizar apellido del actor temporal

SELECT actor_id, first_name, last_name
FROM actor
WHERE first_name = 'DIANA';

UPDATE actor
SET last_name = 'ZETA'
WHERE actor_id = 201;

DELETE FROM actor
WHERE actor_id = 201;

SELECT actor_id,
       first_name,
       last_name
FROM actor
WHERE actor_id = 201;

-- PARTE 6 -CONSULTAS AVANZADAS

-- Mostrar los 5 clientes que mas dinero han pagado

SELECT customer.first_name,
       customer.last_name,
       SUM(payment.amount) AS total_pagado
FROM payment
INNER JOIN customer
ON payment.customer_id = customer.customer_id
GROUP BY customer.customer_id
ORDER BY total_pagado DESC
LIMIT 5;

-- Mostrar las 5 peliculas mas alquiladas
SELECT film.title,
       COUNT(rental.rental_id) AS veces_alquilada
FROM rental
INNER JOIN inventory
ON rental.inventory_id = inventory.inventory_id
INNER JOIN film
ON inventory.film_id = film.film_id
GROUP BY film.film_id
ORDER BY veces_alquilada DESC
LIMIT 5;
