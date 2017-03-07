# liquibase

Test project that uses liquibase as a DB migration tool. Maven plugin used.

Something that kept my head scratching tonight was a special parameter on the connection URL that prevents a failure with the db.change.xml, see [Solution](https://shanmuha.github.io/2016/10/06/Liquibase-and-MySQLJ-6-Column-name-pattern-can-not-be-NULL-or-empty.html).
Adding &nullNamePatternMatchesAll=true to the end of the connection URL prevents the following error: 

When using liquibase with MySQL/J connection version 6.x, any subsequent liquibase update will error with the following errror:

        Caused by: liquibase.exception.DatabaseException: liquibase.exception.DatabaseException: java.sql.SQLException: Column name pattern can not be NULL or empty.
                at liquibase.snapshot.jvm.ColumnSnapshotGenerator.addTo(ColumnSnapshotGenerator.java:120)
                at liquibase.snapshot.jvm.JdbcSnapshotGenerator.snapshot(JdbcSnapshotGenerator.java:73)
                at liquibase.snapshot.SnapshotGeneratorChain.snapshot(SnapshotGeneratorChain.java:50)
                at liquibase.snapshot.DatabaseSnapshot.include(DatabaseSnapshot.java:194)
                at liquibase.snapshot.DatabaseSnapshot.init(DatabaseSnapshot.java:70)
                at liquibase.snapshot.DatabaseSnapshot.<init>(DatabaseSnapshot.java:44)
                at liquibase.snapshot.JdbcDatabaseSnapshot.<init>(JdbcDatabaseSnapshot.java:21)
                at liquibase.snapshot.SnapshotGeneratorFactory.createSnapshot(SnapshotGeneratorFactory.java:150)
                at liquibase.snapshot.SnapshotGeneratorFactory.createSnapshot(SnapshotGeneratorFactory.java:158)
                at liquibase.snapshot.SnapshotGeneratorFactory.getDatabaseChangeLogTable(SnapshotGeneratorFactory.java:165)
                at liquibase.changelog.StandardChangeLogHistoryService.init(StandardChangeLogHistoryService.java:101)
                ... 128 more
        Caused by: liquibase.exception.DatabaseException: java.sql.SQLException: Column name pattern can not be NULL or empty.
                at liquibase.snapshot.ResultSetCache.get(ResultSetCache.java:78)
                at liquibase.snapshot.JdbcDatabaseSnapshot$CachingDatabaseMetaData.getColumns(JdbcDatabaseSnapshot.java:239)
                at liquibase.snapshot.jvm.ColumnSnapshotGenerator.addTo(ColumnSnapshotGenerator.java:112)
                ... 138 more
        Caused by: java.sql.SQLException: Column name pattern can not be NULL or empty.
                at com.mysql.cj.jdbc.exceptions.SQLError.createSQLException(SQLError.java:569)
                at com.mysql.cj.jdbc.exceptions.SQLError.createSQLException(SQLError.java:537)
                at com.mysql.cj.jdbc.exceptions.SQLError.createSQLException(SQLError.java:527)
                at com.mysql.cj.jdbc.exceptions.SQLError.createSQLException(SQLError.java:512)
                at com.mysql.cj.jdbc.exceptions.SQLError.createSQLException(SQLError.java:480)
                at com.mysql.cj.jdbc.DatabaseMetaData.getColumns(DatabaseMetaData.java:2074)
                at liquibase.snapshot.JdbcDatabaseSnapshot$CachingDatabaseMetaData$3.fastFetchQuery(JdbcDatabaseSnapshot.java:277)
                at liquibase.snapshot.ResultSetCache$SingleResultSetExtractor.fastFetch(ResultSetCache.java:290)
                at liquibase.snapshot.ResultSetCache.get(ResultSetCache.java:58)
                ... 140 more
        
Future work will use `db.changelog.yml` file and see how that works. 
         