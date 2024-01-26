# Database Project for **Bookstore** #

The scope of this project is to use all the SQL knowledge gained throught the Software Testing course and apply them in practice.

Application under test: **Bookstore**

Tools used: MySQL Workbench

Database description: **This database has information about the books that you can find in a bookstore, and also information about the authors.**


**1. Database Schema**

You can find below the database schema that was generated through Reverse Engineer and which contains all the tables and the relationships between them.

The tables are connected in the following way:


   - **authors**  is connected with **books** through a **one-to-many** relationship which was implemented through **authors.authorid** as a primary key and **books.authorid** as a foreign key
  

**2. Database Queries**

## DDL (Data Definition Language)

  - The following instructions were written in the scope of CREATING the structure of the database (CREATE INSTRUCTIONS)

create database bookstore;

```sql
create table books (
    title varchar(25) not null,
    author varchar(25) not null
);
```

```sql
create table authors (
    authorid int not null auto_increment,
    fullname varchar (100) not null,
    dateofbirth date not null,
    primary key (authorid)
);
```

  - After the database and the tables have been created, a few ALTER instructions were written in order to update the structure of the database, as described below:

```sql
/* Add new column to the books table */
alter table books
add column releasedate date;

/* Remove the newly added column */
alter table books 
drop releasedate;

alter table books 
modify title char(25) not null;

alter table books 
modify author char(25);

alter table books 
change releasedate releaseyear date;

alter table books 
rename to carti;

alter table books
add column pageNumber int;

/* Add primary key to the books table */
alter table books
add column bookid int primary key auto_increment;

/* Add new column to hold the foreign key for the authorid */
alter table books
add column authorid int not null;
```

### DML (Data Manipulation Language)

- In order to be able to use the database I populated the tables with various data necessary in order to perform queries and manipulate the data. 
- In the testing process, this necessary data is identified in the Test Design phase and created in the Test Implementation phase. 

- Below you can find all the insert instructions that were created in the scope of this project:

```sql
insert into books (title, author, releasedate)
values ('The Shining' , 'Stephen King' , '1977-01-28');

insert into books (title, author, releasedate)
values ('Atonement' , 'Ian McEwan' , '2001-04-15') , ('Shadow and Bone' , 'Leigh Bardugo' , '2012-06-05');

insert into books 
values ('Maitreyi' , 'Mircea Eliade' , '1933-05-18');

insert into books 
values ('The Housemade' , 'Freida McFadden' , '2022-08-14' , '258');

insert into books 
values ('The Housemades Secret' , 'Freida McFadden' , '2023-02-21' , '210');

insert into authors (fullname, dateofbirth)
values ('Stephen King' , '1947-09-21');

insert into authors (fullname, dateofbirth)
values ('Ian McEwan' , '1948-06-21');

insert into authors (fullname, dateofbirth)
values ('Leigh Bardugo' , '1975-04-06');

insert into authors (fullname, dateofbirth)
values ('Mircea Eliade' , '1907-03-13');

insert into authors (fullname, dateofbirth)
values ('Freida McFadden' , '1985-01-05');

insert into authors (fullname, dateofbirth)
values ('George R R Martin' , '1948-09-20');

insert into authors (fullname, dateofbirth)
values ('R F Kuang' , '1996-05-29');

insert into authors (fullname, dateofbirth)
values ('Marin Preda' , '1922-08-05');
```

*This could have also been written by seting all the values one after another*

- After the insert, in order to prepare the data to be better suited for the testing process, I updated some data in the following way:

*Update "authorid" column with valid data to be able to add a foreign key constraint*

```sql
update books set authorid = 1 where bookid = 1;

update books set authorid = 3 where bookid = 2;

update books set authorid = 4 where bookid = 3;

update books set authorid = 5 where bookid = 4;

update books set authorid = 6 where bookid = 5;

update books set authorid = 6 where bookid = 6;

update books set authorid = 7 where bookid = 7;

update books set authorid = 8 where bookid = 8;

update books set authorid = 1 where bookid = 9; 
```

- After updating the books authorid column with valid data we can add the foreign key constraint

```sql
alter table books
add foreign key (authorid) references authors(authorid);
```

### DQL (Data Query Language)

- In order to simulate various scenarios that might happen in real life I created the following queries that would cover multiple potential real-life situations:

```sql
/* Show all the rows from the books table */
select *
from books;

/* Show all the titles for all the books */
select title
from books;
```

**WHERE**

