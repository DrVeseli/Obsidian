
On jan. 3. 2024. Oracle introduces JS support in MySQL. And even tho I believe separation of concerns to be a thing of the past, it is not true for the data base.

```sql
mysql> CREATE FUNCTION hello (s CHAR(20))
mysql> RETURNS CHAR(50) DETERMINISTIC
       RETURN CONCAT('Hello, ',s,'!');
Query OK, 0 rows affected (0.00 sec)

mysql> SELECT hello('world');
+----------------+
| hello('world') |
+----------------+
| Hello, world!  |
+----------------+
1 row in set (0.00 sec)
```

There is a rightfully outdated practice called stored procedures that allowed for functions to be called during data storing. It was never great, and we moved beyond it. 

The reason for this attempted swing back started with GraphQL, also known as "no one wants to learn SQL" for some reason. Because you couldn't let anyone request anything from the data base you needed RLS, row level security, a micro process that determines if the client has privileges to access the data in the row. This is stupid, the client should not be able to access the database directly in any way shape or form.

You have the API and you have the server, they contact the data for the client!

# Why would MySQL do this?

Web dev has been moving serverless, people refuse to learn anything other then TypeScript, so why not? There are performance benefits when you run on the edge and your data is not on the same machine as the server, yes. You can do a lot less trips for the data if you do some calculations in the database. But working with something like this will be a nightmare, and the functions are written in JS which means there will be a lot of them, and most of them will be written poorly.

MySQL is not leading the charge in this, PVL8 for Postgre exist and is used, a lot. 

The V8 pandemic is in full swing and there seems to be nothing we can do to stop it.

Stay safe, use SQLite and compile your server to binary.