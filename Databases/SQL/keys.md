# Keys

*Purpose: intro to "keys". See the difference when keys are used or not*

Execute

	use master
	go
	exec Happybits.Recreate 'Demo'
	use Demo

## Exercise

Create a table "Color" with the columns:
- Id
- Name

Solution

	create table Color(
		Id int, 
		Name varchar(50),
	)

## Exercise

Create a table "Person" with:
- Name
- FavoriteColorId 

Solution

	create table Person(
		Name varchar(50),
		FavoriteColorId int 
	)

## Exercise

Add the following colors:
- 91 Red
- 92 Green
- 93 Blue
- 94 Purple
- 95 Indigo

Solution

	insert into Color 
	values
	(91, 'Red'),
	(92, 'Green'),
	(93, 'Blue'),
	(94, 'Purple'),
	(95, 'Indigo')

## Exercise

Add this people:
- Mia likes Red
- James likes Green
- Liam likes Blue

Solution

	insert into Person
	values
	('Mia', 91),
	('James', 92),
	('Liam', 93)

## Exercise

List all content form the tables

	select * from Color
	select * from Person

Add this color:
- 91 Yellow

What's the problem with the data now?

Solution:

	insert into Color values (91, 'Yellow');

	Now it's not cleared what the color with Id=91 is. Is it red or yellow? Not good.

## Exercise

Add this person

- Joe 666666

What's the problem with the data now?

Solution

	insert into Person values ('Joe', 666666);

	Joe like the color 666666, but what color is that?
	No one knows...


## Exercise (hard, extra exercise)

Add a "foreign key" and "primary key" after the tables are created as above

Hint: use 
- alter table
- alter column

Solution

	-- Don't allow null for column Id (in Color table)
	alter table Color
	alter column Id int not null
	go

	-- Have to remove Yellow to be able to create primary key
	delete from Color where Name='Yellow'
	go

	-- Add a primary key to column Id (in Color table)
	alter table Color
	add primary key(Id)
	go

	-- Have to remove FavoriteColor 666666 to be able to add a foreign key later
	delete from Person where FavoriteColor=666666
	go

	-- Add a foreign key to Person, so no one can enter strange color-id's in the Person table
	alter table Person with 
	check add constraint fk_person_color foreign key(FavoriteColor)
	references Color (id)

## Hint

When running many statements at once, you might need to add

	GO

between the statements

## Exercise

Now we will restart this example. Clear the database and start to use a **primary key** and **foreign key**..**

Execute

	use master
	exec Happybits.Recreate 'Demo'
	use Demo

Add the following two tables with data:

	create table Color(
		Id int primary key, --- new!
		Name varchar(50),
	)

	create table Person(
		Name varchar(50),
		FavoriteColor int foreign key references Color(Id), --- new!
	)

	insert into Color 
	values
	(91, 'Red'),
	(92, 'Green'),
	(93, 'Blue'),
	(94, 'Purple'),
	(95, 'Indigo')

	insert into Person
	values
	('Mia', 91),
	('James', 92),
	('Liam', 93)


## Exercise

Try to add
- 91 Yellow

What happens? Good/bad?

Solution

	insert into Color values (91, 'Yellow');

	We get this

		Violation of PRIMARY KEY constraint 'PK__Color__3214EC0773BC1B85'. Cannot insert duplicate key in object 'dbo.Color'. The duplicate key value is (91).

	Problem solved! This is not allowed anymore because of out "primary key" above. Good!

	If you now run 

		select * from Color

	...you see that the color Yellow wasn't inserted (good)

## Exercise

Try to add the person
- Joe 666666

What happens? Good/bad?

Solution

	insert into Person values ('Joe', 666666);

	We get

		The INSERT statement conflicted with the FOREIGN KEY constraint "FK__Person__Favorite__37A5467C". The conflict occurred in database "Demo", table "dbo.Color", column 'Id'.

	Problem solved! This is now allowed anymore because of out "foreign key" above. Good!
