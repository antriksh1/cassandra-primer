# Cassandra Primer Demo Script

## STEP 1 : CQL Reference
[CQL Reference Docs](http://docs.datastax.com/en/cql/3.3/cql/cql_reference/cqlReferenceTOC.html)


## STEP 2:  Invoke cqlsh
```
    $  cqlsh
```

##  STEP 3: Create a Keyspace 'myflix'
Enter the following in cqlsh
```
    cqlsh>
        CREATE KEYSPACE myflix
        WITH REPLICATION = {
            'class' : 'SimpleStrategy',
            'replication_factor' : 1
            };

    cqlsh>
        describe keyspace myflix;
```

## STEP 4:  Create a 'features' table
Features could be movie or tv-show

First use keyspace `myflix`
```
    cqlsh>
        use myflix;

        CREATE TABLE features (
            code text,
            name text,
            type text,
            release_date timestamp,  // date,  what is the CQL type?
            PRIMARY KEY(code)
        );


     describe table features;
```

## STEP 5:  Insert Sample Data
```
    cqlsh>
        INSERT INTO features (code, name, type, release_date)
            VALUES ('madmen', 'Mad Men', 'TV Show', '2010-01-01');

        INSERT INTO features (code, name, type, release_date)
            VALUES ('sopr', 'Sopranos', 'TV Show', '2008-06-01');

        INSERT INTO features (code, name, type, release_date)
            VALUES ('star1', 'Star Wars Episode 1', 'Movie', '2008-12-31');

        INSERT INTO features (code, name, type, release_date)
            VALUES ('ryan', 'Saving Private Ryan', 'Movie', '2000-02-10');

        INSERT INTO features (code, name, type, release_date)
            VALUES ('loui1', 'Live from Beacon Theater', 'Live Comedy', '2011-02-10');
```

## STEP 6:   Inspect The Table
```
    cqlsh>     
            select * from features;
```


**=> Q : Is the output sorted in anyway?**  
      Rows?   Columns?

**=> Q : Is the output sorted by primary key (code) ? why (not) ?**  


## STEP 7:  Insert The Sample Data Again (Using Step 2)
Inspect the table
```
    cqlsh>     
            select * from features;
```
**=> Q: Do you see any duplicates?   why (not) ?**


## STEP 8: Query All Records
```
    cqlsh>  
        select * from features;
```


## STEP 9: Query A Row
```
    cqlsh>
        select * from features where code='madmen';
```


## STEP 10:  Display All TV-shows
```
    cqlsh>
        select * from features where type = 'TV Show';
```

**=> Q : What is the result of query execution?** 

##  STEP 11: Add an Index On 'Features' Table
Enter the following in cqlsh
```
    cqlsh>

        create index idx_type ON features (type)

        describe table features;
```

##  STEP 12: Try The Query Again
```
    cqlsh>
        select * from features where type = 'TV Show';
```


## STEP 13:  TTL -- Time To Live
Insert data with TTL
```
    cqlsh>
        INSERT INTO features(code, name)
        VALUES('simp', 'The Simpsons')
        USING TTL 20;
```

Issue a query immediately
```
    cqlsh>
        select * from features;
```

**=> Q: Do you see `simpsons` ? **

**=> Wait at least 20 seconds, and inspect the table again.**  
```
    cqlsh>
        select * from features;
```

**=> Q: Do you see `simpsons` now ? **  

**=> Q : Think of some applications you can build with this kind of functionality !**  
Hint : Any discrete chat apps? :-)

