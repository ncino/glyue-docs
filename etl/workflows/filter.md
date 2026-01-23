# Filter

The Filter node enables removing certain records from the ETL process if they match a specific condition.

## Anatomy

<figure><img src="../../.gitbook/assets/ETL Filter Node (1).png" alt=""><figcaption></figcaption></figure>

1. Button to add a new filter condition.
2. A configured filter condition.
3. Button to delete a filter condition.
4. The column to filter on.
5. The operator used to perform the comparison.
6. The type the column should be treated as.
7. The value to compare to.
8. Button to switch to Python mode.

## Python Mode

In addition to the Filter Editor for configuring filter conditions, you can use custom Python to perform the filtering.

<figure><img src="../../.gitbook/assets/ETL Filter Node Custom Python.png" alt=""><figcaption></figcaption></figure>

For more information about writing and executing custom Python, check out [the documentation for the Python transformation](https://app.gitbook.com/o/hMR7ZmLUVPDLpu0EFvkY/s/1flQ2To8tQpCQWl2Ty9U/~/edit/~/changes/249/etl/workflows/transform#python).
