# parameterization in SQL with System.Data.SqlClient in C#

Step 1:

write a SQL Query String with paramater (the parameter must be defined as `@` followed by an identifier, case-insensitive).

Step 2:

invoke the `cmd.Parameters.AddWithValue` to set the value of parameter in SQL Query String.

## examples 
### example 1

```
						SqlCommand sqlCmd = new SqlCommand();
	
						// your sql command string.
            string sql_tmp = "SELECT　* FROM Table1 WHERE Field1 = @Field1 AND Field2 = @Field2; ";
            sqlCmd.Command.AddWithValue("@Field1",2);
            sqlCmd.Command.AddWithValue("@Field2","Value2");
						sqlCmd.CommandText = sql_tmp;
```
