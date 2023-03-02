# SQL-Portfolio
#### Several data analysis tasks made using SQL 
This different data analysis case study resolved using SQL language 

## Case 1:
We'll make use of details regarding a publishing business that issues both translated and original works. `Books`,`Authors`, `Editors`, and `Translators` are the four tables in our database.

- **`Books`**

| ID  | Title | Type  | author_id | editor_id  | translator_id |
| :------------- | :------------- | :------------- | :------------- | :------------- | :------------- |
| 1  |Harry Potter and the Chamber of Secrets   |Original | 11  | 20 |   |
| 2  | Oranges  | Translated  | 12  | 25  | 31  |
| 3  | The Changeling  | Original  | 13  | 21  |   |
| 4  | The Lord of the Rings: Complete Visual Companion | Original  | 14 | 22  |  |
| 5  | Applied AI | Translated | 16 |24  | 34  |
| 6  | Your Trip  | Translated | 15  | 23  | 32  |
| 7  | The Control of Nature | Original | 17  | 26  |   |
| 8  | Your Happy Life	 | Translated | 15  | 23  | 33  |

- **`Authors`**

| ID  | first_name | last_name  | 
| :------------- | :------------- | :------------- | 
| 11  |J.K   |Rowling |
| 12  | Olga   | Savelieva  | 
| 13  |Kate   | Horsley  |
| 14  |Jude  | Fisher  |
| 15  | Yao  | Dou  |
| 16  | Jack  | Smart  |
| 17  | John   | McPhee  |


- **`Editors`**

| ID  | first_name | last_name  | 
| :------------- | :------------- | :------------- | 
| 20  | Peter  | Honess  |
| 21  | Alton    | Raible  |
| 23  | Mark  | Johnson  |
| 24  | Maria  | Evans  |
| 25  | Sebastian  | Wright  |
| 26  | Joshua   | Rothman   |

- **`translators`**

| ID  | first_name | last_name  | 
| :------------- | :------------- | :------------- | 
| 31  |Ira   |Davies |
| 32  | Ling   | Weng  | 
| 33  |Kristian  | Green  |
| 34  |Roman  | Edwards  |



#### Task 1/ Query book titles along with their authors (i.e., the author’s first name and last name):
```sql
  SELECT b.id, b.title, a.first_name, a.last_name
  FROM books b
  INNER JOIN authors a
  ON b.author_id = a.id
  ORDER BY b.id;
```

#### Task 2/ Query books along with their translators (i.e., the translator’s last name). Only half of  books have been translated and thus have a corresponding translator:
```sql
  SELECT b.id, b.title, b.type, t.last_name AS translator
  FROM books b
  JOIN translators t
  ON b.translator_id = t.id
  ORDER BY b.id;
```
 
#### Task 3/ Query information about each book’s author and translator (i.e., their last names). We also want to keep the basic information about each book (i.e., id, title, and type).
```sql
  SELECT b.id, b.title, b.type, a.last_name AS author,
  t.last_name AS translator
  FROM books b
  LEFT JOIN authors a
  ON b.author_id = a.id
  LEFT JOIN translators t
  ON b.translator_id = t.id
  ORDER BY b.id;
```
  
#### Task 4/Query the basic book information (i.e., ID and title) along with the last names of the corresponding editors. Again, we want to keep all of the books in the result set.
```sql
  SELECT b.id, b.title, e.last_name AS editor
  FROM books b
  LEFT JOIN editors e
  ON b.editor_id = e.id
  ORDER BY b.id;
```
#### Task 5/ Join all four tables to get information about all of the books, authors, editors, and translators in one table.
```sql
SELECT b.id, b.title, a.last_name AS author, e.last_name AS editor,
t.last_name AS translator
FROM books b
FULL JOIN authors a
ON b.author_id = a.id
FULL JOIN editors e
ON b.editor_id = e.id
FULL JOIN translators t
ON b.translator_id = t.id
ORDER BY b.id;
```
## Case 2:
 We have the two tables below :

- **`Customer `**

| customer_id  | age | location  | gender|
| :------------- | :------------- | :------------- |  :------------- | 
| 106  |34   |Ariena ||Male |
| 107  | 30   | Sfax  |  Male  | 
| 108  |42  | Sousse  |Female  |
| 110  |40  |Ariena ||Male |
| 140  |40  | Sousse  |Female  |
| 165  |52  |Ariena ||Male |
| 201  |37  | Monastir  |Female  |
 
 
 - **`Order `**
 
 | order_id  | customer_id | date  | amount| is_sale |
| :------------- | :------------- | :------------- |  :------------- | :------------- | 
| 23001  |120   |2021-01-30 |104.10 |True |
| 23006  |124   | 2021-02-14  | 84.20 | False  | 
| 23009  |165  | 2021-04-13  |46.60 |True  |
| 23012  |167  |2021-04-07 |66.30|True |
| 23020  |201  |2021-03-04 |10.60|False |


#### Task 1/ We want to see the average age of customers who made a purchase on 2020–04–17.

```sql
  select avg(customer.age), orders.date
  from customer
  join orders
  on customer.customer_id = orders.customer_id
  where orders.date = '2020–04–17';
```

#### Task 2/ We want to see the average order amount made by customers in Ariena.

```sql
select avg(orders.amount)
from customer
join orders
on customer.customer_id = orders.customer_id
where customer.location = "Ariena";
```

#### Task 3/ We want to see the average order amount for each city.
```sql
select customer.location, avg(orders.amount)
from customer
join orders
on customer.customer_id = orders.customer_id
group by customer.location;
```
#### Task 4/ We want to see the highest order amount and the age of customer who made that order.
```sql
select customer.age, orders.amount
from customer
join orders
on customer.customer_id = orders.customer_id
order by orders.amount desc
limit 1;
```

#### Task 5/ We want to see the highest order amount made by customer whose id is 201.
```sql
select max(orders.amount) 
from customer 
join orders 
on customer.customer_id = orders.customer_id 
where customer.customer_id = 201;
```
#### Task 6/ We want to see the top 5 customers from Sousse in terms of the highest average order amount when there is sale.
```sql
select c.cust_id, avg(o.amount) as average 
from customer c 
join orders o 
on c.customer_id = o.customer_id 
where c.location = "Sousse" and o.is_sale = "True" 
 group by c.customer_id 
 order by average desc limit 5;
 ```
#### Task 7/ We want to find out the location of the customer who has the lowest order amount on 2021–01–30.
```sql
select c.customer_id , c.location
from customer c
join orders o
on c.customer_id  = o.customer_id 
where o.date = "2021–01–30" and o.amount = (
select min(amount) from orders where date = "2021–01–30"
 );
 ```
## Case 3:
