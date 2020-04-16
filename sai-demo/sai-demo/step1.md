The _C_ in CRUD stands for _Create_. In CQL we use the `INSERT` command to create rows.

Let's start by:
- Creating a keyspace named `user_management`
- Setting `user_management` as the default keyspace
- Creating a table we can work with named 'users'

Execute the following:

`// Create the user_management keyspace
CREATE KEYSPACE user_management
  WITH REPLICATION = {
    'class' : 'SimpleStrategy',
    'replication_factor' : 1
  };
// Set user_management as the default keyspace
USE user_management;
// Create the example users table
CREATE TABLE users (
  last_name   text,
  first_name  text,
  email       text,
  address     text,
  PRIMARY KEY((last_name), first_name, email)
);`{{execute}}

Here are the highlights of this table:
- Four text columns: `last_name`, `first_name`, `email` and `address`
- The partition key is the `last_name` column
- The `first_name` and `email` columns are clustering columns
- The `address` column is a payload column - payload columns just go along for the ride

Let's put a row in the table with the following values:

| Column Name | Value           |
|:-----------:|:---------------:|
|last_name    |Smith            |
|first_name   |Alex             |
|email        |asmith@gmail.com |
|address      |123 Main st.     |

Here's the CQL statement to add this row to the table:

`INSERT INTO users
  (last_name, first_name, email, address)
  VALUES(
    'Smith',
    'Alex',
    'asmith@gmail.com',
    '123 Main st.');`{{execute}}

To see the results of the statement, let's retrieve all the rows in the table:

`SELECT * FROM users;`{{execute}}

---

<p><span  style="color:teal">***NOTE:***</span> *The previous unconstrained query requires a full table scan. We would not advise using this type of query on production databases because these queries require a full table scan, which can impact performance. However, for educational purposes on small tables, it's appropriate.*</p>

---

You see the row you created.
Now that you've seen the `INSERT` command, try it out. Insert a row with the following values:

| Column Name | Value           |
|:-----------:|:---------------:|
|last_name    |Smith            |
|first_name   |Bailey           |
|email        |bsmith@gmail.com |
|address      |234 Elm st.      |

<details>
  <summary style="color:teal">Solution</summary>

`INSERT INTO users
  (last_name, first_name, email, address)
  VALUES(
    'Smith',
    'Bailey',
    'bsmith@gmail.com',
    '234 Elm st.');`{{execute}}

</details>

Retrieve all the rows from the `users` table to verify your insert worked as expected.

<details>
  <summary style="color:teal">Solution</summary>

  `SELECT * FROM users;`{{execute}}

</details>


---

<p><span  style="color:green">***ProTip:***</span> *You can use the tab key for command completion and to show you command options. Also, you can scroll through the cqlsh command history using the up/down arrow keys.*</p>

---

Cassandra supports sparse columns, which means non-primary key columns don't have to have a value.
Try creating a row with the following values (note the missing `address` column):

| Column Name | Value           |
|:-----------:|:---------------:|
|last_name    |Jones            |
|first_name   |Chris            |
|email        |cjones@gmail.com |


<details>
  <summary style="color:teal">Solution</summary>

`INSERT INTO users
  (last_name, first_name, email)
  VALUES(
    'Jones',
    'Chris',
    'cjones@gmail.com');`{{execute}}

</details>

<br>

Now, verify the contents of the table again to see how Cassandra displays sparse columns.

<details>
  <summary style="color:teal">Solution</summary>

`SELECT * FROM users;`{{execute}}

</details>

<br>
## Congratulations! You know how to insert rows using CQL!
