# SQL-Portfolio

#### These are  different data analysis cases  resolved using SQL language 

## Case 1: Fictional school

<details>
  <summary>Click to expand</summary>

We habe three tables below for a fictional school that tracks student grades:

- **`students`**

| student_id | first_name | last_name   | email                       |
|------------|------------|-------------|-----------------------------|
| 1          | Fatma      | Ben Youssef | fatma.benyoussef@esprix.com |
| 2          | Amine      | Ben Ali     | amine.benali@esprix.com    |
| 3          | Hela       | Toumi       | hela.toumi@esprix.com      |
| 4          | Nizar      | Gharsa      | nizar.gharsa@esprix.com     |
| 5          | Amina      | Khelifi     | amina.khelifi@esprix.com   |
| 6          | Mohamed    | Ben Ali     | mohamed.benali@esprix.com  |
| 7          | Asma       | Saadi       | asma.saadi@esprix.com      |

-**`courses`**

| course_id | course_name | instructor_id |
|-----------|-------------|---------------|
| 1         | Math 101    | 1             |
| 2         | English 101 | 2             |
| 3         | History 101 | 3             |

-**` instructors`**

| instructor_id | first_name | last_name    | email                        |
|---------------|------------|--------------|------------------------------|
| 1             | Ali        | Hamdi        | ali.hamdi@esprix.com        |
| 2             | Zainab     | Fehmi        | zainab.fehmi@esprix.com      |
| 3             | Youssef    | Bel Haj Amor | youssef.belhajamor@esprix.com|

-**` instructors`**

| enrollment_id | student_id | course_id    | grade                        |
|---------------|------------|--------------|------------------------------|
| 1             | 1        | 1        | 80      |
| 2             | 2     | 2        | 90      |
| 3             | 3    | 3 | 85|
| 4             | 4       | 1       | 75       |
| 5             | 5     | 2        | 92      |
| 6             | 6    | 3 | 87|
| 7             | 7       | 4        | 88        |
| 8             | 1     | 5        | 91      |
| 9             | 2    | 1 | 83|
| 10             | 3        | 2        | 79        |
| 11             | 4     | 3       | 81      |
| 12             | 5    | 4 | 95|


The `Students` table contains information about the students enrolled in the school, including their unique ID, first name, last name, and email. The `Courses` table lists all the courses offered by the school and includes the course ID, name, and instructor ID. The `Instructors` table contains information about the instructors who teach at the school, including their unique ID, first name, last name, and email. Finally, the `Grades` table contains information about the grades earned by each student in each course, including the student ID, course ID, and actual grade earned.


#### Task 1/ What are the names of all students who are enrolled in "Mathematics" course?
```sql
 SELECT s.first_name, s.last_name
FROM students s
INNER JOIN grades g ON s.student_id = g.student_id
INNER JOIN courses c ON g.course_id = c.course_id
WHERE c.course_name = 'Mathematics';
```

#### Task 2/ What are the names of all instructors who taught a course with a grade of "A"?

```sql
 SELECT DISTINCT i.first_name, i.last_name
FROM instructors i
INNER JOIN courses c ON i.instructor_id = c.instructor_id
INNER JOIN grades g ON c.course_id = g.course_id
WHERE g.grade = 'A';
```

#### Task 3/How many students are enrolled in each course?

```sql
SELECT c.course_name, COUNT(DISTINCT g.student_id) as num_students
FROM courses c
INNER JOIN grades g ON c.course_id = g.course_id
GROUP BY c.course_name;
```


#### Task 4/What is the average grade for each course?

```sql
SELECT c.course_name, AVG(g.grade) as avg_grade
FROM courses c
INNER JOIN grades g ON c.course_id = g.course_id
GROUP BY c.course_name;
```

#### Task 5/What is the name of the student who has the highest grade across all courses?

```sql
SELECT s.first_name, s.last_name
FROM students s
INNER JOIN grades g ON s.student_id = g.student_id
WHERE g.grade = (
  SELECT MAX(grade) FROM grades
);
```

#### Task 6/What is the name and email address of the student with the highest average grade?


```sql
SELECT students.first_name, students.last_name, students.email, AVG(grades.grade) AS avg_grade
FROM students
INNER JOIN grades ON students.student_id = grades.student_id
GROUP BY students.student_id, students.first_name, students.last_name, students.email
HAVING AVG(grades.grade) = (
    SELECT MAX(avg_grade)
    FROM (
        SELECT AVG(grade) AS avg_grade
        FROM grades
        GROUP BY student_id
    ) AS student_grades
);
```
</details>

