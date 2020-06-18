# The Instinctive Notes API (Beta)

This is the official API for Instinctive Notes. Instinctive Notes is a clinical note taking service for Allied Health Practices.

This API is in beta and may change significantly during this period.

The Instinctive Notes API is based on REST principles. This means you can use any HTTP client and any programming language to interact with the API.

## Security

The Instinctive Notes API is served over HTTPS to ensure data security and privacy.

HTTP is not supported and will be redirected to HTTPS automatically.

Ensure that the HTTP client is up-to-date and has the latest TLS, cipher suites and SNI available.

From late 2020 requests to the Instinctive Notes API will require TLS 1.2.

## Base URL

The base URL should preceed any endpoint calls made to the API.

The base URL is made up of three components:

- A 3 character shard identifier
- The API URL
- The two character version identifier

An example base URL:
```
https://au1.instinctivenotes.com/api/v2
```

### Shards

A shard is an isolated instance of Instinctive Notes. All the data for a practice will be located in a specific shard based on the location of the practice.

Each shard will be represented by one (or more) three character shard identifiers.

Current shard identifiers are:

- `au1`
- `nz1`

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
- The 3 character shard

Eg: `ABsUn1ggcLazRmRXYXYgQrnc-au1`

The full API Key, including the dash and the shard, should be sent with any request. See the `Authentication` section below for how to send the API Key.

While the API is in Beta please send requests for API Keys to info@instinctive3.com and we will be in touch to discuss what you are looking to do with the API.

## Authentication

The Instinctive Notes API uses Token authentication in the HTTP Authorization header. This is secure as all requests are via HTTPS.

Each Instinctive Notes Account can generate API Keys which are used for authentication.

You should provide the Instinctive Notes API Key in the `Authorization` header with the following structure:

```
Token token="xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"
```

## Identifying your application

To identify your application, you need to send the User-Agent header. In the event of an issue, this allows us to easily track down your requests and contact you. This should be in the form:

`APP_VENDOR_NAME (APP_VENDOR_EMAIL)`

**APP_VENDOR_NAME** is the name of your application

**APP_VENDOR_EMAIL** is a contact email address for you or your company

**If your application does not contain a User-Agent that contains the name and valid contact details, it may be blocked.**

## Errors

Conventional HTTP response codes are used to indicate API errors.

General code rules apply:

- 2xx range indicate success.
- 4xx range indicate an error resulting from the provided information (eg. missing a required parameter)
- 5xx range indicate an error with our Instinctive Notes servers

## Making a request

At a minumum requests must contain:

- The request `URL`
- The `Authorization Token` header with the Instinctive Notes API Key
- The `Accept` header with `application/json`
- The `User Agent` header with your details

The following is a curl request for all soap_notes.

```
curl https://au1.instinctivenotes.com/api/v2/soap_notes \
  -H 'Authorization: Token token="API_KEY"' \
  -H 'Accept: application/json' \
  -H 'User-Agent: APP_VENDOR_NAME (APP_VENDOR_EMAIL)'
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

The integer and string `=` and `!=` operators also accepts a list of entries in the format: `[FIELDNAME]:=[VALUE],[VALUE],[VALUE],[VALUE]`. For example, practitioner_id:=1,2,3

### Sending Filter Parameters

To filter a resource, send a filter string as the q parameter:

`https://au1.instinctivenotes.com/api/v2/soap_notes?q=updated_at:>2019-01-29T11:59:59Z`

To apply multiple filters, send multiple filter strings as an array with the q[] parameter:

`https://au1.instinctivenotes.com/api/v2/soap_notes?q[]=updated_at:>2019-01-29T11:59:59Z&q[]=id:=1,2,3`

The q[] method also works for a single filter string:

`https://au1.instinctivenotes.com/api/v2/soap_notes?q[]=updated_at:>2019-01-29T11:59:59Z`

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

- [SOAP Notes](/resources/soap_notes.md)

## Questions
If you have any questions send an email to info@instinctive3.com
