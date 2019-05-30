# clean-code-SQL

## Table of Contents

1. [Introduction](#introduction)
2. [Table and Column Names](#Table and Column Names)
3. [Functions](#functions)
4. [Objects and Data Structures](#objects-and-data-structures)
5. [Classes](#classes)
6. [SOLID](#solid)
7. [Testing](#testing)
8. [Concurrency](#concurrency)
9. [Error Handling](#error-handling)
10. [Formatting](#formatting)
11. [Comments](#comments)
12. [Translation](#translation)

## Introduction

![Humorous image of software quality estimation as a count of how many expletives
you shout when reading code](http://www.osnews.com/images/comics/wtfm.jpg)

Software engineering principles, from Robert C. Martin's book
[_Clean Code_](https://www.amazon.com/Clean-Code-Handbook-Software-Craftsmanship/dp/0132350882),
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

### SQL keywords are not case sensitive. However, it is common practice to write them in upper case.

## **Table and Column Names**

### Use meaningful and pronounceable Table/Column  names

Two common way of formatting table/column names are lower case.

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