## Case 2: Translated and original works

<details>
  <summary>Click to expand</summary>

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
</details>


## Case 3: Customers/orders

<details>
  <summary>Click to expand</summary>

 We have the two tables below :

- **`Customer`**

| customer_id  | age | location  | gender|
| :------------- | :------------- | :------------- |  :------------- | 
| 106  |34   |Ariena ||Male |
| 107  | 30   | Sfax  |  Male  | 
| 108  |42  | Sousse  |Female  |
| 110  |40  |Ariena ||Male |
| 140  |40  | Sousse  |Female  |
| 165  |52  |Ariena ||Male |
| 201  |37  | Monastir  |Female  |
 
 
 - **`Order`**
 
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
</details>

## Case 4: Zoo management

<details>
  <summary>Click to expand</summary>

We have 4 tables  related to zoo management and contain information about the animals, habitats, employees, and events in the zoo. The `Animals` table stores information about the species, name, gender, birth date, and habitat of each animal. The `Habitats` table contains information about the different habitats in the zoo. The `Employees` table stores information about the zoo staff, including their ID, first name, last name, and job title. Finally, the `Events` table tracks the events happening at the zoo, including the event ID, name, date, time, and the employee responsible for the event. 


- **` Animals Table`**

| animal_id | species     | name     | gender | birth_date | habitat_id |
|-----------|-------------|----------|--------|------------|------------|
| 1         | Lion        | Simba    | M      | 2010-05-15 | 1          |
| 2         | Tiger       | Rajah    | M      | 2012-08-22 | 2          |
| 3         | Elephant    | Dumbo    | M      | 2009-02-28 | 3          |
| 4         | Giraffe     | Melman   | M      | 2011-11-10 | 4          |
| 5         | Penguin     | Skipper  | F      | 2015-03-05 | 5          |
| 6         | Chimpanzee  | Caesar   | M      | 2014-01-01 | 6          |
| 7         | Gorilla     | King Kong| M      | 2007-06-18 | 7          |
| 8         | Flamingo    | Pinky    | F      | 2016-07-03 | 5          |
| 9         | Kangaroo    | Joey     | M      | 2013-04-20 | 8          |
| 10        | Crocodile   | Wally    | M      | 2010-09-12 | 9          |

- **` Habitats Table`**

| habitat_id | habitat_name        |
|------------|---------------------|
| 1          | Lion Habitat        |
| 2          | Tiger Habitat       |
| 3          | Elephant Habitat    |
| 4          | Giraffe Habitat     |
| 5          | Penguin Habitat     |
| 6          | Chimpanzee Habitat  |
| 7          | Gorilla Habitat     |
| 8          | Kangaroo Habitat    |
| 9          | Crocodile Habitat   |

- **` Employees Table`**

| employee_id | name             | job_title   |
|-------------|----------------|-------------|
| 1           | Ahmed Ben Salah | Zookeeper   |
| 2           | Amel Kefi       | Veterinarian|
| 3           | John Smith      | Zoologist   |
| 4           | Aya Nakamura    | Zookeeper   |
| 5           | Adam Johnson    | Zookeeper   |
| 6           | Kim Lee         | Veterinarian|
| 7           | Mohammed Ali    | Zoologist   |

- **` Events Table`**

| event_id | name                 | date       | time     | employee_id |
|----------|----------------------|------------|----------|--------------|
| 1        | Lion Feeding        | 2023-03-09 | 10:00:00 | 1            |
| 2        | Tiger Show          | 2023-03-10 | 14:00:00 | 3            |
| 3        | Elephant Bathing    | 2023-03-11 | 11:00:00 | 4            |
| 4        | Giraffe Feeding     | 2023-03-12 | 10:30:00 | 2            |
| 5        | Penguin March       | 2023-03-13 | 15:00:00 | 5            |
| 6        | Chimpanzee Exhibit  | 2023-03-14 | 12:00:00 | 7            |
| 7        | Gorilla Watching    | 2023-03-15 | 13:30:00 | 3            |
| 8        | Kangaroo Jumping    | 2023-03-16 | 11:00:00 | 1            |
| 9        | Crocodile Feeding   | 2023-03-17 | 10:30:00 | 5            |

#### Task 1/ What is the total number of animals in each habitat, ordered by the habitat name?
```sql
SELECT h.habitat_name, COUNT(*) as num_animals 
FROM Animals a
JOIN Habitats h ON a.habitat_id = h.habitat_id
GROUP BY h.habitat_name
ORDER BY h.habitat_name;
```
| habitat_name       | num_animals |
|--------------------|-------------|
| Chimpanzee Habitat | 1           |
| Crocodile Habitat  | 1           |
| Elephant Habitat   | 1           |
| Giraffe Habitat    | 1           |
| Gorilla Habitat    | 1           |
| Kangaroo Habitat   | 1           |
| Lion Habitat       | 1           |
| Penguin Habitat    | 2           |
| Tiger Habitat      | 1           |

