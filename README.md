# Google-trends-dashboard
This dashboard shows the google trends i.e. the keywords which are mostly searched in a country using API from google trends in realtime.
I used SerpAPI (https://serpapi.com/google-trends-api) to get the data.
This is a free API that allows us to search only 5 keywords at max. We can just specify the keywords as we need.

------------

API query for Country
"let
    // Define the API endpoint
    apiUrl = "https://serpapi.com/search.json",


// Define the parameters
queryParams = [ engine = "google_trends", 
                q = Keywords , 
                data_type = "GEO_MAP", 
                date = "today 5-y", 
                tz="-330",
                api_key = "your api key"],

// Combine the endpoint and parameters
fullUrl = apiUrl & "?" & Uri.BuildQueryString(queryParams),

// Make the HTTP request
response = Web.Contents(fullUrl),

// Parse the JSON response
jsonResponse = Json.Document(response),

// Convert the response to a table
dataTable = Table.FromRecords({jsonResponse}),

// Extract the relevant data
comparedBreakdownByRegion = dataTable{0}[compared_breakdown_by_region]
in
    comparedBreakdownByRegion
"

--------------

Query for By dates
"
let
    // Define the base URL for the API call
    BaseUrl = "https://serpapi.com/search",

// Define the query parameters with engine, terms, data type, date, and time zone
QueryParams = [engine = "google_trends",
               q = Keywords,
               data_type = "TIMESERIES",
               date = "all", 
               tz = "-330",    
               api_key = "your api key"],

// Generate the full URL with query parameters
UrlWithParams = BaseUrl & "?" & Text.Combine(List.Transform(Record.FieldNames(QueryParams), 
    each _ & "=" & Uri.EscapeDataString(Record.Field(QueryParams, _))), "&"),

// Fetch data from the API
JsonResponse = Json.Document(Web.Contents(UrlWithParams)),

// Extract the "interest_over_time" part from the JSON response
InterestOverTime = JsonResponse[#"interest_over_time"]
in
    InterestOverTime
"

-----------------

Query for past 7 Days
"
let
    // Define the base URL for the API call
    BaseUrl = "https://serpapi.com/search",

// Define the query parameters with engine, terms, data type, date, and time zone
QueryParams = [engine = "google_trends",
               q = Keywords,
               data_type = "TIMESERIES",
               date = "all", 
               tz = "-330",    
               api_key = "your api key"],

// Generate the full URL with query parameters
UrlWithParams = BaseUrl & "?" & Text.Combine(List.Transform(Record.FieldNames(QueryParams), 
    each _ & "=" & Uri.EscapeDataString(Record.Field(QueryParams, _))), "&"),

// Fetch data from the API
JsonResponse = Json.Document(Web.Contents(UrlWithParams)),

// Extract the "interest_over_time" part from the JSON response
InterestOverTime = JsonResponse[#"interest_over_time"]
in
    InterestOverTime
"

--------------

Query for related keywords
"
let
    // Define the API endpoint
    apiUrl = "https://serpapi.com/search.json",

// Define the parameters
queryParams = [engine = "google_trends",
               q = Target_keyword,
               data_type = "RELATED_TOPICS",
               api_key = "your api key"],

// Combine the endpoint and parameters to create the full URL
fullUrl = apiUrl & "?" & Uri.BuildQueryString(queryParams),

// Make the HTTP request and get the response
response = Web.Contents(fullUrl),

// Parse the JSON response
jsonResponse = Json.Document(response)
in
    jsonResponse
"

------------------

Based on the data fetched by the APIs we have filtered out the data which is required for our dashboard.
