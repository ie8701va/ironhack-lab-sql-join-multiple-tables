![logo_ironhack_blue 7](https://user-images.githubusercontent.com/23629340/40541063-a07a0a8a-601a-11e8-91b5-2f13e4e6b441.png)

# Lab | SQL Joins en múltiples tablas

En este laboratorio, utilizarás la base de datos Sakila de alquileres de películas.

## Instrucciones

1. Write a query to display each store's ID, city, and country.

SELECT 
	STORE_ID, CITY, COUNTRY
FROM 
	ADDRESS
JOIN 
	STORE ON ADDRESS.ADDRESS_ID  = STORE.ADDRESS_ID
JOIN 
	CITY ON ADDRESS.CITY_ID = CITY.CITY_ID
JOIN
	COUNTRY ON CITY.COUNTRY_ID = COUNTRY.COUNTRY_ID
	
	
2. Write a query to show how much business, in dollars, each store generated.


SELECT 
	SUM(PAYMENT.AMOUNT), STORE.STORE_ID
FROM
	PAYMENT 
JOIN
	STAFF ON PAYMENT.STAFF_ID = STAFF.STAFF_ID
JOIN
	STORE ON STAFF.STORE_ID = STORE.STORE_ID
GROUP BY
    STORE.STORE_ID
	
	
3. What is the average runtime of movies by category?

SELECT 
	AVG(LENGTH), NAME
FROM
	FILM 
JOIN
	FILM_CATEGORY ON FILM.FILM_ID = FILM_CATEGORY.FILM_ID
JOIN
	CATEGORY ON FILM_CATEGORY.CATEGORY_ID = CATEGORY.CATEGORY_ID
GROUP BY
    NAME
	
	
4. Which movie categories have the longest runtimes?

SELECT 
	AVG(LENGTH), 
	NAME,
	SUM(SalesAmount) OVER (PARTITION BY ProductID ORDER BY OrderDate) AS RunningTotal

FROM
	FILM 
JOIN
	FILM_CATEGORY ON FILM.FILM_ID = FILM_CATEGORY.FILM_ID
JOIN
	CATEGORY ON FILM_CATEGORY.CATEGORY_ID = CATEGORY.CATEGORY_ID
GROUP BY
    NAME

	
5. Show the most rented movies in descending order.


SELECT 
    FILM.title AS Movie_Title,
    COUNT(RENTAL.rental_id) AS Rental_Count
FROM 
    rental
JOIN 
    inventory ON rental.inventory_id = inventory.inventory_id
JOIN 
    film ON inventory.film_id = film.film_id
GROUP BY 
    film.film_id, film.title
ORDER BY 
    Rental_Count DESC
LIMIT 5

    
6. List the top five genres by gross revenue in descending order.

SELECT 
    FILM.title AS Movie_Title,
    COUNT(RENTAL.rental_id) AS Rental_Count
FROM 
    rental
JOIN 
    inventory ON rental.inventory_id = inventory.inventory_id
JOIN 
    film ON inventory.film_id = film.film_id
GROUP BY 
    film.film_id, film.title
ORDER BY 
    Rental_Count DESC
LIMIT 5

    
7. Is "Academy Dinosaur" available for rent at Store 1?

SELECT 
    film.title,
    store.store_id
FROM 
    store
JOIN 
    inventory ON store.STORE_ID = inventory.STORE_ID
JOIN 
    film ON film.film_id = inventory.film_id
WHERE
	film.title == 'Academy Dinosaur'
