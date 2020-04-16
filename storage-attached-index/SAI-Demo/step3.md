When we want to change column values we use the CQL `UPDATE` command.

Here's an example of how to change Alex Smith's address:

`UPDATE users
  SET address = '987 Main St.'
  WHERE last_name = 'Smith'
  AND first_name = 'Alex'
  AND email = 'asmith@gmail.com';`{{execute}}

  ---

  <p><span  style="color:teal">***NOTE:***</span> *The `WHERE` clause specifies the row or rows to be updated. To specify a row, the `WHERE` clause must provide a value for each column of the row's primary key. To specify more than one row, you can use the `IN` keyword to introduce a list of possible values. You can only do this for the last column of the primary key.*</p>

  ---

Now, retrieve the modified row to verify the change.

<details>
  <summary style="color:teal">Solution</summary>

`SELECT * FROM users
  WHERE last_name = 'Smith'
  AND first_name = 'Alex'
  AND email = 'asmith@gmail.com';`{{execute}}

</details>

Now, you try it! Recall that Chris Jones doesn't have an `address` column value.
Set the address to 789 Cherry St.

<details>
  <summary style="color:teal">Solution</summary>

`UPDATE users
  SET address = '789 Cherry St.'
  WHERE last_name = 'Jones'
  AND first_name = 'Chris'
  AND email = 'cjones@gmail.com';`{{execute}}

</details>

Retrieve the row to verify the change.

<details>
  <summary style="color:teal">Solution</summary>

`SELECT * FROM users
  WHERE last_name = 'Jones'
  AND first_name = 'Chris'
  AND email = 'cjones@gmail.com';`{{execute}}

</details>

Next, let's try to change the email for Alex Smith to `alexsmith@gmail.com`.

`UPDATE users
  SET email = 'alexsmith@gmail.com'
  WHERE last_name = 'Smith'
  AND first_name = 'Alex'
  AND email = 'asmith@gmail.com';`{{execute}}

We see this doesn't work! Cassandra will not let you change primary key column values.

<details>
  <summary style="color:teal"><b>Want to know why you can't change primary key column values?</b></summary>

  <p style="color:teal"><i>There are two parts to the primary key: the partition key columns and the clustering columns.
  Since Cassandra hashes the partition key to find the partition, changing a partition key column would require removing the row from the old partition and adding it to a new partition.
  If you really need to do this, you can handle this at the application level - or at least you'll be able to after you complete the next step where you will learn about `DELETE` :).
  Similarly, changing a clustering column would require Cassandra to reorder the rows in the partition. Again, if you need to do this, you will handle it in the application by deleting the row and re-adding it with the new primary key values.</i></p>

</details>

Finally, let's update a non-existent row:

`UPDATE users
  SET address = '987 Elm St.'
  WHERE last_name = 'Jones'
  AND first_name = 'Drew'
  AND email = 'djones@gmail.com';`{{execute}}

Surprisingly, this seemed to work without errors???!!!
Query for all users in the Jones partition to see the results.

<details>
  <summary style="color:teal">Solution</summary>

`SELECT * FROM users
  WHERE last_name = 'Jones';`{{execute}}

</details>

The `UPDATE` command created the row.
This is called an _upsert_.
You can also use the `INSERT` command to update an existing row, which in another form of an upsert.

<details>
  <summary style="color:teal"><b>Why does Cassandra perform upserts?</b></summary>

  <p style="color:teal"><i>Cassandra is built for speed.
  As a result, Cassandra does not read a row before writing the row.
  So, an `UPDATE` doesn't check to see if the row exists before changing it.
  Instead, Cassandra just writes the values.
  Similarly, the `INSERT` command doesn't check to see if the row exists before writing.
  So, if the row does exist, the `INSERT` command logically writes over the previous values.</i></p>

</details>

<br>
## Superb! You now know how to update rows!
