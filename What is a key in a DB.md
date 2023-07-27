- Used to allow the arrangement of various tables in a certain DB
- Used to be able to create relations between diff tables
- Used for identification of various records or combinations of records from the the DB

DB Name: Restaurant
- ResAdress
- ResName
- ResSpecialization
- ResIDNumber
- ResPhonenumber
- ResEmail

## Primary Key
- Most important key
- Only 1 primary key in table. Cant be a dupe or a null value
- Ex ResIDNumbner

## Unique Key
- Can be a single or a set of columns
- Cant have dupe values
- Ex: ResPhonenumber, ResEmail

## Super key
- A single or a set of multiple keys used to identify Data
- Ex: ResIDNumber and ResName

## Alternate Key
- Can be used as primary key candidate when needed
- Ex: ResEmail

## Candidate Key
- A set of single column or multiple columns. Are unique keys that can also replace the primary key
- Ex: ResPhonenumber, ResEmail

## Composite key
- Normally combination of columns that are used to identify records in a table
- Ex: ResIDNumber and ResPhonenumber

## Foreign Key
- Key that simply part of another DB and has been imported
- Ex: ResIDNumber


