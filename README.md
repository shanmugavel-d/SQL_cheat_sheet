# SQL_cheat_sheet
self study notes reference 
SQL
Select :
tables : t

cols : col1,col2….

SELECT * FROM t #to select some columns
SELECT col1,col2... FROM t #to select some columns
SELECT expression FROM t #to select the expressions on columns (eg can be double a column or concatanation of two columns)
select 1 from t #returns just value 1 repeated for all rows
select col1*2 from t #returns twice the first column value
ALIAS
SELECT expression as alias from table_name #the expression is now desplayed with the alias column name
Where :
Lets you filter data depending on condition

SELECT col1,col2.... 
FROM t
WHERE condition1 and/or/not condition2 and/or/not condition3
CREATE TABLE t (
  `col1` INTEGER,
  `col2` VARCHAR(1)
);

INSERT INTO t
  (`col1`, `col2`)
VALUES
  ('1', 'a'),
  ('2', 'b'),
  ('3', 'c'),
  ('4', 'd'),
  ('5', 'e');
#get col1 > 2
SELECT * FROM t
WHERE col1 > 2

#filter on string based column -> put it inside ''
SELECT * FROM t
WHERE col2 = 'a'
Name	Description
>	Greater than operator
=>	Greater than or equal operator
<=	Less than operator
<>, !=	Not equal operator
<=	Less than or equal operator
<=>	NULL-safe equal to operator
=	Equal operator
BETWEEN … AND …	Whether a value is within a range of values
COALESCE()	Return the first non-NULL argument
GREATEST()	Return the largest argument
IN()	Whether a value is within a set of values
INTERVAL()	Return the index of the argument that is less than the first argument
IS	Test a value against a boolean
IS NOT	Test a value against a boolean
IS NOT NULL	NOT NULL value test
IS NULL	NULL value test
ISNULL()	Test whether the argument is NULL
LEAST()	Return the smallest argument
LIKE	Simple pattern matching
NOT BETWEEN ... AND …	Whether a value is not within a range of values
NOT IN()	Whether a value is not within a set of values
NOT LIKE	Negation of simple pattern matching
STRCMP()	Compare two strings
Expresssions that return boolean values, can return 3 types of values :

1 → true

0 → false

null

IS NULL : Querying for NULL/NON-NULL columns can be done by ‘=’, you need to use the is NULL/is NOT NULL keyword

SELECT * FROM t
WHERE col2 is NULL
IN : lets you choose from a list of values.

SELECT * FROM t
where col1 in (1,3,5)
LIKE : lets you match patterns using wildcards,

% → any number of characters.

_ → single char

movie_name like ‘The%’	gives all movies that start with The
movie_name like ‘%and%’	gives all movies that have and somewhere in the middle
column_name like ‘a__’	match all columns of length 3 starting with a
Limit :
lets you get a certain number of rows

Offset
lets you do the limit operation starting after the offset number

#refer to countries.csv
select * from countries
where Country LIKE 'A%'
limit 5
offset 1
Distinct :
selects distinct combination of values from the rows only for the selected ones

CREATE TABLE t (
  `a` INTEGER,
  `b` VARCHAR(1),
  `c` VARCHAR(1)
);

INSERT INTO t
  (`a`, `b`, `c`)
VALUES
  ('1', 'a', 'A'),
  ('1', 'a', 'A'),
  ('1', 'a', 'B'),
  ('1', 'b', 'B'),
  ('1', 'b', 'C'),
  ('2', 'a', 'A'),
  ('2', 'a', 'A'),
  ('2', 'a', 'B'),
  ('2', 'b', 'B'),
  ('2', 'b', 'C');
select distinct a , b from t
#returns
#1 a
#1 b
#2 a
#2 b
#NOTE distinct is being applied to both a and b not just to a
Count
to count total number of rows :

CREATE TABLE t (
  `col1` INTEGER,
  `col2` VARCHAR(1),
  `col3` VARCHAR(1)
);

INSERT INTO t
  (`col1`, `col2`, `col3`)