#### Task 2/What is the number of events for each employee?
```sql
SELECT e.name AS employee_name, COUNT(*) AS event_count
FROM Events ev
JOIN Employees e ON ev.employee_id = e.employee_id
GROUP BY e.name;
```
| employee_name       | event_count |
|--------------------|-------------|
| Adam Johnson	 | 2           |
| Ahmed Ben Salah | 1           |
| Amel Kefi   | 1           |
| Aya Nakamura   | 2          |
| John Smith  | 1           |
| Kim Lee  | 1           |
| Mohammed Ali   | 2           |


#### Task 3/ What are the names and job titles of all the employees who have worked with elephants?
```sql
SELECT e.name, e.job_title 
FROM Employees e
JOIN Events ev ON e.employee_id = ev.employee_id
JOIN Animals a ON ev.animal_id = a.animal_id
WHERE a.species = 'Elephant';
```
| name             | job_title    |
|------------------|--------------|
| Ahmed Ben Salah  | Zookeeper    |
| John Smith       | Zoologist    |

#### Task 4/ What are the names of the animals and their habitats that will have an event on March 13, 2023?
```sql
SELECT a.name, h.habitat_name 
FROM Events ev
JOIN Animals a ON ev.animal_id = a.animal_id
JOIN Habitats h ON a.habitat_id = h.habitat_id
WHERE ev.date = '2023-03-13';
```
| name     | habitat_name  |
|----------|---------------|
| Skipper  | Penguin Habitat|

#### Task 5/ Who is the employee with the most events?
```sql
SELECT e.name, COUNT(*) as num_events 
FROM Events ev
JOIN Employees e ON ev.employee_id = e.employee_id
GROUP BY e.name
ORDER BY num_events DESC
LIMIT 1;
```

| name            | num_events |
|-----------------|-------------|
| Ahmed Ben Salah | 2           |

#### Task 6/ Which animals are in habitats managed by employees with the job title "Zookeeper"?
```sql
SELECT a.name, h.habitat_name, e.name as employee_name
FROM Animals a
JOIN Habitats h ON a.habitat_id = h.habitat_id
JOIN Employees e ON h.habitat_id = e.employee_id
WHERE e.job_title = 'Zookeeper';
```

| name     | habitat_name     | employee_name      |
|----------|-----------------|--------------------|
| Simba    | Lion Habitat     | Ahmed Ben Salah    |
| Rajah    | Tiger Habitat    | Adam Johnson       |
| Dumbo    | Elephant Habitat | Ahmed Ben Salah    |
| Melman   | Giraffe Habitat  | Adam Johnson       |
| Skipper  | Penguin Habitat  | Adam Johnson       |
| Joey     | Kangaroo Habitat | Ahmed Ben Salah    |
| Wally    | Crocodile Habitat| Ahmed Ben Salah    |


#### Task 7/ What is the total number of events organized by each employee, and how many of those events involved animals of a certain species? (Let's use the species "Penguin" as an example)
```sql
SELECT e.name, COUNT(DISTINCT ev.event_id) AS total_events, 
       COUNT(DISTINCT CASE WHEN a.species = 'Penguin' THEN ev.event_id ELSE NULL END) AS penguin_events
FROM Employees e
JOIN Events ev ON e.employee_id = ev.employee_id
LEFT JOIN Event_Animals ea ON ev.event_id = ea.event_id
LEFT JOIN Animals a ON ea.animal_id = a.animal_id
GROUP BY e.name;
```
| name             | total_events | penguin_events |
|------------------|--------------|-----------------|
| Ahmed Ben Salah  | 4            | 0               |
| Amel Kefi        | 0            | 0               |
| John Smith       | 0            | 0               |
| Aya Nakamura     | 0            | 0               |
| Adam Johnson     | 2            | 1               |
| Kim Lee          | 0            | 0               |
| Mohammed Ali     | 0            | 0               |

</details>

## Case 5: Customers / companies

<details>
  <summary>Click to expand</summary>

We have three tables :  The `Customers` table contains customer information such as name, email, and phone number, and is linked to the `Companies` table which contains company information such as name and industry. The `Deals` table tracks deals and their associated customers and companies, deal stage, value, and creation date. These tables provide a complete view of the customers and companies a business is interacting with and the deals they are pursuing.

