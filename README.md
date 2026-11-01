## Evidence of each action

### Get (with new pizza)

https://localhost:7014/Pizza> get
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8
Date: Sat, 31 Oct 2026 21:55:25 GMT
Server: Kestrel
Transfer-Encoding: chunked

[
{
"id": 1,
"name": "Classic Italian",
"isGlutenFree": false
},
{
"id": 2,
"name": "Veggie",
"isGlutenFree": true
},
{
"id": 3,
"name": "Supreme",
"isGlutenFree": false
}
]

### Get by id 3

https://localhost:7014/Pizza> get 3
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8
Date: Sat, 31 Oct 2026 21:56:18 GMT
Server: Kestrel
Transfer-Encoding: chunked

{
"id": 3,
"name": "Supreme",
"isGlutenFree": false
}

### Post

https://localhost:7014/Pizza> post -c "{"name":"Hawaii","isGlutenFree":false}"  
HTTP/1.1 201 Created
Content-Type: application/json; charset=utf-8
Date: Sat, 31 Oct 2026 21:59:29 GMT
Location: https://localhost:7014/Pizza/4
Server: Kestrel
Transfer-Encoding: chunked

{
"id": 4,
"name": "Hawaii",
"isGlutenFree": false
}

### Put

https://localhost:7014/Pizza> put 4 -c "{"id":4, "name":"Hawaiian", "isGlutenFree":true}"
HTTP/1.1 204 No Content
Date: Sat, 31 Oct 2026 21:59:59 GMT
Server: Kestrel

### Delete

https://localhost:7014/Pizza> delete 4  
HTTP/1.1 204 No Content
Date: Sat, 31 Oct 2026 22:00:15 GMT
Server: Kestrel

## Work with files and directories

Summary function:

```
void createSalesSummaryFile(string salesSummaryDir, IEnumerable<string> salesFiles, double salesTotal, string currentDirectory)
{
StringBuilder summary = new StringBuilder($"Sales Summary{Environment.NewLine}");
    summary.Append($"---------------------------{Environment.NewLine}");
summary.Append($"Total Sales: {salesTotal:C}{Environment.NewLine}{Environment.NewLine}");
    summary.Append($"Details:{Environment.NewLine}");

    foreach (var file in salesFiles)
    {
        // Read the contents of the file
        string salesJson = File.ReadAllText(file);

        // Parse the contents as JSON
        SalesData? data = JsonConvert.DeserializeObject<SalesData?>(salesJson);
        var total = data?.Total ?? 0;
        if (total > 0)
        {
            // Add the amount found in the Total field to the salesTotal variable
            //var salesFileName = Path.GetFileName(file);
            var salesFileName = Path.GetRelativePath(currentDirectory,file);
            summary.Append($"{salesFileName}: {data?.Total ?? 0:C}");
            summary.Append(Environment.NewLine);
        }
    }

    string fileName = Path.Combine(salesSummaryDir, "summary.txt");

    File.WriteAllText(fileName,summary.ToString());

}
```
