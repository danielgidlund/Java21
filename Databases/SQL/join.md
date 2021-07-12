# Join

*Purpose: compare different types of "joins" + read data and create tables*

Read more:

	https://www.w3schools.com/sql/sql_join.asp
	https://www.w3schools.com/sql/sql_join_inner.asp
	https://www.w3schools.com/sql/sql_join_left.asp
	https://www.w3schools.com/sql/sql_join_right.asp
	https://www.w3schools.com/sql/sql_join_full.asp

## Exercise

Use the GUI (Management Studio) to solve this exercise

Create a new database named Demo

Create the tables Person and Color

Add colors 

- 91 Red
- 92 Green
- 93 Blue
- 94 Purple
- 95 Indigo

...and people

- Mia and Olivia like red
- James likes green
- Liam likes blue
- Ava don't know
- An undefined person likes red

In the following exercises you'll only write SQL-code (not use the GUI)

## Exercise

Create a clean database

	use master
	exec Happybits.Recreate 'Demo'
	go
	use Demo

## Exercise

Create a table "Person" with two columns:
- Name (a string)
- FavoriteColor (a number)

Solution

	create table Person(
		Name varchar(50),
		FavoriteColor int
	)

## Exercise

Create a table "Color" with two columns:
- Id (a number)
- Name (a string)

Solution

	create table Color(
		Id int,
		Name varchar(50)
	)

## Exercise

Add the following to the Color table:

- 91 Red
- 92 Green
- 93 Blue
- 94 Purple
- 95 Indigo

...then show the content of the table

Solution

	insert into Color 
	values
	(91, 'Red'),
	(92, 'Green'),
	(93, 'Blue'),
	(94, 'Purple'),
	(95, 'Indigo')

	select * from Color

## Hint

Press Ctrl-Shift-R to "Refresh Local Cache". Then "intellisence" will work better.

## Exercise

Add the following People: 
- Mia and Olivia like red
- James likes green
- Liam likes blue
- Ava don't know
- An undefined person likes red

...the list all People

Solution

	insert into Person
	values
	('Mia', 91),
	('Olivia', 91),
	('James', 92),
	('Liam', 93),
	('Ava',	null),
	(null, 91)

	select * from Person
	
## Exercise

List all info of both tables:

	select * from Person
	select * from Color

Run the command below:

	select * 
	from Person 
	join Color 
	on Person.FavoriteColor = Color.Id

...what result do you get?

Solution

	Inner join
	- Only the people that has a favorite color is included in the result
	- So Ava in not in the result
	- "join" = "inner join"
	- (Person with name "null" will be included)

## Exercise

Run the command below:

	select * 
	from Person 
	left join Color 
	on Person.FavoriteColor = Color.Id

...what result do you get?

Solution

	Left join
	- Same as above but all from the left table is in the result
	- So Ava is in the result even if she doesn't have a favorite color 

## Exercise

Do a "right join". What result do you get?

	select * 
	from Person 
	right join Color 
	on Person.FavoriteColor = Color.Id

	Right join:
	- All from the right table is in the result
	- So the colors that no one likes (Purple and Indigo) is in the result
	- (Ava is not in the result)

## Exercise

Do a "full join". What result do you get?

Solution

	select * 
	from Person 
	full join Color 
	on Person.FavoriteColor = Color.Id

	Full join
	- All rows from both tables is included (at least ones)
	- So Ava is include. And Purple and Indigo
	- All people + a nullrow for green = 3+1=4 rows
	- "full join" == "full outer join"  