```sql
/* Show all the books that have more than 200 pages */
select *
from books
where pageNumber > 200; 

/* Show all the books that have 210 pages */
select *
from books
where pageNumber = 210;

/* Show all the books that have less than 300 pages */
select *
from books
where pageNumber < 300; 

/* Show all the books that have under 210 pages and the ones that have 210 pages */
select *
from books
where pageNumber <= 210; 

/* Show all the books that have 210 pages and the ones with more than 210 pages */
select *
from books
where pageNumber >= 210;

/* Show all the books that were relesed after 2000 */
select *
from books
where releasedate > '2000-12-31'; 

/* Show just the books that are titled "The Shining" and "Maitreyi" */
select *
from books
where title in ('The Shining' , 'Maitreyi');

/* Show all the books except "The Shining" and "Maitreyi" */
select *
from books
where title not in ('The Shining' , 'Maitreyi');

/* Show all the books except the ones that have the author "Freida McFadden" */
select *
from books
where author != 'Freida McFadden'; 

/* Show all the books that have a page count between 190 and 300 */
select *
from books
where pageNumber between 190 and 300;
```

**AND**

```sql
/* Show all the books that have the title that starts with "The" and the release date starts with "2" */
select * from books
where title like 'The%'
and releasedate like '2%';
```

**OR**

```sql
/* Show all the books that have the title that starts with "The" or the release date starts with "2" */
select * from books
where title like 'The%'
or releasedate like '2%';

/* Show all the books that have the title that starts with "The" or the release date starts with "2" and author starts with "Mc" */
select * from books
where title like 'The%'
or releasedate like '2%'
and author like '%Mc%';

/* Show all the books that have the title that starts with "The" or the release date starts with "2" and author starts with "Mc" */
select * from books
where (title like 'The%'
or releasedate like '2%')
and author like '%Mc%';
```

**NOT**

```sql
/* Show all the books that have page number */
select *
from books
where pageNumber is not null;
```

**LIKE**

```sql
/* Show all the books with the author's name starting with "St" */
select *
from books
where author like 'St%';

/* Show  all the books with the author's name ending with "en" */
select *
from books
where author like '%en';

/* Show  all the books with the author's name contains the letter "e" */
select *
from books
where author like '%e%';

/* Show  all the books with the release date starts with "20" */
select *
from books
where releasedate like '20%';

/* Show  all the books with the release date contains "05" */
select *
from books
where releasedate like '%05%';

/* Show  all the books with the release date contains "-05-" */
select *
from books
where releasedate like '%-05-%';
```

**INNER JOIN**

```sql
select *
from books
inner join authors
on books.authorid = authors.authorid;

select books.title, authors.fullname, books.releasedate, books.pageNumber 
from books 
inner join authors 
on books.authorid = authors.authorid;
```

**LEFT JOIN**

```sql
/* Show the authors that don't have a coresponding book in the books table */
select bookid, books.title, authors.fullname
from authors 
left join books 
on books.authorid = authors.authorid
where books.title is null; 
```


**RIGHT JOIN**

```sql
select books.title, authors.fullname, books.releasedate, books.pageNumber 
from books 
right join authors 
on books.authorid = authors.authorid;
```

**FUNCTII AGREGATE**

```sql
/* Show the sum of all page numbers */
select sum(pageNumber)
from books;

/* Show the average of page number */
select avg(pageNumber)
from books;

/* Show the minimum page number */
select min(pageNumber)
from books;

/* Show the maximum page number */
select max(pageNumber)
from books;

/* Show the sum of page numbers from books that have more then 200 pages */
select sum(pageNumber)
from books where pageNumber > 200;

/* Show how many books don't have the pageNumber */
select count(*)
from books
where pageNumber is null;

/* Show how many books have more than 200 pages */
select count(title)
from books
where pageNumber > 200; 

/* Show the avarage page number for books with authors starting with "Mc" */
select avg(pageNumber)
from books
where author like '%Mc%';
```

**ORDER BY**

```sql
/* Order by bookid descending */
select *
from books
order by bookid desc;

/* Order by bookid ascending */
select *
from books
order by bookid asc;

/* Order by authpor ascending */
select *
from books 
order by author asc;
```

**LIMIT**

```sql
/* Limit result to first 2 rows */
select *
from books
limit 2;

/* Limit result to first 1 row */
select *
from books
where title like 'The%'
limit 1;
```

### Conclusions

**I have learned how a bookstore database will work, and what informations I could obtain from it. I have learned how to create a database model using the tools of MySQL Workbench. I learned how to define tables, columns, and how to create one-to-many relationship between tables.**

