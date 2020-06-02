

# Embedded Transcriber Application

​

 * tools for ground truth creation

​

## How It Works

​

## Setup

​

Include the script for bootstrapping the transcriber application:

​

```html

<script src="https://transkribus.eu/r/read/embed.js"></script>

```

​

Intialize the application looks like this:

​

```javascript

var app;

$.ready(function () {

	app = new TranscriberApplication('#transcriber-application', {

		/* specify what page should be loaded */

		id: {colId: 42, docId: 42, pageNum: 1},

		/* this token will be used to authenticated you with transkribus server */

		origin: 'someoriginidentifier',

	    /* hide the image, i.e. show transcript only */

	    image: false,

		mode: 'annotation',

		token: 'your-transcribus-authentication-token',

		success: function () { alert("Success loading application."); },

		error: function () { alert("Failed to load application."); },

	});

});

```

​

Make sure `origin` is set correctly (that's the name of the website you are embedding the application into). If no value for origin is specified it is resolved to http://localhost:8000.

​

In case your origin is invalid, you might get the following error in the console:

​

```

TranscriberApplication.js:189 Expected origin https://some-orgin.org but got https://some-other-origin.org

handleMessage @ TranscriberApplication.js:189

``` 

​

**NOTE:** The content of _#transcriber-application_ element fills all available vertical and horizontal space ([example layout](http://jsfiddle.net/2j53y6eb/12/)).

​

**NOTE:** See section on [Authentication](#authentication) to find out how to retrieve a token.

​

Saving a transcript works as follows:

​

```javascript

$('#save-button').on('click', function () {

	app.save({status: app.STATUS.DONE});

```

​

You can specify callbacks that will be executed when the transcript has been saved:

​

```javascript

$('#save-button').on('click', function () {

	app.save({

		success: function () {

			alert("Transcript Saved!");

		},

		error: function () {

			alert("Something went wrong!");

		}

	});

});

```

​

**NOTE**: `pageId` is not really an id but the number of the page, i.e. `pageNr` on TranskribusServer

​

## Authentication

​

Use the `/auth/login` endpoint for logging in:

​

```bash

curl --location --data 'user=your-account-name&pw=your-password' --verbose --request POST https://transkribus.eu/TrpServer/rest/auth/login

```

​

The value in for the cookie named `JSESSIONID` is your `token`.

​

For logging out, use the `/auth/logout` endpoint like this:

​

```bash

curl --location --verbose --cookie "JSESSIONID=token-goes-here" --request POST https://transkribus.eu/TrpServer/rest/auth/logout

```

​

To check if your session is still valid, do the following:

```bash

curl --location --verbose --cookie "JSESSIONID=token-goes-here" --request GET https://transkribus.eu/TrpServer/rest/auth/checkSession

```

​

**NOTE:** Tokens are valid for 8 hours.

