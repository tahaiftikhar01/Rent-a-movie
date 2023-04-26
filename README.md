# Rent-a-movie
College Project on SQL 
install.packages("DBI", dependencies = TRUE)
install.packages("RSQLite", dependencies = TRUE)

library(DBI)
library(RSQLite)

drv <- dbDriver("SQLite")
con <- dbConnect(drv, dbname = "rent-a-movie.db")
dbListTables(con)

#how to write a query
dbGetQuery(con, "SELECT country,SUM (amount) AS Sum
           FROM country
           LEFT JOIN city
           ON city.country_id = country.country_id
           LEFT JOIN address on address.city_id = city.city_id
           LEFT JOIN customer on customer.address_id = address.address_id
           LEFT JOIN payment ON payment.customer_id = customer.customer_id
           WHERE payment.amount >= 11
           GROUP BY country")
#USING funtion can be used if the name of two tables is the same in JOINS

genre <- dbGetQuery(con, "SELECT DISTINCT name
                           FROM category
                           JOIN film_category USING (category_id)
                           JOIN film USING (film_id)
                           JOIN inventory USING (film_id)
                           JOIN rental USING (inventory_id)
                           JOIN payment USING (rental_id)
                           WHERE amount >= 11")