- **` Customer`**

|Customer ID|	Name|	Email|	Phone|
|------------------|--------------|----------|-------|
|1|	Fatma Ben Ali|	fatma.ben.ali@hotmail.com|	+216 20 000 000|
|2|	Amira Khelifi|	amira.khelifi@hotmail.com|	+216 71 000 000|
|3|	Mohamed Ben Salah|	mohamed.ben.salah@hotmail.com|	+216 98 000 000|
|4|	Ahmed Bouazizi|	ahmed.bouazizi@hotmail.com|	+216 50 000 000|
|5|	Salma Ben Ammar|	salma.ben.ammar@hotmail.com|	+216 27 000 000|

- **` Companies`**

|Company ID	|Name|	Industry|
|-----------|--- |--------|
|1|Groupe Chimique Tunisien|	Chemicals|
|2|Tunisie Télécom|	Telecommunications|
|3|STEG	|Utilities|
|4|BIAT	|Banking|

- **` Deals`**

|Deal ID|	Customer ID|	Company ID|	Deal Stage|	Value|	Creation Date|
|------|--------------|----------|------------------|--------------|----------|
|1	|1	|1	|Negotiation	|10000 TND	|2022-01-01|
|2	|2|	2	|Closed	|25000 TND	|2022-02-15|
|3	|3	|3	|Proposal	|50000 TND	|2022-03-20|
|4|	4|	1	|Qualified	|15000 TND	|2022-04-12|
|5	|5|	4	|Negotiation	|30000 TND	|2022-05-10|



#### Task 1/Which customers have deals worth more than 20000 TND?

```sql
SELECT c.Name
FROM Customers c
INNER JOIN Deals d ON c.`Customer ID` = d.`Customer ID`
WHERE d.Value > 20000;
```
| Name             |
|------------------|
| Amira Khelifi    |
| Mohamed Ben Salah |
| Salma Ben Ammar  |

#### Task 2/How many deals does each company have in each stage?
```sql
SELECT c.Name, d.`Deal Stage`, COUNT(*)
FROM Companies c
INNER JOIN Deals d ON c.`Company ID` = d.`Company ID`
GROUP BY c.Name, d.`Deal Stage`;

```
| Name                  | Deal Stage   | COUNT(*) |
|-----------------------|--------------|----------|
| BIAT                  | Qualified   | 1        |
| Groupe Chimique Tunisien | Negotiation | 2        |
| Groupe Chimique Tunisien | Qualified   | 1        |
| STEG                  | Proposal    | 1        |
| Tunisie Télécom          | Closed      | 1        |

#### Task 3/Which customers have deals with more than one company?
```sql
SELECT c1.Name
FROM Customers c1
INNER JOIN (
    SELECT `Customer ID`, COUNT(DISTINCT `Company ID`) AS num_companies
    FROM Deals
    GROUP BY `Customer ID`
) c2 ON c1.`Customer ID` = c2.`Customer ID`
WHERE c2.num_companies > 1;
```

| Name             |
|------------------|
| Fatma Ben Ali    |
| Salma Ben Ammar  |

#### Task 4/What is the average deal value for each industry?
```sql
SELECT c.Industry, AVG(d.Value)
FROM Companies c
INNER JOIN Deals d ON c.`Company ID` = d.`Company ID`
GROUP BY c.Industry;
```
| Industry         | AVG(d.Value) |
|------------------|---------------|
| Banking          | 15000.00 TND |
| Chemicals        | 12500.00 TND |
| Telecommunications | 25000.00 TND |
| Utilities        | 50000.00 TND |

#### Task 5/Which customers have not been associated with any deals?
```sql
SELECT c.Name
FROM Customers c
LEFT JOIN Deals d ON c.`Customer ID` = d.`Customer ID`
WHERE d.`Deal ID` IS NULL;
```

| Name        |
|-------------|
| Ali Ben Ali |

#### Task 6/What is the total value of deals for each company in the "Proposal" stage?
```sql
SELECT c.Name, SUM(d.Value)
FROM Companies c
INNER JOIN Deals d ON c.`Company ID` = d.`Company ID`
WHERE d.`Deal Stage` = 'Proposal'
GROUP BY c.Name;
```

| Name                  | SUM(d.Value) |
|-----------------------|---------------|
| Groupe Chimique Tunisien | 5000.00 TND  |
| STEG                  | 10000.00 TND |

</details>

## Case 6: Car dealer management

<details>
  <summary>Click to expand</summary>
  
