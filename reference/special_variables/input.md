# input

A dictionary representing the request that is passed to an integration.&#x20;

| Property | Description                                                                                                                                                                                                       |
| -------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| payload  | Dictionary of payload values passed in the body of the request                                                                                                                                                    |
| method   | The HTTP method used to call the integration (e.g. `POST`, `GET`, etc.)                                                                                                                                           |
| params   | Dictionary of query parameters present on the request                                                                                                                                                             |
| path     | <p>Dictionary of path parameters present on the request<br><br>Note: Path parameters are defined in an integration's <a href="../web-service-endpoints.md">web service endpoint(s)</a></p>                        |
| fullpath | Full path of the integration's execution URL, regardless of whether it or a web service endpoint was used to call the integration                                                                                 |
| headers  | Dictionary of headers present on the request                                                                                                                                                                      |
| files    | Dictionary of files present on a `multipart`request. Key is the name of the file, value is a handle to the [GlyueFile](../special_functions/open_glyuefile.md) that represents the file with Integration Gateway. |
| user     | Object containing the `id`, `username`, and `email`of the user who called the integration                                                                                                                         |



Common usage is:

```
# Retrieve value from payload
input.payload.some_value

# Check method on request
if (input.method == "POST"):
    # Do something
    
# Retrieve customer ID from path params
cust_id = input.params["cust_id"]
```

`input` can be referenced in any formula block / lifecycle hook, including the `value` column of a field mapping.&#x20;
