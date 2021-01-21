# SurfReport

## Section 1: Resource Description

- 1 - 3 sentences, usually starts with a verb
- Given example: "Contains information about surfing conditions, including the
  surf height, water temperature, wind, and tide. Also provides an overall
  recommendation about whether to go surfing."
- My attempt: "Provides information about surfing conditions like surf height,
  wind, water temperature, tide, and a recommendation of whether or not to go
  surfing. Riptide conditions are not included."

## Section 2: Endpoints

\*Endpoints indicate how to access the resource, method indicates allowed
interactions (GET, POST, etc.) with the resource

- One resource will have a variety of endpoints with different paths and methods
  (will return different info)
- Most important part of documenting so distinguish it somehow from everything
  else
- Should include a path (GET surfreport/{beachid}) and a short description of
  what information is there

* Path params are represented with curly braces (e.g.
  /campaigns/{campaign_id}/actions/send)
  - Can also use another color to distinguish these params

- Endpoint lists the end path only (what comes after the base URL) -> Base URL
  and endpoint together are a resource URL
  - An intro section will explain the full url and required authorization
- Endpoints can be grouped by method or the type of information returned
  - Can list on separate pages, on the same page, in different sections by
    method, etc. depending on how much you have to say about each endpoint, what
    makes sense for that resource

GET surfreport/{beachId} Gets the surf conditions for a specific beach ID.

## Section 3: Parameters

- Params are options passed with endpoint (e.g. response format or amount of
  info returned)
- Types of params:
  - Header params: Included in request header, usually related to authorization
    - Not usually documented with each endpoint but in the authorization
      requirements section
    - If endpoint requires unique params to be in the header document them with
      the endpoint
  - Path params: Included in path of endpoint before the query string, usually
    set off with curly braces
    - Part of the endpoint so not optional and order is important
    - Indicated default values, allowed values, and other details
  - Query string params: Included in the query string of the endpoint, after the
    ?, listed with & between
    - Order doesn't matter
    - e.g. /surfreport/{beachId}?days=3&units=metric&time=1400
- Types are often documented in separate groups on the same page
  - Not all endpoints contain all types of params
- Often listed in a table like this:

| Parameter | Required/Optional | Data Type |
| --------- | ----------------- | --------- |
| format    | Optional          | String    |

- Closely related to params and used to be referred to as a param is the
  requestBody

  - Usually only used with CREATE/PUT methods and includes JSON object in the
    body of the request

- Regardless of param type always define the following:

  - Data type, most common are:
    - String - alphanumeric sequence of letters/numbers
    - Integer - whole number, positive or negative
    - Boolean - true/false
    - Object - Key-value pairs in JSON format
    - Array - List of value
      - Depending on the language it's important to use really specific types,
        Java for instance allocates mem based on types (this is usually not
        necessary with REST APIs but note it if you know the specific types)
  - Max and min value, indicate restricted values if appropriate
    - Not required for all params such as:
      - Booleans (only true/false so no need for max/min)
      - Strings that use enums (If string is restricted to an enumerated list,
        max/min are not necessary. E.g if only north, south, east, west are
        allowed there is no max or min value)
    - Need to consider ways the API could be broken (e.g. try entering an id of
      300 characters or submitting a file that is 80 MB)
      - QA team should know what limitations exist since it's their job to test
        systems

- When testing an API try running endpoint without required params or wrong
  params or values that exceed max/min
  - See what errors are and include that response in status and error codes
    section

#### Path Parameters

| Path parameter | Description                                                                                                     |
| -------------- | --------------------------------------------------------------------------------------------------------------- |
| {beachId}      | The value for the beach you want to look up. Valid beachId values are available from our site at sampleurl.com. |

#### Query String Parameters

| Query String Parameter | Required/Optional | Description                                                                     | Type                                         |
| ---------------------- | ----------------- | ------------------------------------------------------------------------------- | -------------------------------------------- |
| days                   | Optional          | The number of days to include in the response. Default is 3.                    | Integer                                      |
| time                   | Optional          | If you include the time only the current hour will be returned in the response. | Integer. Unix format (ms since 1970) in UTC. |

## Section 4: Request Example

- Sample request using the endpoint and some configured params
  - Doesn't show all possible param configurations but should use as many params
    as possible
  - Sometimes include code snippets to show same request in various languages
    besides curl
    - curl is common for showing requests because it is language agnostic, shows
      header info required, and shows the method used
    - Should always provide a curl example, other languages are bonus and often
      unnecessary...there are so many languages to consider so it is usually
      left up to users to figure out their own queries
      - However, this can make the api easier to implement (use tools like
        Postman, readme.com, or swaggerhub to create snippets) --> Check samples
        with dev team
      - Check with product managers what languages target users are developing
        in to focus code samples
      - If a language specific SDK is being included it's less important to have
        code samples since the SDK will make it easier to use. Still good to
        have snippets in the language of the SDK
    - Add backslashes to separate params into their own lines
