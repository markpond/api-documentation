# Authorisation

Depending on your type of app, you'll want to either use the tokens workflow for web apps, or the password workflow for native apps.

### Tokens

Markpond uses a standard OAuth2 workflow.

The tokens workflow requires you to get an authorisation code, which you can then exchange for an access token.

The endpoint for authorisation is

    https://markpond.com/oauth/authorize
    
Using the `oauth2` gem, the first thing you need to do is initialize a client. For this example, we'll add your tokens into variables.

    callback = "http://myapp.com/callback"
    app_id = "b8ab7c5716bf850f8fed3c66ca491432316e7a411e4b563fb624fa2ab1850210"
    secret = "2e8c5d7e041dba58e7c6786057e6e5d7590eb80c1a599a939b0d990339ef45c7"
    
Then, initialize a client using this config:

	client = OAuth2::Client.new(app_id, secret, site: "https://markpond.com/")
	
To obtain the authorisation URL, run the `auth_code` method:

	redirect = client.auth_code.authorize_url(redirect_uri: callback)
	
	# If using Rails, now redirect the user
	redirect_to redirect
	
The user will then go and choose whether or not to authorize your application.

If the user refuses to grant access to your application, they will be redirected to an URL similar to:

    http://myapp.com/callback?error=access_denied&error_description=The+resource+owner+or+authorization+server+denied+the+request.
    
However, if the user does grant access, they will be redirected to an URL like this:

	http://myapp.com/callback?code=bfe5d04837d8d51e23a0ee07b94975f7a40d7138131f08daceafeb4ea10be759
	
Then, take the code out of the URL and get the access object:

	code = params[:code]
	access = client.auth_code.get_token(code, redirect_uri: callback)
	
The access token can be fetched using

	access.token

Store this token in your database for future requests. You're done authorising, so now you might want to check out the [endpoints](https://markpond.com/developers/endpoints).

### Password Credentials

Use this flow for mobile or other native applications that need to be authorised without using a web browser.

