# SQL-Portfolio
Several data analysis tasks made using SQL 
This different data analysis case study resolved using SQL language 

For the examples, we will use information about a publishing house that publishes original and translated books. Our database contains four tables: books, authors, editors, and translators.

1/Query book titles along with their authors (i.e., the author’s first name and last name):
```sql
  SELECT b.id, b.title, a.first_name, a.last_name
  FROM books b
  INNER JOIN authors a
  ON b.author_id = a.id
  ORDER BY b.id;
```

2/ Query books along with their translators (i.e., the translator’s last name). Only half of  books have been translated and thus have a corresponding translator:

  SELECT b.id, b.title, b.type, t.last_name AS translator
  FROM books b
  JOIN translators t
  ON b.translator_id = t.id
  ORDER BY b.id;
 
3/ Query information about each book’s author and translator (i.e., their last names). We also want to keep the basic information about each book (i.e., id, title, and type).

  SELECT b.id, b.title, b.type, a.last_name AS author,
  t.last_name AS translator
  FROM books b
  LEFT JOIN authors a
  ON b.author_id = a.id
  LEFT JOIN translators t
  ON b.translator_id = t.id
  ORDER BY b.id;
  
4/Query the basic book information (i.e., ID and title) along with the last names of the corresponding editors. Again, we want to keep all of the books in the result set.

  SELECT b.id, b.title, e.last_name AS editor
  FROM books b
  LEFT JOIN editors e
  ON b.editor_id = e.id
  ORDER BY b.id;