Working for a manager at a car dealership , looking to improve the company's sales and customer satisfaction. To achieve this, we need to analyze the data related to car sales and customer interactions. We have access to four tables that contain the necessary information for the analysis: `customers`, `cars`, `sales`, and `salespeople`. 
The `customers` table includes details about the customers who have purchased cars from the dealership, while the `cars` table provides information about the cars available for sale. The `sales` table records the details of each car sale, including the customer who purchased the car and the sale amount. Lastly, the `salespeople` table contains information about the salespeople who work at the dealership.

- **` Customer`**

|customer_id|	name|	address|	phone|	email|
|-----------|-----|--------|------|----------|
|1|	Ahmed Ben Ali|	123 Rue de la Paix, Tunis|	+216 71 123 456|	ahmed.benali@gmail.com|
|2|	Fatima Khaldi|	456 Avenue Habib Bourguiba, Sfax|	+216 74 987 654|	fatima.khaldi@yahoo.com|
|3|	Mohamed Belhaj|	789 Rue du Lac, Tunis|	+216 71 555 444|	mohamed.belhaj@outlook.tn|
|4|	Amina Chaker|	987 Boulevard du 7 Novembre, Sousse|	+216 73 111 222|	amina.chaker@hotmail.com|
|5|	Mehdi Ben Youssef|	111 Rue de l'Indépendance, Tunis|	+216 71 777 888|	mehdi.byoussef@gmail.com|

- **` Cars`**

|car_id|	make|	model|	year|	color|	price|
|------|-----|-----|--------|------|----------|
|1|	Renault	|Clio	|2019	|Blue	|12000|
|2|	Volkswagen|	Golf|	2018|	Silver|	15000|
|3|	Peugeot	|208	|2020	|Red	|14000|
|4|	Toyota	|Yaris |Hybrid	|2019|	White|	17000|
|5|	Honda|	Civic	|2021|	Black	|25000|

- **` Sales`**

|sale_id|	customer_id|	car_id	|sale_date|	sale_amount|
|------|----------|--------|------|----------|
|1|	1|	1|	2022-01-05|	10000|
|2|	2	|3	|2022-02-12	|12000|
|3|	3	|4	|2022-03-07	|16000|
|4|	4	|5	|2022-04-23	|22000|
|5|	5	|2	|2022-05-15	|14000|

- **` Salespeople`**


|salesperson_id|	name	phone|	email|
|--------------|----------|--------|
|1|	Nizar Ben Salah	|+216 71 555 333	|nizar.bensalah@outlook.tn|
|2|	Rim Bouhlel	|+216 71 444 555	|rim.bouhlel@gmail.com|
|3|	Anis Jomaa	|+216 71 222 333	|anis.jomaa@gmail.com|


#### Task 1/What is the total revenue generated from car sales in 2022?

```sql
SELECT SUM(sale_amount) as total_revenue 
FROM sales 
WHERE sale_date >= '2022-01-01' AND sale_date < '2023-01-01';
```
|total_revenue|
|---------------|
|       98100.0|

#### Task 2/What is the average price of cars sold in Sfax?

```sql
SELECT AVG(price) as avg_price
FROM cars
INNER JOIN sales
ON cars.car_id = sales.car_id
INNER JOIN customers
ON sales.customer_id = customers.customer_id
WHERE customers.address LIKE '%Sfax%';
```
|   avg_price |    
| ------------------| 
|  22509.375| 

#### Task 3/Who is the top-performing salesperson in terms of revenue generated in 2022?

```sql
SELECT salespeople.name as salesperson, SUM(sales.sale_amount) as revenue
FROM salespeople
INNER JOIN sales
ON salespeople.salesperson_id = sales.salesperson_id
WHERE sales.sale_date >= '2022-01-01' AND sales.sale_date < '2023-01-01'
GROUP BY salespeople.name
ORDER BY revenue DESC
LIMIT 1;
```
|salesperson | revenue| 
|-------------|---------|
 |Rim Bouhlel  | 56800.0|

#### Task 4/What is the average price of cars sold to customers with Gmail accounts?

```sql
SELECT AVG(cars.price) as avg_price
FROM cars
INNER JOIN sales
ON cars.car_id = sales.car_id
INNER JOIN customers
ON sales.customer_id = customers.customer_id
WHERE customers.email LIKE '%gmail.com%';
```

|     avg_price  |    
|--------------------|
| 18544.45|


#### Task 5/Which customer(s) has/have purchased the most expensive car(s)?

