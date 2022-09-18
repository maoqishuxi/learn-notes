# go documentation database tutorial

## Tutorial: Accessing a relational database

目录

1. Prerequisites 准备
2. Create a folder for your code 为你的代码创建一个文件夹
3. Set up a database 设置一个数据库
4. Find and import a database driver 查找并导入一个数据库驱动
5. Get a database handle and connect 获取数据库句柄并连接
6. Query for multiple rows 多行查询
7. Query for a single row 单行查询
8. Add data 添加数据
9. Conclusion 总结
10. Completed code  完成的代码

这篇教程是介绍用`go`和`database/sql`标准库包访问基础的关系型数据库

您将使用的 `database/sql` 包的类型和函数用于连接数据库、执行事务、取消正在进行的操作等。

在这篇教程中，你将逐步通过以下部分：

1. Create a folder for you code.
2. Set up a database.
3. import the database driver.
4. Get a database handle and connect.
5. Query for multiple rows.
6. Query for a single row.
7. Add data.

Prerequisites(准备)

- 一个已安装的MySQL关系型数据库管理系统
- 一个已安装的Go。
- 一个代码编辑器
- 一个命令行

### 为你的代码创建一个文件夹

Next, you’ll create a database.

### 设置数据库

在这一步，你将用你创建的数据库工作。你将用`CLI`DBMS数据库管理工具创建数据库和表并添加数据。

1. Open a new command prompt.

2. At the command line, log into your DBMS, as in the following example for MySQL.(用命令行登录到你的数据库管理工具)

3. At the `mysql` command prompt, create a database.

4. Change to the database you just created so you can add tables. (切换到创建的数据库，你可以添加表)

5. In your text editor, in the data-access folder, create a file called `create-tables.sql` to hold SQL script for adding tables.在文本编辑器数据访问文件夹创建一个`create-tables.sql`去保存添加表SQL脚本

6. Into the file, paste the following SQL code, then save the file.
   ```
   DROP TABLE IF EXISTS album;
   CREATE TABLE album (
     id         INT AUTO_INCREMENT NOT NULL,
     title      VARCHAR(128) NOT NULL,
     artist     VARCHAR(255) NOT NULL,
     price      DECIMAL(5,2) NOT NULL,
     PRIMARY KEY (`id`)
   );
   
   INSERT INTO album
     (title, artist, price)
   VALUES
     ('Blue Train', 'John Coltrane', 56.99),
     ('Giant Steps', 'John Coltrane', 63.99),
     ('Jeru', 'Gerry Mulligan', 17.99),
     ('Sarah Vaughan', 'Sarah Vaughan', 34.98);
   ```

7. From the `mysql` command prompt, run the script you just created. 从 `mysql` 命令行运行你刚创建的脚本
   You'll use the `source` command in the following form: 

   ```
   mysql> source /path/to/create-tables.sql
   ```

8. At your DBMS command prompt, use a `SELECT` statement to verify you've successfully created the table with data. 用`SELECT` 语句验证你成功创建的表数据
   ```
   mysql> select * from album;
   +----+---------------+----------------+-------+
   | id | title         | artist         | price |
   +----+---------------+----------------+-------+
   |  1 | Blue Train    | John Coltrane  | 56.99 |
   |  2 | Giant Steps   | John Coltrane  | 63.99 |
   |  3 | Jeru          | Gerry Mulligan | 17.99 |
   |  4 | Sarah Vaughan | Sarah Vaughan  | 34.98 |
   +----+---------------+----------------+-------+
   4 rows in set (0.00 sec)
   ```

Next, you’ll write some Go code to connect so you can query.

### Find and import a database driver

现在你已得到数据库和一些数据，开始你的Go代码

Locate (发现) and import a database driver that will translate requests you make through functions in the `database/sql` package into requests the database understands.

找到并导入一个数据库驱动它将翻译你通过`database/sql` 包函数的请求使数据库理解。

1. In your browser, visit the [`SQLDrivers`](https://github.com/golang/go/wiki/SQLDrivers) wiki page to identify a driver you can use. 
   Use the list on the page to identify the driver you’ll use. For accessing MySQL in this tutorial, you’ll use [Go-MySQL-Driver](https://github.com/go-sql-driver/mysql/).

2. Note the package name for the driver – here, `github.com/go-sql-driver/mysql`.

3. Using your text editor, create a file in which to write your Go code and save the file as `main.go` in the data-access directory you created earlier. 在工作目录创建`main.go` 文件

4. Into `main.go`, paste the following code to import the driver package.
   ```
   package main
   
   import "github.com/go-sql-driver/mysql"
   ```

With the driver imported, you’ll start writing code to access the database.

### Get a database handle and connect

Now write some Go code that gives you database access with a database handle. 现在写一些让你能够用数据句柄访问数据库的go 代码

You’ll use a pointer to an `sql.DB` struct, which represents access to a specific database. 用 `sql.DB` 结构表示对指定数据库的访问

#### Write the code

1. Into `main.go` , beneath the `import` code you just added, paste the following Go code to create a database handle. 
   ```
   var db *sql.DB
   
   func main() {
       // Capture connection properties.
       cfg := mysql.Config{
           User:   os.Getenv("DBUSER"),
           Passwd: os.Getenv("DBPASS"),
           Net:    "tcp",
           Addr:   "127.0.0.1:3306",
           DBName: "recordings",
       }
       // Get a database handle.
       var err error
       db, err = sql.Open("mysql", cfg.FormatDSN())
       if err != nil {
           log.Fatal(err)
       }
   
       pingErr := db.Ping()
       if pingErr != nil {
           log.Fatal(pingErr)
       }
       fmt.Println("Connected!")
   }
   ```

   In this code, you:

   - Declare a `db` variable of type `*sql.Db`. This is your database handle.
     Making `db` a global variable simplifies this example. In production, you'd avoid the global variable, such as by passing the variable to functions that need it or by wrapping it in a struct. 在生产环境中，你可以把`db`放在结构中。
   - Use the MySQL driver’s [`Config`](https://pkg.go.dev/github.com/go-sql-driver/mysql#Config) – and the type’s [`FormatDSN`](https://pkg.go.dev/github.com/go-sql-driver/mysql#Config.FormatDSN) -– to collect connection properties and format them into a DSN for a connection string. 用MySQL驱动和类型格式化去收集连接属性和格式化它们成为DSN连接字符。
   - Call [`sql.Open`](https://pkg.go.dev/database/sql#Open) to initialize the `db` variable, passing the return value of `FormatDSN`. 
   - Check for an error from `sql.Open`. It could fail if, for example, your database connection specifics weren’t well-formed.
     To simplify the code, you’re calling `log.Fatal` to end execution and print the error to the console. In production code, you’ll want to handle errors in a more graceful way. 为了简化代码用`log.Fatal` 去结束执行和打印出错误信息，在生产环境有更好的错误处理方式
   - Call [`DB.Ping`](https://pkg.go.dev/database/sql#DB.Ping) to confirm that connecting to the database works. At run time, `sql.Open` might not immediately connect, depending on the driver. You’re using `Ping` here to confirm that the `database/sql` package can connect when it needs to. 使用 Ping 来确认 database/sql 包可以在需要时连接。
   - Check for an error from `Ping`, in case the connection failed.
   - Print a message if `Ping` connects successfully.

2. Near the top of the main.go file, just beneath the package declaration, import the packages you’ll need to support the code you’ve just written.

   The top of the file should now look like this:

   ```
   package main
   
   import (
       "database/sql"
       "fmt"
       "log"
       "os"
   
       "github.com/go-sql-driver/mysql"
   )
   ```

3. Save main.go.

#### Run the code 

1. Begin tracking(追踪) the MySQL driver module as a dependency. 
   Use the [`go get`](https://go.dev/cmd/go/#hdr-Add_dependencies_to_current_module_and_install_them) to add the github.com/go-sql-driver/mysql module as a dependency for your own module. Use a dot argument to mean “get dependencies for code in the current directory.”

   ```
   $ go get .
   go get: added github.com/go-sql-driver/mysql v1.6.0
   ```

2. From the command prompt, set the `DBUSER` and `DBPASS` environment variables for use by the Go program.
   On Linux or Mac:

   ```
   $ export DBUSER=username
   $ export DBPASS=password
   ```

   On Windows:

   ```
   C:\Users\you\data-access> set DBUSER=username
   C:\Users\you\data-access> set DBPASS=password
   ```

3. From the command line in the directory containing main.go, run the code by typing `go run` with a dot argument to mean “run the package in the current directory.”
   ```
   $ go run .
   Connected!
   ```

You can connect! Next, you’ll query for some data.

### Query for multiple rows

In this section, you'll use Go to execute an SQL query designed to return multiple rows.

For SQL statements that might return multiple rows, you use the `Query` method from the `database/sql` package, then loop through the rows it returns. (You’ll learn how to query for a single row later, in the section [Query for a single row](https://go.dev/doc/tutorial/database-access#single_row).)

#### Write the code 

1. Into main.go, immediately above `func main`, paste the following definition of an `Album` struct. You’ll use this to hold row data returned from the query. 在`func main` 上方粘贴`Album` 结构。你将用它来保存返回的行数据。
   ```
   type Album struct {
       ID     int64
       Title  string
       Artist string
       Price  float32
   }
   ```

2. Beneath `func main`, paste the following `albumsByArtist` function to query the database. 
   ```
   // albumsByArtist queries for albums that have the specified artist name.
   func albumsByArtist(name string) ([]Album, error) {
       // An albums slice to hold data from returned rows.
       var albums []Album
   
       rows, err := db.Query("SELECT * FROM album WHERE artist = ?", name)
       if err != nil {
           return nil, fmt.Errorf("albumsByArtist %q: %v", name, err)
       }
       defer rows.Close()
       // Loop through rows, using Scan to assign column data to struct fields.
       for rows.Next() {
           var alb Album
           if err := rows.Scan(&alb.ID, &alb.Title, &alb.Artist, &alb.Price); err != nil {
               return nil, fmt.Errorf("albumsByArtist %q: %v", name, err)
           }
           albums = append(albums, alb)
       }
       if err := rows.Err(); err != nil {
           return nil, fmt.Errorf("albumsByArtist %q: %v", name, err)
       }
       return albums, nil
   }
   ```

   In this code, you: 

   - Declare an `albums` slice of the `Album` type you defined. This will hold data from returned rows. Struct field names and types correspond to database column names and types. 结构字段名和类型与数据库字段名和类型对应。
   - Use [`DB.Query`](https://pkg.go.dev/database/sql#DB.Query) to execute a `SELECT` statement to query for albums with the specified artist name. 
     `Query`’s first parameter is the SQL statement. After the parameter, you can pass zero or more parameters of any type. These provide a place for you to specify the values for parameters in your SQL statement. By separating the SQL statement from parameter values (rather than concatenating them with, say, `fmt.Sprintf`), you enable the `database/sql` package to send the values separate from the SQL text, removing any SQL injection risk. 
   - Defer closing `rows` so that any resources it holds will be released when the function exits. 关闭rows释放资源
   - Loop through the returned rows, using [`Rows.Scan`](https://pkg.go.dev/database/sql#Rows.Scan) to assign each row’s column values to `Album` struct fields. 通过Rows.Scan去分配每行的列值给结构字段。
     `Scan` takes a list of pointers to Go values, where the column values will be written. Here, you pass pointers to fields in the `alb` variable, created using the `&` operator. `Scan` writes through the pointers to update the struct fields. 通过指针赋值
   - Inside the loop, check for an error from scanning column values into the struct fields. 检查从列值赋值到结构字段的错误。
   - Inside the loop, append the new `alb` to the `albums` slice. 添加数据
   - After the loop, check for an error from the overall query, using `rows.Err`. Note that if the query itself fails, checking for an error here is the only way to find out that the results are incomplete. 检查完整查询的错误，此处是找到结果不完整的唯一方法。

3. Update your `main` function to call `albumsByArtist`.
   To the end of `func main`, add the following code.

   ```
   albums, err := albumsByArtist("John Coltrane")
   if err != nil {
       log.Fatal(err)
   }
   fmt.Printf("Albums found: %v\n", albums)
   ```

   In the new code, you now:

   - Call the `albumsByArtist` function you added, assigning its return value to a new `albums` variable.
   - Print the result.

#### Run the code

From the command line in the directory containing main.go, run the code.

```
$ go run .
Connected!
Albums found: [{1 Blue Train John Coltrane 56.99} {2 Giant Steps John Coltrane 63.99}]
```

Next, you’ll query for a single row.

### Query for a single row

In this section, you’ll use Go to query for a single row in the database.

For SQL statements you know will return at most a single row, you can use `QueryRow`, which is simpler than using a `Query` loop.

#### Write the code

1. Beneath `albumsByArtist`, paste the following `albumByID` function.
   ```
   // albumByID queries for the album with the specified ID.
   func albumByID(id int64) (Album, error) {
       // An album to hold data from the returned row.
       var alb Album
   
       row := db.QueryRow("SELECT * FROM album WHERE id = ?", id)
       if err := row.Scan(&alb.ID, &alb.Title, &alb.Artist, &alb.Price); err != nil {
           if err == sql.ErrNoRows {
               return alb, fmt.Errorf("albumsById %d: no such album", id)
           }
           return alb, fmt.Errorf("albumsById %d: %v", id, err)
       }
       return alb, nil
   }
   ```

   In this code, you:

   - Use [`DB.QueryRow`](https://pkg.go.dev/database/sql#DB.QueryRow) to execute a `SELECT` statement to query for an album with the specified ID.
     It returns an `sql.Row`. To simplify the calling code (your code!), `QueryRow` doesn’t return an error. Instead, it arranges to return any query error (such as `sql.ErrNoRows`) from `Rows.Scan` later. `QueryRow` 不返回任何错误。它返回查询错误在`Rows.Scan` 之后。
   - Use [`Row.Scan`](https://pkg.go.dev/database/sql#Row.Scan) to copy column values into struct fields.
   - Check for an error from `Scan`.
     The special error `sql.ErrNoRows` indicates that the query returned no rows. Typically that error is worth replacing with more specific text, such as “no such album” here. 空行错误

2. Update `main` to call `albumByID`.

   To the end of `func main`, add the following code.
   ```
   // Hard-code ID 2 here to test the query.
   alb, err := albumByID(2)
   if err != nil {
       log.Fatal(err)
   }
   fmt.Printf("Album found: %v\n", alb)
   ```

   In the new code, you now:

   - Call the `albumByID` function you added.
   - Print the album ID returned.

#### Run the code

From the command line in the directory containing main.go, run the code.

```
$ go run .
Connected!
Albums found: [{1 Blue Train John Coltrane 56.99} {2 Giant Steps John Coltrane 63.99}]
Album found: {2 Giant Steps John Coltrane 63.99}
```

Next, you’ll add an album to the database.

### Add data

In this section, you’ll use Go to execute an SQL `INSERT` statement to add a new row to the database. 在这节，你将用Go 去执行SQL `INSERT` 语句去添加一个新行到数据库

You’ve seen how to use `Query` and `QueryRow` with SQL statements that return data. To execute SQL statements that *don’t* return data, you use `Exec`. 你已经看到怎么用`Query` 和`QueryRow` 语句返回数据。去执行SQL语句不返回数据，你可以用`Exec` 。

#### Write the code 

1. Beneath `albumByID`, paste the following `addAlbum` function to insert a new album in the database, then save the main.go.  插入一个新的album到数据库
   ```
   // addAlbum adds the specified album to the database,
   // returning the album ID of the new entry
   func addAlbum(alb Album) (int64, error) {
       result, err := db.Exec("INSERT INTO album (title, artist, price) VALUES (?, ?, ?)", alb.Title, alb.Artist, alb.Price)
       if err != nil {
           return 0, fmt.Errorf("addAlbum: %v", err)
       }
       id, err := result.LastInsertId()
       if err != nil {
           return 0, fmt.Errorf("addAlbum: %v", err)
       }
       return id, nil
   }
   ```

   In this code, you:

   - Use `DB.EXEC` to execute an `INSERT` statement.
     Like `Query`, `Exec` takes an SQL statement followed by parameter values for the SQL statement. 跟`Query`一样`Exec`也是用SQL语句的参数值
   - Check for an error from the attempt to `INSERT`.
   - Retrieve the ID of the inserted database row using [`Result.LastInsertId`](https://pkg.go.dev/database/sql#Result.LastInsertId).
     获取插入数据库行的ID
   - Check for an error from the attempt to retrieve the ID. 检查尝试获取ID的错误

2. Update `main` to call the new `addAlbum` function. To the end of `func main`, add the following code. 
   ```
   albID, err := addAlbum(Album{
       Title:  "The Modern Sound of Betty Carter",
       Artist: "Betty Carter",
       Price:  49.99,
   })
   if err != nil {
       log.Fatal(err)
   }
   fmt.Printf("ID of added album: %v\n", albID)
   ```

   In the new code, you now:

   - Call `addAlbum` with a new album, assigning the ID of the album you’re adding to an `albID` variable.  调用`addAlbum`插入新album，分配ID给`albID` 变量

#### Run the code

From the command line in the directory containing main.go, run the code.

```
$ go run .
Connected!
Albums found: [{1 Blue Train John Coltrane 56.99} {2 Giant Steps John Coltrane 63.99}]
Album found: {2 Giant Steps John Coltrane 63.99}
ID of added album: 5
```

## Accessing relational databases

Table of contents

1. Supported database management systems 支持哪些驱动

2. Functions to execute queries or make database changes 执行查询和更改数据库的函数

3. Transactions 事务

4. Query cancellation 回滚

5. Managed connection pool 连接池

Using Go, you can incorporate a wide variety of databases and data access approaches into your applications. Topics in this section describe how to use the standard library’s [`database/sql`](https://pkg.go.dev/database/sql) package to access relational databases. Go可以用各种数据库和数据访问方法在你的程序中。本节中的主题描述了如何使用标准库的 database/sql 包来访问关系数据库。

Go supports other data access technologies as well, including ORM libraries for higher-level access to relational databases, and also non-relational NoSQL data stores. Go 还支持其他数据访问技术，包括用于对关系数据库进行更高级别访问的 ORM 库，以及非关系 NoSQL 数据存储。

- **Object-relational mapping (ORM) libraries.** While the `database/sql` package includes functions for lower-level data access logic, you can also use Go to access data stores at a higher abstraction level. For more about two popular object-relational mapping (ORM) libraries for Go, see [GORM](https://gorm.io/index.html) ([package reference](https://pkg.go.dev/gorm.io/gorm)) and [ent](https://entgo.io/) ([package reference](https://pkg.go.dev/entgo.io/ent)).
- **NoSQL data stores.** The Go community has developed drivers for the majority of NoSQL data stores, including [MongoDB](https://docs.mongodb.com/drivers/go/) and [Couchbase](https://docs.couchbase.com/go-sdk/current/hello-world/overview.html). You can search [pkg.go.dev](https://pkg.go.dev/) for more.

### Supported database management systems

Go supports all of the most common relational database management systems, including MySQL, Oracle, Postgres, SQL Server, SQLite, and more.

You’ll find a complete list of drivers at the [SQLDrivers](https://github.com/golang/go/wiki/SQLDrivers) page. 数据库驱动页面

### Functions to execute queries or make database changes

The `database/sql` package includes functions specifically designed for the kind of database operation you’re executing. For example, while you can use `Query` or `QueryRow` to execute queries, `QueryRow` is designed for the case when you’re expecting only a single row, omitting the overhead of returning an `sql.Rows` that includes only one row. You can use the `Exec` function to make database changes with SQL statements such as `INSERT`, `UPDATE`, or `DELETE`.

### Transactions

Through `sql.Tx`, you can write code to execute database operations in a transaction. In a transaction, multiple operations can be performed together and conclude with a final commit, to apply all the changes in one atomic step, or a rollback, to discard them.

### Query cancellation

You can use `context.Context` when you want the ability to cancel a database operation, such as when the client’s connection closes or the operation runs longer than you want it to. 用`context.Context` 取消数据库操作，例如当客户端连接超时关闭连接。

For any database operation, you can use a `database/sql` package function that takes `Context` as an argument. Using the `Context`, you can specify a timeout or deadline for the operation. You can also use the `Context` to propagate a cancellation request through your application to the function executing an SQL statement, ensuring that resources are freed up if they’re no longer needed. 

### Managed connection pool

When you use the `sql.DB` database handle, you’re connecting with a built-in connection pool that creates and disposes of connections according to your code’s needs. A handle through `sql.DB` is the most common way to do database access with Go. For more, see [Opening a database handle](https://go.dev/doc/database/open-handle). 

The `database/sql` package manages the connection pool for you. However, for more advanced needs, you can set connection pool properties as described in [Setting connection pool properties](https://go.dev/doc/database/manage-connections#connection_pool_properties). 

For example, your code might need to:

- Make schema changes through a DDL, including logic that contains its own transaction semantics. Mixing `sql` package transaction functions with SQL transaction statements is a poor practice, as described in [Executing transactions](https://go.dev/doc/database/execute-transactions).
- Perform query locking operations that create temporary tables.
