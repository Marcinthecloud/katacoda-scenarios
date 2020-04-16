In the previous step we inserted rows into the `users` table.
We also saw how to query for the entire table contents.
As we mentioned, we avoid retrieving the entire table in production because such a query requires a full table scan, which is not performant.

First, let's describe the `users` table to remind ourselves of it's construction:

`// Make sure user_management is the default keyspace
USE user_management;
// Let's look at the table construction
DESCRIBE TABLE users;`{{execute}}

Notice that the primary key contains the `last_name`, `first_name` and `email` columns and the partition key is the `last_name` column.

<details>
  <summary style="color:teal"><b>Why did we chose these columns for the primary key?</b></summary>

  <p style="color:teal"><i>We designed this table to support a query for all users with the same last name. So, we chose `last_name` as the partition key.
  We want the results of these queries to be in alphabetical order, so we used `first_name` as a clustering column.
  It is possible that two different people could have the same first and last name. We need the rows for these two people to be different, so we added the `email` column as a clustering column to make each row unique.</i></p>

</details>

Let's look at how to retrieve a single partition using the `WHERE` clause:

`SELECT * FROM users
  WHERE last_name = 'Smith';`{{execute}}

Now, you try it. Retrieve the partition for those with a last name of Jones:

<details>
  <summary style="color:teal">Solution</summary>

`SELECT * FROM users
  WHERE last_name = 'Jones';`{{execute}}

</details>

Suppose we don't want to retrieve all the columns from the row. Instead, let's only retrieve the `address` column:

`SELECT address FROM users
  WHERE last_name = 'Smith';`{{execute}}

You try it. Retrieve the `email` column for those with a last name of Jones.

<details>
  <summary style="color:teal">Solution</summary>

`SELECT email FROM users
  WHERE last_name = 'Jones';`{{execute}}

</details>

If we want multiple fields, we can use a comma-separated list:

`SELECT first_name, email FROM users
  WHERE last_name = 'Jones';`{{execute}}

Formulate a query to retrieve the `first_name`, `email` and `address` columns for those with a last name of Smith.

<details>
  <summary style="color:teal">Solution</summary>

`SELECT first_name, email, address FROM users
  WHERE last_name = 'Smith';`{{execute}}

</details>

What if we want to retrieve all users with a specific first name? We might anticipate this would work:

`SELECT * FROM users
  WHERE first_name = 'Alex';`{{execute}}

But, we see that this query does _not_ work. This is because we tried to query without the partition key.

<details>
  <summary style="color:teal"><b>Why must queries include the partition key?</b></summary>

  <p style="color:teal"><i>Cassandra hashes the partition key to locate the partition within the cluster.
  Hashing is very fast, which is what makes Cassandra scale so well.
  Cassandra stores all rows with the same partition key in the same partition.
  So, without the partition key, Cassandra would have to do a full table scan to locate the specified rows.
  Such a scan on a production table would not be performant.</i></p>

</details>

So, we see that a query without using the partition key doesn't work. Let's try another query:

`SELECT * FROM users
  WHERE last_name = 'Smith'
  AND email = 'asmith@gmail.com';`{{execute}}

We see that this query doesn't work either because when we use clustering columns, we must use them in the order specified when we created the table.

<details>
  <summary style="color:teal"><b>Why must the clustering columns be in order for queries?</b></summary>

  <p style="color:teal"><i>Cassandra stores the rows of a partition ordered by the clustering columns.
  This means Cassandra sorts the rows of the partition initially on the first clustering column, and then on the second and so forth.
  Therefore, queries that use the clustering columns out of order would require a full scan of the partition, which could be slow for larger partitions.</i></p>

</details>

Now that we understand the rules for using clustering columns within queries, let's see if you can formulate a query for the email and address of the user whose last name is Smith and first name is Bailey.

<details>
  <summary style="color:teal">Solution</summary>

`SELECT email, address FROM users
  WHERE last_name = 'Smith'
  AND first_name = 'Bailey';`{{execute}}

</details>

<br>
## Great! If you made it successfully to the end of this step, then you understand CQL queries.