VALUES
  (NULL, 'a', 'A'),
  ('1', 'a', 'A'),
  ('1', 'a', 'B'),
  ('1', NULL, 'B'),
  ('1', NULL, 'C'),
  ('2', NULL, 'A'),
  ('2', 'a', 'A'),
  ('2', 'a', 'B'),
  ('2', 'b', 'B'),
  (NULL, 'b', 'C');
SELECT COUNT(*) FROM table_name
to count total non null values for a particular column :

SELECT count(col_name) FROM table_name
to count total non null distinct values for a particular column :

SELECT count(distinct col_name) FROM table_name
select count(col1) as col1_count, count(col2) as col2_count
from t
#RETURNS 8,7
Aggregate :
SELECT agg_fn(col)
FROM table
The functions SUM, COUNT, MAX,MIN and AVG are "aggregates", each may be applied to a numeric attribute resulting in a single row being returned by the query

GROUP BY
creates groups for the columns put inside the group by clause, all the columns in the same group get coalesced down into one. The only ppt of a group that can now be selected is the group columns and aggregates(count,sum,max,min)

select cols 
from table
group by g_cols

#select can only have columns that are present in group by and aggregates.
ORDER BY
lets you output based on the value of a particular column(s)

if there are multiple cols inside order the 2,3,4… column will be tie-breakers

select cols 
from table
group by g_cols
order by o_cols
select *, rank() over(partition by class,section order by marks), max(marks) over (partition by class,section) from stu order by class,section

Having
To filter on top of aggr columns,

select cols 
from table_name
where normal_condition
group by g_cols
having aggregate_condition #this applies to aggr columns
#refer to students.sql
select class,section,count(*) as total_in_class
from stu
group by class, section
having total_in_class > 2
order by class, section
IF
if takes three args, first is condition and the value is second arg if the condition is true else the third arg

#syntax
select col, IF(cond,cond_true_expr,cond_false_expr)
from tb
#creation
CREATE TABLE info (
  `first_name` VARCHAR(8),
  `age` INTEGER
);

INSERT INTO info
  (`first_name`, `age`)
VALUES
  ('Delmor', '20'),
  ('Rena', '20'),
  ('Bonni', '3'),
  ('Korrie', '27'),
  ('Abagael', '3'),
  ('Chloette', '28'),
  ('Mar', '29'),
  ('Elwin', '19'),
  ('Johann', '28'),
  ('Kelly', '22'),
  ('Catlee', '11'),
  ('Vonnie', '25'),
  ('Fionnula', '7'),
  ('Jermaine', '7'),
  ('Ruby', '4');
#query

SELECT first_name as name, if(age>=18, 'adult', 'minor')
from info
Case
#syntax
CASE
    WHEN condition1 THEN result1
    WHEN condition2 THEN result2
    WHEN conditionN THEN resultN
    ELSE result
END;
#creation
CREATE TABLE marks (
  `student` VARCHAR(10),
  `marks` INTEGER
);

INSERT INTO marks
  (`student`, `marks`)
VALUES
  ('Ewan', '10'),
  ('Brenn', '42'),
  ('Ferdinanda', '24'),
  ('Rozele', '69'),
  ('Guthrie', '79'),
  ('Francene', '68'),
  ('Darcey', '77'),
  ('Jerry', '19'),
  ('Jehu', '89'),
  ('Joanne', '32'),
  ('Reeta', '3'),
  ('Ros', '90'),
  ('Lorianna', '70'),
  ('Hanna', '60'),
  ('Ulberto', '4'),
  ('Faustina', '35'),
  ('Otho', '47'),
  ('Francklin', '40'),
  ('Otes', '48'),
  ('Zaneta', '58');
#query
select 
  student,
  marks,
  CASE
    WHEN marks<20 THEN 'E'
    WHEN marks<40 THEN 'D'
    WHEN marks<60 THEN 'C'
    WHEN marks<80 THEN 'B'
    ELSE 'A'
  END as grade
from marks
Join
output table gets all columns from the first and the second table
works as a filter on a cross
join

JOIN →

output table with schema from both
Cross
apply the join condition (predicate) as a filter
see if it is left.right.inner
SELECT cols from table1 [type_of_join] table2 ON condition
#t1 -> a t2->a,b
SELECT t1.a , b from t1 left join t2 on t1.a = t2.a 
