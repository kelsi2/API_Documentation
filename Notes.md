- curl tip: End request with | python -m json.tool to un-minify response
  - Include -i to view response header, -I to view only the header
  * curl -X GET http://example.com -I will do a GET request (default but
    explicit here with -X) for the header (-I)
    - Can use any of the methods below after the -X to perform different
      requests
  * Use -H to include headers (one for each key value pair) e.g. curl -X GET -H
    "Cache-Control: no-cache" -H "Postman-Token:
    930d08d6-7b2a-6ea2-0725-27324755c684"
    "https://api.openweathermap.org/data/2.5/weather?zip=95050&appid=APIKEY&units=imperial"

| HTTP Method | Description       |
| ----------- | ----------------- |
| POST        | Create a resource |
| GET         | Read a resource   |
| PUT         | Update a resource |
| DELETE      | Delete a resource |

- Query string params are passed to the endpoint after the ? and each param is
  separated by an & (?zip=95050&appid=APIKEY&units=imperial)
  - The order of params doesn't matter unless they are part of the path params
    rather then the query params (before the ? they are considered part of the
    URL)

Common curl commands:

| curl command    | Description                                                                                                                                                                                                                                            | Example                                                     |
| --------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ----------------------------------------------------------- |
| -i or --include | Includes the response headers in the response.                                                                                                                                                                                                         | curl -i http://www.example.com                              |
| -d or --data    | Includes data to post to the URL. The data needs to be url encoded. Data can also be passed in the request body.                                                                                                                                       | curl -d "data-to-post" http://www.example.com               |
| -H or --header  | Submits the request header to the resource. Headers are common with REST API requests because the authorization is usually included in the header.                                                                                                     | curl -H "key:12345" http://www.example.com                  |
| -X POST         | Specifies the HTTP method to use with the request (in this example, POST). If you use -d in the request, curl automatically specifies a POST method. With GET requests, including the HTTP method is optional, because GET is the default method used. | curl -X POST -d "resource-to-update" http://www.example.com |
| @filename       | Loads content from a file.                                                                                                                                                                                                                             | curl -X POST -d @mypet.json http://www.example.com          |
