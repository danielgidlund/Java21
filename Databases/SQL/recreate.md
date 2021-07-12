# Recreate

*Purpose: the following script will make it easy to remove and recreate a database*

Add the following stored procedure "Recreate". It will make it easy to create a fresh database.

	use master
	go
	
	create schema Happybits
	go
	
	create or alter proc Happybits.Recreate @databasename varchar(50)
	as
	begin

		if DB_ID(@databasename) is not null
		begin
			exec ('alter database ' + @databasename + ' set single_user with rollback immediate')
			exec ('drop database ' + @databasename)
		end

		exec('create database '+ @databasename)

	end
	go


Alternative solution.

You probably don't need this

	alter proc Happybits.Recreate @databasename varchar(50)
	as
	begin

		declare @sql varchar(4000)
		set @sql = ''
		if DB_ID(@databasename) is not null
		begin
			set @sql = @sql + 'alter database ' + @databasename + ' set single_user with rollback immediate;'
			set @sql = @sql + 'drop database ' + @databasename + ';create database '+ @databasename+';'
		end

		print @sql
		exec(@sql)

	end
	go