- If there are a lot of params use multiple examples to show possible
  combinations
  - This makes sense if the params wouldn't be used together
- API explorers let users make requests from the documentation using inserted
  values to see the response
  - Can be tricky if the data being retrieved is not public or the API key is
    restricted
  - This can also cause problems if users submit test data or delete information
    - GET methods are safe but other methods can end up with accidentally
      corrupted data
      - Can also include a warning

Sample request curl -I -X GET
"https://api.openweathermap.org/data/2.5/surfreport?zip=95050&appid=APIKEY&units=imperial&days=2"
(replace APIKEY with your actual key)

## Section 5: Response Example and Schema

- Show sample response from the request example and response schema to define
  all possible elements in the response
  - Not comprehensive of all parameter configurations or operations but should
    correspond with params passed in request example
  - Lets users know if the resource contains info they want, format, and how
    info is structured and labeled
- Response schema is a description of the response, listing each property that
  could possibly be returned, what each property contains, data format of the
  values, structure, and other details
  - This is sometimes omitted if the information is self-evident or intuitive
    but it's best to include it
- Sometimes good to include header information in the example if it provides
  unique information other than status codes
- Values should be realistic without being real (shouldn't use distracting
  values such as comic book character names)
  - Do not use real customer data (double check info received from engineers to
    be sure they didn't scrape the db)
- Make sure JSON is formatted well, add syntax highlighting if possible

Sample Response

```json
{
    "surfreport": [
        {
            "beach": "Santa Cruz",
            "monday": {
                "1pm": {
                    "tide": 5,
                    "wind": 15,
                    "watertemp": 80,
                    "surfheight": 5,
                    "recommendation": "Go surfing!"
                },
                "2pm": {
                    "tide": -1,
                    "wind": 1,
                    "watertemp": 50,
                    "surfheight": 3,
                    "recommendation": "Surfing conditions are okay, not great."
                },
                "3pm": {
                    "tide": -1,
                    "wind": 10,
                    "watertemp": 65,
                    "surfheight": 1,
                    "recommendation": "Not a good day for surfing."
                }
                ...
            }
        }
    ]
}
```

Response Definitions

| Response item               | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           | Data type |
| --------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------- |
| beach                       | The beach you selected based on the beach ID in the request. The beach name is the official name as described in the National Park Service Geodatabase.                                                                                                                                                                                                                                                                                                                                                               | String    |
| {day}                       | The day of the week you selected. A max of 3 days gets returned in the response.                                                                                                                                                                                                                                                                                                                                                                                                                                      | Object    |
| {time}                      | The time for the conditions. This item is included only if you include a time param in the request                                                                                                                                                                                                                                                                                                                                                                                                                    | String    |
| {day}/{time}/tide           | The level of tide at the beach for a specific day and time. Tide is the distance inland that the water rises to, and can be a positive or negative number. When the tide is out, the number is negative. When the tide is in, the number is positive. The 0 point reflects the line when the tide is neither going in nor out but is in transition between the two states.                                                                                                                                            | Integer   |
| {day}/{time}/wind           | The wind speed at the beach, measured in knots (nautical miles per hour). Wind affects the surf height and general wave conditions. Wind speeds of more than 15 knots make surf conditions undesirable because the wind creates white caps and choppy waters.                                                                                                                                                                                                                                                         | Integer   |
| {day}/{time}/watertemp      | The temperature of the water, returned in Fahrenheit or Celsius depending upon the units you specify. Water temperatures below 70 F usually require you to wear a wetsuit. With temperatures below 60, you will need at least a 3mm wetsuit and preferably booties to stay warm.                                                                                                                                                                                                                                      | Integer   |
| {day}/{time}/surfheight     | The height of the waves, returned in either feet or centimeters depending on the units you specify. A surf height of 3 feet is the minimum size needed for surfing. If the surf height exceeds 10 feet, it is not safe to surf.                                                                                                                                                                                                                                                                                       | Integer   |
| {day}/{time}/recommendation | An overall recommendation based on a combination of the various factors (wind, watertemp, surfheight). Three responses are possible: (1) "Go surfing!", (2) "Surfing conditions are okay, not great", and (3) "Not a good day for surfing." Each of the three factors is scored with a maximum of 33.33 points, depending on the ideal for each element. The three elements are combined to form a percentage. 0% to 59% yields response 3, 60% - 80% and below yields response 2, and 81% to 100% yields response 1. | String    |
