# HAPI - Humanized API

The goal of HAPI is to define a standard for creating Web-based APIs that are machine ready but human/developer friendly.
HAPI attempts to accomplish this by trying to adhere to the following principles:

1. A HAPI should be accessible through a URL with nothing more than a standard web browser.
2. All inputs and outputs to a HAPI should generally be readable in a sentence form.
3. A HAPI should be self-documenting and generally understandable by non-technical people.

## Requests

### General Rules

1. A HAPI *must* support the HTTP GET verb on all operations.
2. A HAPI *must* not require any HTTP headers above and beyond what a standard web browser will send.
3. A HAPI *should* strive to keep all input parameters within the URL of the request and not require any content within the HTTP request body.
4. For requests with content (e.g. files) that cannot be sent through a URL, the HAPI *must* render an HTML page with a form to submit the content.
5. A HAPI *must* support non-secure HTTP requests, but *should* return an error message with the proper secure, HTTPS URL.
6. A HAPI *can* support cookies for security and tracking purposes, but must provide an HTML form to generate and/or set the cookies.

### URLs

URLs to access the operations on a HAPI are meant to generally follow the structure of an english sentence so as to be easily memorized and recalled without having to consult documentation.

#### GET and CRUD Operations

URLs for GET and CRUD (CReate, Update, Delete) operations *must* adhere to the following forms:

(Note: spaces have been added for readability)

GET (all): `https://api.` **[domain]** `/ get / all /` **[resource_type]**

GET (query): `https://api.` **[domain]** `/ get / all /` **[resource_type]** `/ where / ?` **[parameters]**

GET (specific): `https://api.` **[domain]** `/ get /` **[resource_type]** `/ called /` **[resource_id]**

CREATE: `https://api.` **[domain]** `/ create /` **[resource_type]** `/ with / ?` **[parameters]**

UPDATE: `https://api.` **[domain]** `/ change /` **[resource_type]** `/ called /` **[resource_id]** `/ to / ?` **[parameters]**

DELETE: `https://api.` **[domain]** `/ delete /` **[resource_type]** `/ called /` **[resource_id]**

Where:

**[domain]**: The domain name of the HAPI. While HAPIs should generally not be segmented into separate parts, it is acceptable to prefix the domain with a sub-domain for purposes of staging, and testing environments such as: *api.staging.mydomain.com*.

**[resource_type]**: The name of a type of resource, like *employee* or *post*. The HAPI *must* support and treat the singular and plural form of the resource type as equal. For example, *employee* and *employees* will both be valid and considered equal.

**[resource_id]**: The unique ID or name of a resource, like *2a89ef* or *fritz_734*.

**[parameters]**: Any parameters required to complete the request in standard name/value pair format for HTTP query params.

Examples:

```
https://api.doh-main.com/create/donut/with/?filling=jelly

https://api.doh-main.com/get/all/donuts

https://api.doh-main.com/get/all/donuts/where/?filling=jelly

https://api.doh-main.com/get/donut/called/mmmmm_donut_01

https://api.doh-main.com/change/donut/called/mmmmm_donut_01/to/?filling=custard
```

## Responses

Responses to HAPI operations are meant to generally follow the structure of an english sentence so as to be easily understood by a lay-person.

### General Rules

1. A HAPI *must* return all important content to a request in the body of the HTTP response.
2. A HAPI *may* return HTTP headers that set cookies, standard security parameters or other features that are generally understood by most common browsers.
3. A HAPI *must* return content in JSON formatted text as a default response to any operation. Other formats may be supported as needed.

### Errors

Errors will be returned as JSON formatted text with an HTTP response code that matches as close as possible to the type of error being returned.

```
{ "this": "failed", "with_a": [error_code], "because": "[error_message]" } 
```

Where:

**[error_code]**: A numerical error code for easy parsing by machines. This may or may not be different than the HTTP response code.

**[error_message]**: A human readable error message.

Example:

```
{ "this": "failed", "with_a": 5600, "because": "donuts with holes can't contain jelly" }
```

