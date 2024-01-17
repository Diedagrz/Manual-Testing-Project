<h1>Database Project for **Bookstore**</h1>

The scope of this project is to use all the SQL knowledge gained throught the Software Testing course and apply them in practice.

Application under test: **Bookstore**

Tools used: MySQL Workbench

Database description: **This database has information about the books that you can find in a bookstore, and also information about the authors.**

<ol>
<li>Database Schema </li>
<br>
You can find below the database schema that was generated through Reverse Engineer and which contains all the tables and the relationships between them.

The tables are connected in the following way:

<ul>
  <li> **authors**  is connected with **books** through a **one-to-many** relationship which was implemented through **authors.authorid** as a primary key and **books.authorid** as a foreign key</li>
  

<li>Database Queries</li><br>

<ol type="a">
  <li>DDL (Data Definition Language)</li>

  The following instructions were written in the scope of CREATING the structure of the database (CREATE INSTRUCTIONS)

create database bookstore;

CREATE TABLE books (
    title VARCHAR(25) NOT NULL,
    author VARCHAR(25) NOT NULL
);

create table authors (
authorid int not null auto_increment,
fullname varchar (100) not null,
dateofbirth date not null,
primary key (authorid));


  After the database and the tables have been created, a few ALTER instructions were written in order to update the structure of the database, as described below:

alter table books
add column releasedate date;

alter table books 
drop releasedate;

alter table books 
modify title char (25) not null;
alter table books 
modify author char (25);

alter table books 
change releasedate releaseyear date;

alter table books 
rename to carti;

alter table books
add column pageNumber int;

alter table books add column bookid int primary key auto_increment;
alter table books add foreign key (authorid) references authors(authorid);

  
  <li>DML (Data Manipulation Language)</li>

  In order to be able to use the database I populated the tables with various data necessary in order to perform queries and manipulate the data. 
  In the testing process, this necessary data is identified in the Test Design phase and created in the Test Implementation phase. 

  Below you can find all the insert instructions that were created in the scope of this project:

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

insert into authors (fullname, dateofbirth) values ('Stephen King' , '1947-09-21');
insert into authors (fullname, dateofbirth) values ('Ian McEwan' , '1948-06-21');
insert into authors (fullname, dateofbirth) values ('Leigh Bardugo' , '1975-04-06');
insert into authors (fullname, dateofbirth) values ('Mircea Eliade' , '1907-03-13');
insert into authors (fullname, dateofbirth) values ('Freida McFadden' , '1985-01-05');
insert into authors (fullname, dateofbirth) values ('George R R Martin' , '1948-09-20');
insert into authors (fullname, dateofbirth) values ('R F Kuang' , '1996-05-29');
insert into authors (fullname, dateofbirth) values ('Marin Preda' , '1922-08-05');

/* this could also be written by seting all the values one after another*/


  After the insert, in order to prepare the data to be better suited for the testing process, I updated some data in the following way:


update books set pageNumber = '160' 
where title = 'Shadow and Bone';

/* update "authorid" column with valid data to be able to add a foreign keys constraint */

update books set authorid = 1 where bookid = 1;
update books set authorid = 3 where bookid = 2;
update books set authorid = 4 where bookid = 3;
update books set authorid = 5 where bookid = 4;
update books set authorid = 6 where bookid = 5;
update books set authorid = 6 where bookid = 6;
update books set authorid = 7 where bookid = 7;
update books set authorid = 8 where bookid = 8;
update books set authorid = 1 where bookid = 9; 


  <li>DQL (Data Query Language)</li>

After the testing process, I deleted the data that was no longer relevant in order to preserve the database clean: 


In order to simulate various scenarios that might happen in real life I created the following queries that would cover multiple potential real-life situations:


select * from books;
select title from books;

**- where**<br>

select * from books where pageNumber > 200;
select * from books where pageNumber = 210;
select * from books where pageNumber < 300;
select * from books where pageNumber <= 210;
select * from books where pageNumber >= 210;
select * from books where releasedate > '2000-12-31';

select * from books where title in ('The Shining' , 'Maitreyi');
select * from books where title not in ('The Shining' , 'Maitreyi');

select * from books where author !='Freida McFadden';
select * from books where pageNumber between 190 and 300;

**- AND**<br>

select * from books
where title like 'The%'
and releasedate like '2%';

**- OR**<br>

select * from books
where title like 'The%'
or releasedate like '2%';

select * from books
where title like 'The%'
or releasedate like '2%'
and author like '%Mc%';

select * from books
where (title like 'The%'
or releasedate like '2%')
and author like '%Mc%';

**- NOT**<br>

select * from books where pageNumber is null;
select * from books where pageNumber is not null;

**- like**<br>

select * from books where author like 'St%';
select * from books where author like '%en';
select * from books where author like '%e%';

select * from books where releasedate like '20%';
select * from books where releasedate like '%05%';
select * from books where releasedate like '%-05-%';

**- inner join**<br>

select * from books inner join authors on books.authorid = authors.authorid;

select books.title, authors.fullname, books.releasedate, books.pageNumber 
from books 
inner join authors 
on books.authorid = authors.authorid;

**- left join**<br>

select bookid, books.title, authors.fullname 
from authors 
left join books 
on books.authorid = authors.authorid;

**- OPTIONAL: right join**<br>

select books.title, authors.fullname, books.releasedate, books.pageNumber 
from books 
right join authors 
on books.authorid = authors.authorid;

**- OPTIONAL: cross join**<br>

**- functii agregate**<br>

select sum(pageNumber) from books;
select avg(pageNumber) from books;
select min(pageNumber) from books;
select max(pageNumber) from books;
select count(pageNumber) from books;
select count(title) from books;

select sum(pageNumber) from books where pageNumber > 200;
select count(*) from books where pageNumber is null;
select avg(pageNumber) from books where author like '%Mc%';

**- group by**<br>

select * from books
order by bookid desc;

select * from books
order by bookid asc;

select * from books 
order by author asc;

select * from books limit 2;

select * from books where title like 'The%' limit 2;

**- having**<br>
**- OPTIONAL DAR RECOMANDAT: Subqueries - nu au fost in scopul cursului. Puteti sa consultati tutorialul [asta](https://www.techonthenet.com/mysql/subqueries.php) si daca nu intelegeti ceva contactati fie trainerul, fie coordonatorul de grupa**<br>

</ol>

<li>Conclusions</li>

**I have learned how to create a database model using the tools of MySQL Workbench. I learned how to define tables, columns, and how to create one-to-many relationship between tables.**

</ol>