```sql
SELECT customers.name, cars.make, cars.model, cars.price
FROM customers
INNER JOIN sales
ON customers.customer_id = sales.customer_id
INNER JOIN cars
ON sales.car_id = cars.car_id
WHERE cars.price = (SELECT MAX(price) FROM cars)
```

|      name       |  make  |     model     | price  |
|-----------------|--------|---------------|--------|
| Mohamed Ben Ali | Toyota | Land Cruiser | 40000.0|

</details>

## Case 7 :  Pharmacy
<details>
  <summary>Click to expand</summary>
  
  The pharmacy database consists of four tables: `Patients`, `Prescriptions`, `Medications`, and `Doctors`. The `Patients` table stores information about patients such as their name, address, and date of birth. The `Prescriptions` table contains information about the medications prescribed to each patient including dosage, frequency, and start/end dates. The `Medications` table contains information about the medications available at the pharmacy including name, description, and price. Finally, the `Doctors` table stores information about doctors who prescribe medications, including their name, specialty, and contact information. Together, these tables form the basis of the pharmacy's data management system, which enables the pharmacy to keep track of patient information, medications prescribed, and doctors who prescribe them.
  
  - **` Patients`**
  
  |patient_id|	name|	address|	date_of_birth|
  |-----------|-----|--------|---------------|
|1	|Mohamed Ben Ali	|123 Main St, Tunis	|1990-05-10|
|2	|Fatima Haddad	|456 Elm St, Sfax	|1985-12-25|
|3	|Karim Zouari	|789 Maple Ave, Tunis	|1995-07-01|
|4	|Amira Khemiri	|456 Pine St, Sousse	|1980-03-15|
  
  - **` Prescriptions`**
  
|prescription_id|	patient_id|	medication_id|	dosage|	frequency|	start_date|	end_date|
|-----------|-----------------|------------|--------|-------|------------|-------|
|1	|1	|1	|50mg|	Once a day|	2022-02-01|	2022-03-01|
|2	|2	|2	|100mg	|Twice a day	|2022-02-15	|2022-03-15|
|3	|3	|3	|25mg|	Three times a day|	2022-02-10|	2022-03-10|
|4	|4	|4	|75mg	|Once a day	|2022-02-20	|2022-03-20|
  
  - **` Medications`**
  
|medication_id|	name|	description|	price|
|--------|-------|------------|-------|
|1|	Ibuprofen	|A nonsteroidal anti-inflammatory drug	|10|
|2|	Amoxicillin|	An antibiotic used to treat bacterialinfections	|15|
|3|	Prozac	|An antidepressant	|20|
|4|	Lipitor	|A medication used to lower cholesterol levels	|25|
  
  - **` Doctors`**
  
|doctor_id|	name|	specialty|	contact_number|
|--------|-------|------------|-------|
|1|	Dr. Ali Ben Salem	|Cardiologist	|555-1234|
|2|	Dr. Omar Chakroun	|Endocrinologist	|555-5678|
|3|	Dr. Amina Bouzidi	|Neurologist	|555-9012|
|4|	Dr. Ahmed Jomaa	|Oncologist	|555-3456|
  
#### Task 1/What are the details of all patients with a prescription that ends before March 15th, 2022?
```sql
SELECT p.*
FROM Patients p
JOIN Prescriptions r ON p.patient_id = r.patient_id
WHERE r.end_date < '2022-03-15';
```

|      patient_id       |  name  |     address     | date_of_birth  |
|-----------------|--------|---------------|--------|
| 1 |  Mohamed Ben Ali  | 123 Main St, Tunis| 1990-05-10|
| 2  |  Fatima Haddad  | 456 Elm St, Sfax | 1985-12-25|  
  
  
#### Task 2/What is the total price of all medications prescribed to each patient?

```sql
SELECT Patients.patient_id, Patients.name, SUM(Medications.price) AS total_price
FROM Patients
JOIN Prescriptions ON Patients.patient_id = Prescriptions.patient_id
JOIN Medications ON Prescriptions.medication_id = Medications.medication_id
GROUP BY Patients.patient_id;
```

|patient_id | name| total_price|
|-----------|-----|-----------|
|1| Mohamed Ben Ali |10|
|2| Fatima Haddad   |15|
|3| Karim Zouari    |20|
|4| Amira Khemiri   |25|
                    
#### Task 3/Which medications have been prescribed to patients by more than one doctor?

```sql
SELECT m.name AS medication_name, COUNT(DISTINCT p.doctor_id) AS num_doctors_prescribed
FROM medications m
INNER JOIN prescriptions pr ON m.medication_id = pr.medication_id
INNER JOIN patients p ON pr.patient_id = p.patient_id
GROUP BY m.name
HAVING COUNT(DISTINCT p.doctor_id) > 1;
```

