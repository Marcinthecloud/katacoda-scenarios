Test your understanding by answering the following questions.


>>Which of the following are true about the CQL <code>INSERT</code> statement? (check all that apply)<<
[*] The column <code>VALUES</code> must follow the order specified earlier in the <code>INSERT</code> command
[ ] <code>INSERT</code> cannot be used to update values
[*] <code>INSERT</code> commands require values for all the primary key columns
[*] Since Cassandra supports sparse columns, non-primary key values are optional

>>Which of the following are true about the CQL <code>SELECT</code> statement? (check all that apply)<<
[ ] Selecting all rows in a table is a common practice for large production tables
[*] <code>SELECT</code> statements require specifying all partition key columns
[ ] <code>SELECT</code> statements require specifying all primary key columns
[*] <code>SELECT</code> statements can retrieve entire partitions as well as individual rows

>>Which of the following are true about the CQL <code>UPDATE</code> statement? (check all that apply)<<
[ ] Updates never insert and inserts never update
[*] <code>UPDATE</code> statements always require a <code>WHERE</code> clause
[*] <code>UPDATE</code> statements require the WHERE clause to specify all values of the primary key
[ ] <code>UPDATE</code> statements can update clustering columns

>>Which of the following are true about the CQL <code>DELETE</code> statement? (check all that apply)<<
[ ] The <code>DELETE</code> statement deletes entire tables
[*] <code>DELETE</code> can delete payload column values from rows
[*] <code>DELETE</code> can delete entire rows
[ ] <code>DELETE</code> statements can delete partition key column values since these values are only a <i>part</i> of the primary key
[*] <code>DELETE</code> can delete an entire partition by only specifying the partition key value(s) in the <code>WHERE</code> clause
