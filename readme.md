# AirtableToolkit Documentation

## Introduction

### What is AirtableToolkit ?

AirtableToolkit is a tools that aims to bring access to all Airtable features through an API.

### How does it work ?

The tool can be divided in two main components :
- the AirtableCookiesFetcher Chrome Extension
- the AirtableTookit API

The extension is used to fetch your Airtable account cookies (and updated them when needed) and send them to our server.
The AirtableToolkit API then uses those cookies to provide you additional features through the API.

### How do you offer those "additional features" ?

Basically, Airtable itself uses an API for all the actions you do on Airtable.
Each time you create a field, rename a table or even create an automation, an API call is sent by the Airtable webapp to the Airtable backend.
We are just providing a "proxy" to this API.

## Technical documentation

### BaseURL

Please note this development is still in BETA and base url is subject to changes.

**Take this into consideration when building integrations that rely on this tool.**

Good practice is to use dedicated variable storing the base URL, so you can easily change it in one place later.

**Current base URL is :**
`https://airtabletoolkit.herokuapp.com`

### Authentication

**Method 1 : Authorization header (preferred)**

We use Bearer Token authentication as shown below.

You will receive your token by email quickly after installing the extension.

| Header name | Header value |
| --- | --- |
| Authorization | Bearer {{yourApiKeyHere}} |

**Method 2 : Query params**

You can just call any endpoint and add the additional `AirtableToolkitAPIKey` parameter with you APIKey as value.

| Query parameter name | Query parameter value |
| --- | --- |
| AirtableToolkitAPIKey | {{yourApiKeyHere}} |

**Example endpoint with required parameter**
`https://{{BASE_URL}}/getLoggedInUser?AirtableToolkitAPIKey={{yourApiKeyHere}}`

### Airtable IDs : generation function

<aside>
ℹ️ This is especially useful when using the “create” endpoint where the API requires you to give an ID to use.
Usually, when creating a field, table or view, you don’t really care about the ID, you just want to generate one that is valid. That’s exactly the role of this function.
It works as a basic string that you can "inject" in your URL and we will automatically replace it with a valid ID.
</aside>

**Function :** `*generateId:app*`

**Example endpoint :** `/v0.3/field/***generateId:fld***/create`

**Resulting endpoint :** `/v0.3/field/**fldfJVP3dsSR6j1su**/create`

- **Common ID prefixes**
    - app/base : app
    - table : tbl
    - field : fld
    - view : viw
    - …

### Proxy endpoints

This is a specific endpoint that allows you to use any available endpoint from Airtable.
It "just" forwards the calls. There are quite a few things happening in the background but this is what this endpoint is made for.
So if the documentation is missing your required endpoint, feel free to use the Network tab from Chrome Developer panel and use the same urls and parameters.

[List of interesting endpoint](./autogenerated_2022-07-28T20%3A50%3A47.463Z.md)
This file will give you a basic documentation for interesting endpoint.

Please feel free to reach me by email (florianverdonck [at] gmail.com) if you need other endpoints documentation.

### Account endpoints

⚠️ These are specific endpoints with data that is not available from the frontend API but fetched directly from the page content.

[GET] `/account/`

Returns the currently "logged in user" details.

[GET] `/account/collaborators`

Returns all the collaborators you are working with in all the bases you have access to