|medication_name  | num_doctors_prescribed|
|----------------|-----------------------|
|Ibuprofen        | 2|             
  

#### Task 4/What is the average number of prescriptions per patient?


```sql
SELECT AVG(num_prescriptions) AS avg_prescriptions_per_patient
FROM (
  SELECT patient_id, COUNT(*) AS num_prescriptions
  FROM prescriptions
  GROUP BY patient_id
) subquery;
```
|avg_prescriptions_per_patient|
|------------------------------|
|                          1.0|

#### Task 5/Which doctor has prescribed the most medications?


```sql
SELECT d.name AS doctor_name, COUNT(*) AS num_prescriptions
FROM doctors d
INNER JOIN prescriptions p ON d.doctor_id = p.doctor_id
GROUP BY d.name
ORDER BY num_prescriptions DESC
LIMIT 1;


```

|doctor_name      | num_prescriptions|
|-----------------|------------------|
|Dr. Omar Chakroun |                 2|
  
#### Task 6/What is the total price of all medications prescribed by each doctor?


```sql
SELECT d.name AS doctor_name, SUM(m.price) AS total_price
FROM doctors d
JOIN prescriptions p ON d.doctor_id = p.doctor_id
JOIN medications m ON p.medication_id = m.medication_id
GROUP BY d.doctor_id;
```

|doctor_name           | total_price|
|----------------------|-------------|
|Dr. Ali Ben Salem     | 10|
|Dr. Omar Chakroun     | 55|
|Dr. Amina Bouzidi     | 20|
|Dr. Ahmed Jomaa       | 75|

#### Task 7/What is the average price of medications prescribed by each doctor for patients older than 30 years old??
  
```sql
SELECT d.name AS doctor_name, AVG(m.price) AS avg_price
FROM doctors d
JOIN prescriptions p ON d.doctor_id = p.doctor_id
JOIN medications m ON p.medication_id = m.medication_id
JOIN patients pt ON p.patient_id = pt.patient_id
WHERE pt.date_of_birth < '1992-03-10'
GROUP BY d.doctor_id;
```
|doctor_name           | avg_price|
|----------------------|-----------|
|Dr. Omar Chakroun     | 15|
|Dr. Amina Bouzidi     | 20|
|Dr. Ahmed Jomaa       | 25|

                    
  </details>


## Case 8: TV shows

<details>
  <summary>Click to expand</summary>
  
  TV shows have become an integral part of our lives.They have a massive following and viewership worldwide.In this case,we have four tables:`TV_Shows`, `Episodes`, Cast, and Ratings. `TV_Shows` table contains information about all the TV shows including the title, genre, year, and rating. `Episodes` table contains details of all the episodes of TV shows such as the title, season, episode number, and the ID of the TV show it belongs to. `Cast` table includes the details of all the actors in TV shows including their name, gender, age, and nationality. `Ratings` table stores the ratings given by users to TV shows with the ID of the TV show and the rating provided.
  
  - **` TV_Shows`**
  
|id | title | genre | year | rating|
|---|-------|-------|------|-------|
|1 | Breaking Bad | Drama | 2008 | 9.5|
|2 | Game of Thrones | Fantasy | 2011 | 9.3|
|3 | Friends | Comedy | 1994 | 8.9|
|4 | The Office | Comedy | 2005 | 8.9|
|5 | Stranger Things | Sci-Fi/Horror | 2016 | 8.7|
|6 | The Crown | Drama | 2016 | 8.7|
|7 | The Sopranos | Drama | 1999 | 9.2|
|8 | Narcos | Crime/Drama | 2015 | 8.8|
|9 | The Big Bang Theory| Comedy | 2007 | 8.1|
|10 | Modern Family | Comedy | 2009 | 8.4|
  
  - **` Episodes`**
  
|id | title | season | episode_no | tv_show_id|
|---|------|---------|------------|-----------|
|1 | Pilot | 1 | 1 | 1|
|2 | Cat's in the Bag | 1 | 2 | 1|
|3 | ...And the Bag's in the River | 1 | 3 | 1|
|4 | Cancer Man | 1 | 4 | 1|
|5 | Gray Matter | 1 | 5 | 1|
|6 | The Pointy End | 1 | 8 | 2|
|7 | Fire and Blood | 1 | 10 | 2|
|8 | Winter Is Coming | 1 | 1 | 2|
|9 | The Kingsroad | 1 | 2 | 2|
|10 | Cripples, Bastards, and Broken Things | 1 | 4 | 2|
  
  - **` Cast`**
  
