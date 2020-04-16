The CQL `DELETE` command lets you delete values, both individual column values as well as entire rows.

Let's start by deleting the `address` value for Chris Jones.

`DELETE address FROM users
  WHERE last_name = 'Jones'
  AND first_name = 'Chris'
  AND email = 'cjones@gmail.com';`{{execute}}

Retrieve the Jones partition to verify the changes.

<details>
  <summary style="color:teal">Solution</summary>

`SELECT * FROM users
  WHERE last_name = 'Jones';`{{execute}}

</details>

Now you try it. Delete the address value for Alex Smith and verify the change.

<details>
  <summary style="color:teal">Solution</summary>

`// Delete the address value for Alex Smith
DELETE address FROM users
  WHERE last_name = 'Smith'
  AND first_name = 'Alex'
  AND email = 'asmith@gmail.com';
// Select all the Smiths to verify the delete
SELECT * FROM users
  WHERE last_name = 'Smith'
  AND first_name = 'Alex';`{{execute}}

</details>

We can delete the entire row by not specifying any values. Notice that we also can omit the `email` specifier from the `WHERE` clause. This is because Cassandra allows you to delete multiple rows with a single statement.

`DELETE FROM users
  WHERE last_name = 'Jones'
  AND first_name = 'Drew';`{{execute}}

Retrieve the Jones partition to see the results.

<details>
  <summary style="color:teal">Solution</summary>

`SELECT * FROM users
  WHERE last_name = 'Jones';`{{execute}}

</details>

What happens when we try to delete a clustering column value?

`DELETE email FROM users
  WHERE last_name = 'Smith'
  AND first_name = 'Alex'
  AND email = 'asmith@gmail.com';`{{execute}}

We see that this doesn't work.
Cassandra requires values for all primary key columns, so you can't delete one of those columns.

<details>
  <summary style="color:teal"><b>Why does Cassandra require values for all the primary key columns?</b></summary>

  <p style="color:teal"><i>Although Cassandra supports sparse columns, this only applies to non-primary key values.
  Cassandra requires the partition key columns to locate the partition.
  Cassandra requires the clustering columns to order the rows within a partition.
  Therefore, removing any of the primary key columns would not allow Cassandra to operate correctly.</i></p>

</details>

As a final test of you mastery of the `DELETE` command, see if you can delete the entire Smith partition in a single command.

<details>
  <summary style="color:teal">Solution</summary>

`DELETE FROM users
  WHERE last_name = 'Smith';`{{execute}}

</details>

Verify the `DELETE` by querying for the Smith partition.

<details>
  <summary style="color:teal">Solution</summary>

`SELECT * FROM users
  WHERE last_name = 'Smith';`{{execute}}

</details>

<br>
## Outstanding! Now you know about the CQL `DELETE` command
