# The Instinctive Notes API (Beta)

This is the official API for Instinctive Notes. Instinctive Notes is a clinical note taking service for Allied Health Practices.

This API is in beta and may change significantly during this period.

The Instinctive Notes API is based on REST principles. This means you can use any HTTP client and any programming language to interact with the API.

## Security

The Instinctive Notes API is served over HTTPS to ensure data security and privacy.

HTTP is not supported and will be redirected to HTTPS automatically.

Ensure that the HTTP client is up-to-date and has the latest TLS, cipher suites and SNI available.

## Base URL

The base URL should preceed any endpoint calls made to the API.

The base URL is made up of three components:

- A 2 character shard identifier
- The API URL
- The two character version identifier

An example base URL:
```
https://au1.instinctivenotes.com/api/v2
```

### Shards

A shard is an isolated instance of Instinctive Notes. All the data for a practice will be located in a specific shard based on the location of the practice.

Each shard will be represented by one (or more) two character shard identifiers.

Current shard identifiers are:

- `au`
- `nz`

If an incorrect shard is used in the URL for an API call to Instinctive Notes the API will respond with a `401` error.

See the `API Key` section below for how to identify the shard for a specific practice.

### Versions

The API is versioned and is currently at version 2.

Typically API responses will not change field names or remove fields within a version, although fields may be added.

If a field will be removed from a future version it will be deprecated. The field will continue to be returned in this version of the API but will be designated as deprecated to allow for API clients to be updated.

The API version will be added to the end of the base URL.

## API Keys

Each practice may have one or more API Keys which provide access to all API endpoints for that practice.

Keys have two components separated by a `-`

- The secret key for the practice
- The 2 character shard

Eg: `ABsUn1ggcLazRmRXYXYgQrnc-au`

The full API Key, including the dash and the shard, should be sent with any request. See the `Authentication` section below for how to send the API Key.


## Identifying your application

You must register your application or service name with Instinctive Notes to have access to the API. This is to ensure we know who is accessing the API and who to contact if there is an issue.

To register please send an email to info@instinctive3.com and include:

- Your app/service name - all lower case with underscores for spaces
- An email contact in case we need to get in touch with something
- A one liner on what the app/service is going to do

We will let you know when your app is registered and you can access the API.

## Authentication

The Instinctive Notes API uses Token authentication in the HTTP Authorization header. This is secure as all requests are via HTTPS.

Every request should add an `Authorization` header with the following structure:

```
Token token="API_KEY-SHARD", app="APP_NAME"
```

eg

```
Token token="adi7ygkjlsdfhgjseoujk-au1", app="spinebone"
```

If the shard or app name is incorrect then a `400 - Bad Request` will be returned.

If the Api Key is incorrect then a `401 - Unauthorized` will be returned.

## Errors

Conventional HTTP response codes are used to indicate API errors.

General code rules apply:

- 2xx range indicate success.
- 4xx range indicate an error resulting from the provided information (eg. missing a required parameter)
- 5xx range indicate an error with our Instinctive Notes servers

## Making a request

At a minumum requests must contain:

- The request `URL`
- The `Authorization Token` header with the Instinctive Notes API Key, shard and app
- The `Accept` header with `application/json`

The following is a curl request for all soap_notes.

```
curl https://au1.instinctivenotes.com/api/v2/soap_notes \
  -H 'Authorization: Token token="API_KEY-SHARD", app="APP_NAME' \
  -H 'Accept: application/json' \
```

## Response data format

All responses are sent as JSON.

## Dates and times

All dates and times in a response are in the UTC timezone and sent in iso8601 format.

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
    "self":"https://au1.instinctivenotes.com/api/v2/soap_notes?page=2",
    "first":"https://au1.instinctivenotes.com/api/v2/soap_notes?page=1",
    "last":"https://au1.instinctivenotes.com/api/v2/soap_notes?page=3",
    "next":"https://au1.instinctivenotes.com/api/v2/soap_notes?page=3",
    "previous":"https://au1.instinctivenotes.com/api/v2/soap_notes?page=1"
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

Some resources allow the results to be filtered. Filter fields will be documented with the resource where available.

### Boolean Filter Operators

- `=` Equal to

Pass `true` or `false` only

### Date Filter Operators

- `=` Equal to
- `>=` Greater than or equal to
- `>` Greater than
- `<=` Less than or equal to
- `<` Less than

Dates must be in YYYY-MM-DD format.

### DateTime Filter Operators

- `>=` Greater than or equal to
- `>` Greater than
- `<=` Less than or equal to
- `<` Less than

### Integer Filter Operators

- `=` Equal to
- `!=` Not equal to

### String Filter Operators

- `=` Equal to
- `!=` Not equal to

### Sending Filter Parameters

The filter string format is `[FIELDNAME]:[OPERATOR][VALUE]`

The integer and string `=` and `!=` operators also accepts a list of entries in the format:
`[FIELDNAME]:=[VALUE],[VALUE],[VALUE],[VALUE]`. For example, practitioner_id:=1,2,3

### Sending Filter Parameters

To filter a resource, send a filter string as the q parameter:

`https://au.instinctivenotes.com/api/v2/soap_notes?q=updated_at:>2019-01-29T11:59:59Z`

To apply multiple filters, send multiple filter strings as an array with the q[] parameter:

`https://au.instinctivenotes.com/api/v2/soap_notes?q[]=updated_at:>2019-01-29T11:59:59Z&q[]=id:=1,2,3`

The q[] method also works for a single filter string:

`https://au.instinctivenotes.com/api/v2/soap_notes?q[]=updated_at:>2019-01-29T11:59:59Z`

### Format rules

- `DateTime` must be sent as iso8601 format in UTC time_zone - eg: `2019-01-29T11:59:59Z`

### Filtering tips

For index endpoints you can get records that have been updated since a certain time by sending a filter for updated_at

Eg: `q=updated_at:>2019-01-29T11:59:59Z`

This is highly recommended where data will be stored in your application to reduce the number of calls and data returned.

## Webhooks

Instinctive Notes provides webhooks to notify your application when specific events happen.

Webhooks can remove the need to poll Instinctive Notes for up to date information.

[Learn more about webhooks](/guides/webhooks.md)

## API Resources

- [Schedules](/resources/schedules.md)
- [SOAP Notes](/resources/soap_notes.md)

## Questions
If you have any questions send an email to info@instinctive3.com