|id | name | gender | age | nationality|
|----|-----|--------|-----|------------|
|1 | Bryan Cranston | M | 65 | American|
|2 | Aaron Paul | M | 42 | American|
|3 | Anna Gunn | F | 53 | American|
|4 | Emilia Clarke | F | 35 | British|
|5 | Kit Harington | M | 35 | British|
|6 | Sophie Turner | F | 25 | British|
|7 | Jennifer Aniston | F | 53 | American|
|8 | Courteney Cox | F | 57 | American|
|9 | Lisa Kudrow | F | 58 | American|
|10 | Matt LeBlanc | M | 54 | American|
  
  - **` Ratings`**
  
|id | tv_show_id | rating|
|---|------------|-------|
|1 | 1 | 9.5|
|2 | 2 | 9.3|
|3 | 3 | 8.9|
|4 | 4 | 8.9|
|5 | 5 | 8.7|
|6 | 6 | 8.7|
|7 | 7 | 9.2|
|8 | 8 | 8.8|
|9 | 9 | 8.1|
|10 | 10 | 8.4|

#### Task 1/What are the top 5 TV shows with the highest rating?

```sql
SELECT title, rating
FROM TV_Shows
ORDER BY rating DESC
LIMIT 5;
```
|title|	rating|
|-----|--------|
|Breaking Bad	|9.5|
|Game of Thrones	|9.3|
|Friends	|8.9|
|The Office	|8.9|
|Stranger Things	|8.7|

#### Task 2/Who are the top 5 actors who appeared in the most TV shows?

```sql
SELECT name, COUNT(*) AS appearances
FROM Cast
GROUP BY name
ORDER BY appearances DESC
LIMIT 5;
```
|name	|appearances|
|------|-----------|
|Jennifer Aniston	|3|
|Courteney Cox	|3|
|Lisa Kudrow	|3|
|Matt LeBlanc	|3|
|Bryan Cranston	|1|

#### Task 3/What is the average rating of TV shows released before the year 2000?

```sql
SELECT AVG(rating) AS avg_rating
FROM TV_Shows
WHERE year < 2000;
```
|avg_rating|
|---------|
|8.75|

#### Task 4/What are the titles of the episodes with the highest rating for each TV show?

```sql
SELECT t.title, e.title AS episode_title, e.rating
FROM TV_Shows t
JOIN Episodes e ON t.id = e.tv_show_id
WHERE e.rating = (SELECT MAX(rating) FROM Episodes WHERE tv_show_id = t.id);
```
|title	|episode_title	|rating|
|------|-------------|--------|
|Breaking Bad|	Felina|	9.9|
|Game of Thrones|	The Winds of Winter|	9.9|
|Friends|	The One with the Embryos	|9.5|
|The Office|	Goodbye, Michael	|9.8|
|Stranger Things|	Chapter Eight: The Upside Down|	9.5|

#### Task 5/What is the total number of episodes for each TV show?

```sql
SELECT t.title, COUNT(*) AS total_episodes
FROM TV_Shows t
JOIN Episodes e ON t.id = e.tv_show_id
GROUP BY t.title;
```
|title	|total_episodes|
|--------|------------|
|Breaking Bad|	62|
|Game of Thrones	|73|
|Friends|	236|
|The Office	|201|
|Stranger Things	|25|

#### Task 6/What is the total number of episodes for each TV show?

```sql
SELECT t.title, e.title AS episode_title, e.rating
FROM TV_Shows t
JOIN Episodes e ON t.id = e.tv_show_id
WHERE e.rating > (SELECT AVG(rating) FROM Episodes) 
ORDER BY t.title, e.rating DESC;
```
|title|	episode_title|	rating|
|-----|--------------|--------|
|Breaking Bad|	Felina	|9.9|
|Friends|	The One Where Everybody Finds Out|	9.8|
|Friends|	The One with the Embryos	|9.5|
|Game of Thrones|	The Winds of Winter	|9.9|
|The Office|	Goodbye, Michael	|9.8|
|The Office|	Stress Relief	|9.7|

#### Task 7/What is the total number of episodes for each TV show?

```sql
SELECT name, COUNT(*) AS comedy_shows
FROM Cast c
JOIN TV_Shows t ON c.tv_show_id = t.id
WHERE t.genre = "Comedy"
GROUP BY name
HAVING comedy_shows >= 2;
```
|name	|comedy_shows|
|-----|-------------|
|Courteney Cox	|2|
|Jennifer Aniston	|2|
|Lisa Kudrow	|2|
|Matt LeBlanc	|2|

</details>
