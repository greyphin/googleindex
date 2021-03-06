# Google Indexing API

This CFML wrapper has been developed to allow you to easily interact with the [Google Indexing API](https://developers.google.com/search/apis/indexing-api/v3/quickstart) so that any site owner can directly notify Google when pages are added or removed, allowing Google to schedule pages for a fresh crawl.

## Requirements for this wrapper

This wrapper has been developed to manage the full OAuth2 flow using service account credentials, generated by Google and provided as a `.JSON` file.

The component manages the generation of the required JWT and submission to the authentication API to obtain the required access token to then make authenticated requests to the API.

# CommandBox Compatible

## Installation
This CF wrapper can be installed as standalone or as a ColdBox Module. Either approach requires a simple CommandBox command:

`box install googleindex`

Then follow either the standalone or module instructions below.

### Standalone
This wrapper will be installed into a directory called `googleindex` and then can be instantiated via `new googleindex.GoogleIndex()` with the following constructor arguments:

```
    filePath			    =	'',
    tokenEndpoint		    =	'',
    notificationsEndpoint	=	''
```

### ColdBox Module
This package also is a ColdBox module as well. The module can be configured by creating a `googleindex` configuration structure in your application configuration file( `config/Coldbox.cfc`) with the following settings:

```
googleindex = {
    filePath			    =	'',
    tokenEndpoint		    =	'',
    notificationsEndpoint	=	''
};
```
Then you can leverage the CFC via the injection DSL: `googleindex@googleindex`

## Example

To get started, instantiate the `GoogleIndex` component, passing in the file path to the `.JSON` file that contains the service account credentials:

```
<cfset objGoogleIndex = new googleindex.GoogleIndex( expandPath( './credentials.json' ) ) />
```

Make a call to obtain an access token:

```
<cfset stuAccessToken = objGoogleIndex.getAccessToken() />
```

The `getAccessToken()` method will store the access token within the component but also returns the struct containing the response should you wish to store the data elsewhere.

From this point, there are three methods you can call to interact directly with the Indexing API:

* `updateURL()`
* `removeURL()`
* `getStatus()`

All three methods require a `url` parameter to complete the request.

To notify Google of a new URL to crawl or that content at a previously-submitted URL has been updated, use the `updateURL()` method:

```
<cfset response = objGoogleIndex.updateURL( 'https://www.monkehworks.com' ) />
```

After you delete a page from your servers, you can notify Google so that they can remove the page from their index and so that they don't attempt to crawl the URL again. You can do this using the `removeURL()` method:

```
<cfset response = objGoogleIndex.removeURL( 'https://www.monkehworks.com' ) />
```

** Before you request removal, you must remove the page from your server and the URL must return a 404 or 410 status code. **

You can use the Indexing API to check the last time Google received each kind of notification for a given URL. The GET request doesn't tell you when Google indexes or removes a URL; it only returns whether you successfully submitted a request. You can do this using the `getStatus()` method:

```
<cfset response = objGoogleIndex.getStatus( 'https://www.monkehworks.com' ) />
```


