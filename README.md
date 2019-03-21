# The Instinctive Notes API (Beta)
This is the official API for Instinctive Notes. Instinctive Notes is a clinical note taking service for chiropractors in Australia.

This API is in beta and may change significantly during this period.

The Instinctive Notes API is based on REST principles. This means you can use any HTTP client and any programming language to interact with the API.

## Security
The Instinctive Notes API is served over HTTPS to ensure data security and privacy.

HTTP is not supported and will be redirected to HTTPS automatically.

Ensure that the HTTP client is up-to-date and has the latest TLS, cipher suites and SNI available.

## Base URL
All URLs in this documenation will use the following base

```
https://au.instinctivenotes.com/api/v2
```

## Authentication
The Instinctive Notes API uses Token authentication in the HTTP Authorization header. This is secure as all requests are via HTTPS.

Each Instinctive Notes Account can generate API Keys which are used for authentication.

You should provide the Instinctive Notes API Key in the `Authorization` header with the following structure:

```
Token token="xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"
```

While the API is in Beta please send requests for API use to info@instinctivenotes.com and we will be in touch to discuss what you are looking to do with the API.

## Identifying your application
To identify your application, you need to send the User-Agent header. In the event of an issue, this allows us to easily track down your requests and contact you. This should be in the form:

`APP_VENDOR_NAME (APP_VENDOR_EMAIL)`

**APP_VENDOR_NAME** is the name of your application

**APP_VENDOR_EMAIL** is a contact email address for you or your company

## Errors
Conventional HTTP response codes are used to indicate API errors.

General code rules apply:

- 2xx range indicate success.
- 4xx range indicate an error resulting from the provided information (eg. missing a required parameter)
- 5xx range indicate an error with our Instinctive Notes servers

## Making a request
At a minumum requests must contain:

- The request URL
- The `Authorization` header with the Instinctive Notes users API Key
- The `Accept` header with `application/json`
- The `User Agent` header with your details

The following is a curl request for all soap_notes.

```
curl https://au.instinctivenotes.com/api/v2/soap_notes \
  -H 'Authorization: Token token="API_KEY"' \
  -H 'Accept: application/json' \
  -H 'User-Agent: APP_VENDOR_NAME (APP_VENDOR_EMAIL)'
```

## Response data format
All responses are sent as JSON.

## Dates and times
All dates and times in a resppnse are in the UTC timezone and sent in iso8601 format.

All date or datetime parameters should be converted to UTC timezone and sent in iso8601 format.

Eg: `2019-02-26T06:05:51Z`

## Pagination
Requests that return multiple items will be limited to 100 items per page. You can specify further pages with the page parameter.

All responses will include a `meta` entry which will include item counts and page links.

It is recommended to follow the page links provided instead of constructing your own URLs.

```
"soap_notes": {
  ...
},
"meta": {
  "page_items": 100,
  "total_items": 250,
  "pages": {
    "self":"https://au.instinctivenotes.com/api/v2/soap_notes?page=2",
    "first":"https://au.instinctivenotes.com/api/v2/soap_notes?page=1",
    "last":"https://au.instinctivenotes.com/api/v2/soap_notes?page=3",
    "next":"https://au.instinctivenotes.com/api/v2/soap_notes?page=3",
    "previous":"https://au.instinctivenotes.com/api/v2/soap_notes?page=1"
  }
}
```

The possible pagination links are:

`self` Shows the URL of the current page of results.

`first` Shows the URL of the first page of results.

`last` Shows the URL of the last page of results.

`next` Shows the URL of the immediate next page of results.

`previous` Shows the URL of the immediate previous page of results.

The pagination links will only be included if they are relevant (eg. there will be no next link if you are on the last page).

## Filtering results
Some resources allow the results to be filtered. This will be documented with the resource if it is available.

### DateTime Filter Operators

- `>=` Greater than or equal to
- `>` Greater than
- `<=` Less than or equal to
- `<` Less than

### Sending Filter Parameters
To filter a resource, send a filter string as the q parameter:

`https://au.instinctivenotes.com/api/v2/soap_notes?q=updated_at:>2019-01-29T11:59:59Z`

### Example request (greater than)

```
curl https://au.instinctivenotes.com/api/v2/soap_notes?q=updated_at:>2019-01-29T11:59:59Z \
  -H 'Authorization: Token token="API_KEY"' \
  -H 'Accept: application/json' \
  -H 'User-Agent: APP_VENDOR_NAME (APP_VENDOR_EMAIL)'
```

### Format rules
Dates and datetimes must be sent in UTC time_zone and sent as iso8601

Eg: `2019-01-29T11:59:59Z`

### Filtering tips
For index endpoints you can get records that have been updated since a certain time by sending a filter for updated_at

Eg: `q=updated_at:>2019-01-29T11:59:59Z`

This is highly recommended to reduce the number of calls and data returned.

## API Resources

- SOAP Notes

## Questions
If you have any questions send us an email to info@instinctivenotes.com
