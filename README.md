# SQL-Portfolio
#### Data analysis tasks made using SQL 
#### This different data analysis case study resolved using SQL language 

## Case 1:

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

## Case 2:
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
## Case 3:
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

## Case 4:

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

## Case 5:

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
