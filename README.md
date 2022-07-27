# clean-code-SQL

## Table of Contents

1. [Introduction](#introduction)
2. [Table and Column Names](#table-and-column-names)
3. [Indenting](#Indenting)
4. [Joins](#Joins)
5. [SELECT](#select)


## Introduction

![Humorous image of software quality estimation as a count of how many expletives
you shout when reading code](http://www.osnews.com/images/comics/wtfm.jpg)

Software engineering principles, from Robert C. Martin's book
[_Clean Code_](https://www.amazon.co.uk/Clean-Code-Handbook-Software-Craftsmanship/dp/0132350882/ref=sr_1_1?adgrpid=55974923914&gclid=Cj0KCQjw_r3nBRDxARIsAJljleGT9BCkNKPgIoGAb_FeO8qQhDgrz3hCsMWDxqGwiDIRYAE7fJxmc7MaAkUCEALw_wcB&hvadid=259111288054&hvdev=c&hvlocphy=9044965&hvnetw=g&hvpos=1t2&hvqmt=e&hvrand=11926822464135628181&hvtargid=kwd-300722081224&hydadcr=17585_1775264&keywords=clean+code+handbook&qid=1559207523&s=gateway&sr=8-1),
adapted for SQL. This is not a style guide. It's a guide to developing
readable, reusable, and refactorable scripts in SQL.

Not every principle herein has to be strictly followed, and even fewer will be
universally agreed upon. These are guidelines and nothing more, but they are
ones codified over many years of collective experience.

Our craft of software engineering is just a bit over 50 years old, and we are
still learning a lot. When software architecture is as old as architecture
itself, maybe then we will have harder rules to follow. For now, let these
guidelines serve as a touchstone by which to assess the quality of the
SQL scripts that you and your team produce.

One more thing: knowing these won't immediately make you a better SQL developer, and working with them for many years doesn't mean you won't make
mistakes. Every piece of script starts as a first draft, like wet clay getting
shaped into its final form. Finally, we chisel away the imperfections when
we review it with our peers. Don't beat yourself up for first drafts that need
improvement. Beat up the code instead!

Special thanks to [riptutorial](https://riptutorial.com/) and stackoverflow

### SQL keywords are not case sensitive. However, it is common practice to write them in upper case.

## **Table and Column Names**

### Use meaningful and pronounceable Table/Column  names

A common way of formatting table/column names is lower case.

Names should describe what is stored in their object. This implies that column names usually should be singular. Whether table names should use singular or plural is a [heavily discussed](http://stackoverflow.com/questions/338156/table-naming-dilemma-singular-vs-plural-names) question, but in practice, it is more common to use plural table names.

Adding prefixes or suffixes like tbl or col reduces readability, so avoid them.

**Bad:**

```SQL
SELECT FName, LName
FROM Emps
WHERE Salary > 500;

SELECT f_name, l_name
FROM emps
WHERE salary > 500;
```

**Good:**

```SQL
SELECT firstname, lastname
FROM employees
WHERE Salary > 500;

SELECT first_name, last_name
FROM employees
WHERE salary > 500;
```

**[⬆ back to top](#table-of-contents)**

## **Indenting**

There is no widely accepted standard. What everyone agrees on is that squeezing everything into a single line is bad.

**Bad:**

```SQL
SELECT d.name, COUNT(*) AS employees FROM departments AS d JOIN employees AS e ON d.employeeid = e.departmentid WHERE d.name != 'HR' HAVING COUNT(*) > 10 ORDER BY COUNT(*) DESC;
```

At the minimum, put every clause into a new line, and split lines if they would become too long otherwise.

**Good:**

```SQL
SELECT d.name,
       COUNT(*) AS employees
FROM departments AS d
JOIN employees AS e ON d.id = e.departmentid
WHERE d.name != 'HR'
HAVING COUNT(*) > 10
ORDER BY COUNT(*) DESC;
```

Sometimes, everything after the SQL keyword introducing a clause is indented to the same column.

```SQL
SELECT   d.name,
         COUNT(*) AS employees
FROM     departments AS d
JOIN     employees AS e ON d.id = e.departmentid
WHERE    d.name != 'HR'
HAVING   COUNT(*) > 10
ORDER BY COUNT(*) DESC;
```

(This can also be done while aligning the SQL keywords right.)

Another common style is to put important keywords on their own lines.

```SQL
SELECT
    d.name,
    COUNT(*) AS employees
FROM
    departments AS d
JOIN
    employees AS e
    ON d.id = e.departmentid
WHERE
    d.name != 'HR'
HAVING
    COUNT(*) > 10
ORDER BY
    COUNT(*) DESC;
```

Vertically aligning multiple similar expressions improves readability.

```SQL
SELECT model,
       employeeid
FROM   cars
WHERE  customerid = 42
  AND  status     = 'READY';
```
### Using multiple lines makes it harder to embed SQL commands into other programming languages. However, many languages have a mechanism for multi-line strings, e.g., @"..." in C#, """...""" in Python, or R"(...)" in C++.



**[⬆ back to top](#table-of-contents)**

## **Joins**

Explicit joins should always be used; implicit joins have several problems.

The join condition is somewhere in the WHERE clause, mixed up with any other filter conditions. This makes it harder to see which tables are joined, and how.

Due to the above, there is a higher risk of mistakes, and it is more likely that they are found later.

In standard SQL, explicit joins are the only way to use outer joins.

```SQL
SELECT    d.name,
          e.firstname || e.lastame AS employeename
FROM      departments AS d
LEFT JOIN employees   AS e ON d.id = e.departmentid;
```

Explicit joins allow using the USING clause.

```SQL
SELECT recipeid,
       recipes.name,
       COUNT(*) AS numberofingredients
FROM   recipes
LEFT JOIN ingredients USING (recipeid);
```

(This requires that both tables use the same column name.
USING automatically removes the duplicate column from the result, e.g., the join in this query returns a single recipeid column.)


**[⬆ back to top](#table-of-contents)**

## **Select**

SELECT * returns all columns in the same order as they are defined in the table.

When using SELECT *, the data returned by a query can change whenever the table definition changes. This increases the risk that different versions of your application or your database are incompatible with each other.

Furthermore, reading more columns than necessary can increase the amount of disk and network I/O.

So you should always explicitly specify the column(s) you actually want to retrieve.

**Bad:**

```SQL
SELECT * 
  FROM employees;
```

**Good:**

```SQL
  SELECT id, fullname, lastname, phonenumber
  FROM   employees;
```

(When doing interactive queries, these considerations do not apply.)

However, SELECT * does not hurt in the subquery of an EXISTS operator, because EXISTS ignores the actual data anyway (it checks only if at least one row has been found). For the same reason, it is not meaningful to list any specific column(s) for EXISTS, so SELECT * actually makes more sense

```SQL
 -- list departments where nobody was hired recently
SELECT id,
       name
FROM departments
WHERE NOT EXISTS (SELECT *
                  FROM  employees
                  WHERE departmentid = departments.id
                    AND hiredate >= '2015-01-01');
```

**[⬆ back to top](#table-of-contents)**


