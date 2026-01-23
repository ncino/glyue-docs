# Workflows

A workflow is primary organization unit within Integration Gateway ETL. A workflow comprises:

* A source data connection
* A destination data connection
* A set of transformations

### Workflow Connections

When configuring a workflow, you must select the specific record(s) being read from the source connection as well as the specific record(s) being written to / modified on the destination connection.

For example, in an SFTP --> HubSpot workflow, you must select the file on the SFTP server to read from, and the HubSpot object to write to.

<figure><img src="../.gitbook/assets/Screenshot 2025-03-20 at 11.37.02 AM.png" alt=""><figcaption><p>Selecting source and destination connections for a workflow</p></figcaption></figure>

### Transformations

Workflows exist to perform transformations on data from the source system, then write that data into the destination system. A transformation consists of:

* A source field
* A destination field
* A transformation type
* If requirement, additional details on the transformation type

<figure><img src="../.gitbook/assets/Screenshot 2025-03-20 at 11.38.31 AM.png" alt=""><figcaption><p>Example transformations</p></figcaption></figure>

Records from the source system are transformed sequentially, and all transformations are applied to a given record. If a transformation encounters an error, Integration Gateway ETL will note the error and continue with the remaining transformations and records.



Integration Gateway ETL supports the following transformation types:

| Transformation Type | Purpose                                                                        | Parameters                                                                                                     |
| ------------------- | ------------------------------------------------------------------------------ | -------------------------------------------------------------------------------------------------------------- |
| One-to-One          | Passes the value from the source directly, without modification                | None                                                                                                           |
| Boolean             | Converts values into boolean (true/false) values recognized by the destination | A list of values that should evalute to `true` (all other values will evaluate to `false` )                    |
| Date                | Specify the date formats.                                                      | The format the source date is in, and the desired output format for the date                                   |
| Python              | Custom logic and processing. Can combine values from multiple source columns.  | Python code to be executed. See [Python Transformations](workflows.md#python-transformations) for more details |
| Value Mapping       | Maps from one set of predefined values to another.                             | The Value Mapping Set to be applied.                                                                           |

### Python Transformations

A Python transformation can access any column(s) from a row. The `record_value` dictionary contains the row's values. Specific values can be accessed by name, e.g. `record_value['my_column']`&#x20;

If your transformation spans more than one line or does not evaluate to a value, you must specify the value to be used by assigning to the `retvalue` variable. For example,

```
if record_value["count"] > 1:
   retvalue = record_value["product_name"] + "s"
else:
   retvalue = record_value["product_name"]
```

