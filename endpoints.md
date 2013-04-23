# Endpoints

All requests require the `access_token` parameter, which you should have obtained during the authorisation flow.

All responses are in valid JSON.

The root of the API is at

    https://markpond.com/api/v1/

## [Users](#users-endpoints)

- [**`GET` /users/me**](#users-me) - information about the current user
- [**`GET` /users/:id**](#users-id) - information about a specific user
- [**`GET` /users/:id/following**](#users-following) - who a user follows
- [**`GET` /users/:id/followers**](#users-following) - who follows a user 
- [**`POST` /users/:id/follow**](#users-follow) - follow a user
- [**`POST` /users/:id/unfollow**](#users-unfollow) - unfollow a user

## [Bookmarks](#bookmarks-endpoints)

All bookmark endpoints return 50 bookmarks. 

- [**`GET` /users/:id/bookmarks/**](#bookmarks-list) - the user's most recent bookmarks
- [**`POST` /bookmarks/**](#bookmarks-create) - create a bookmark
- [**`PUT` /bookmarks/:id**](#bookmarks-update) - update a bookmark
- [**`POST` /bookmarks/:id/star**](#bookmarks-star) - star a bookmark
- [**`POST` /bookmarks/:id/unstar**](#bookmarks-star) - un-star a bookmark
- [**`POST` /bookmarks/:id/add_tags**](#bookmarks-add_tags) - add tags without knowing the existing ones
- [**`POST` /bookmarks/:id/remove_tags**](#bookmarks-remove_tags) - remove tags from a bookmark

## [Feed](#feed-endpoints)

- [**`GET` /feed/**](#feed-endpoints) - return the user's feed

<hr />
<hr />

<h2 id="users-endpoints">Users</h2>

<h3 id="users-me">/users/me</h3>

This endpoint takes no parameters other than the required access token.

Sample request:

    GET https://markpond.com/api/v1/users/me
    
Sample response:

    {
        "account_type": 2,
        "active": true,
        "created_at": "2013-04-14T14:54:29Z",
        "email": "user@example.com",
        "id": 1,
        "real_name": "Alex",
        "updated_at": "2013-04-16T17:35:18Z",
        "username": "alex"
    }
    
<h3 id="users-id">/users/:id</h3>

This endpoint takes no parameters other than the required access token.

Sample request:

    GET https://markpond.com/api/v1/users/32
    
Sample response:

    {
        "created_at": "2013-04-14T15:34:29Z",
        "id": 32,
        "updated_at": "2013-04-16T17:35:18Z",
        "username": "myuser"
    }
    
<h3 id='users-following'>users/:id/following and users/:id/followers</h3>

These endpoints do not take any additional parameters, and behave similarly.

Sample requests:

    GET https://markpond.com/api/v1/users/1/following
    GET https://markpond.com/api/v1/users/1/followers

Sample response:

    [
        {
            "created_at": "2013-04-23T11:25:32Z",
            "followed_id": 2,
            "follower_id": 1,
            "id": 3,
            "updated_at": "2013-04-23T11:25:32Z"
        },
        {
            "created_at": "2013-04-23T11:24:43Z",
            "followed_id": 3,
            "follower_id": 1,
            "id": 4,
            "updated_at": "2013-04-23T11:24:43Z"
        }
    ]

If no users are found, the response will be:

    []

<h3 id="users-follow">/users/:id/follow</h3>

Sample request:

    POST https://markpond.com/api/v1/users/1/follow
    
Sample response:

    {
        "success": true
    }

However, if the current user is already following that user, you will recieve an error:

    {
        "success": false,
        "error": "Already following user 2"
    }
    
<h3 id="users-unfollow">/users/:id/unfollow</h3>

Sample request:

    POST https://markpond.com/api/v1/users/1/unfollow
    
Sample response:

    {
        "success": true
    }

However, if the current user is not following that user, you will recieve an error:

    {
        "success": false,
        "error": "Not following user 2"
    }
    
<h2 id="bookmarks-endpoints">Bookmarks</h2>

<h3 id="bookmarks-list">/users/:id/bookmarks</h3>

If the requested user is the user that authorised your app, all bookmarks are returned. However, if the requested user is not that user, no private bookmarks will be returned.

Parameters:

- `tags` - only return bookmarks with these tags (comma separated)
- `query` - search for bookmarks with this query
- `offset` - how many bookmarks to skip (offset 50 will start with the 51st bookmark)

Sample requests:

	GET https://markpond.com/api/v1/users/1/bookmarks?tags=apple&offset=50
	GET https://markpond.com/api/v1/users/1/bookmarks?query=bergcloud
	
Sample response:

	[
	    {
	        "archive": 0,
	        "tags": "gun control,us government",
	        "created_at": "2013-04-21T09:44:34Z",
	        "domain": "theverge.com",
	        "excerpt": "The 45 senators who blocked a gun control amendment despite its 90-plus percent approval rating may be in trouble with the white-haired godfather of Silicon Valley, \"super angel\" tech investor Ron...",
	        "id": 10,
	        "private": false,
	        "starred": true,
	        "title": "Can tech mogul Ron Conway use social media to oust 45 senators?",
	        "updated_at": "2013-04-21T12:20:38Z",
	        "url": "http://www.theverge.com/2013/4/19/4238760/ron-conway-most-sophisticated-social-media-campaign-ever-gun-control",
	        "user_id": 1,
	        "via": "Markpond Test"
	    },
	    {
	        "archive": 2,
	        "cached_tag_list": "music,daft punk",
	        "created_at": "2013-04-19T16:54:56Z",
	        "domain": "youtube.com",
	        "excerpt": null,
	        "id": 7,
	        "private": false,
	        "starred": null,
	        "title": "Daft Punk - 'Get Lucky'",
	        "updated_at": "2013-04-19T16:55:30Z",
	        "url": "http://www.youtube.com/watch?v=vxp0PFoIdmU",
	        "user_id": 1,
	        "via": "Chrome Extension"
	    }
	]
	
<h3 id="bookmarks-create">POST /bookmarks</h3>

Use this endpoint to create a new bookmark.

Parameters:

- `url` (Required) - the URL of the bookmark (valid URL)
- `title` - the bookmark title (guessed automatically if not supplied)
- `private` (Required) - privacy (true or false)
- `archive` - archive the bookmark (true or false)
- `excerpt` - the bookmark's excerpt
- `starred` - whether or not the bookmark is starred (true or false)

Sample request:

	POST https://markpond.com/api/v1/bookmarks
	
Sample response (the created bookmark):

	{
	    "archive": 0,
	    "tags": "amd,business",
	    "created_at": "2013-04-23T16:58:02Z",
	    "domain": "arstechnica.com",
	    "excerpt": "The Athlon 64 was AMD's high point, and it's been largely downhill since then.",
	    "id": 12,
	    "private": true,
	    "starred": null,
	    "title": "A company on the ropes",
	    "updated_at": "2013-04-23T16:58:02Z",
	    "url": "http://arstechnica.com/business/2013/04/amd-on-ropes-from-the-top-of-the-mountain-to-the-deepest-valleys/",
	    "user_id": 1,
	    "via": "Markpond Test"
	}

Error response:

	{
	    "success": false,
	    "errors": {
	        "url": [
	            "is not valid"
	        ]
	    }
	}
	
<h3 id="bookmarks-create">PUT /bookmarks/:id</h3>

Use this endpoint to update an existing bookmark.

Available Parameters:

- `title` - change the title
- `excerpt` - change the excerpt
- `tags` - replace all the current tags with new ones
- `private` - make a bookmark private


Sample request:

	PUT https://markpond.com/api/v1/bookmarks/12
		title = 'My new title'
		
Sample response (the edited bookmark):

 
	{
	    "archive": 0,
	    "tags": "amd,business",
	    "created_at": "2013-04-23T16:58:02Z",
	    "domain": "arstechnica.com",
	    "excerpt": "The Athlon 64 was AMD's high point, and it's been largely downhill since then.",
	    "id": 12,
	    "private": true,
	    "starred": null,
	    "title": "My new title",
	    "updated_at": "2013-04-23T16:58:02Z",
	    "url": "http://arstechnica.com/business/2013/04/amd-on-ropes-from-the-top-of-the-mountain-to-the-deepest-valleys/",
	    "user_id": 1,
	    "via": "Markpond Test"
	}
	
Example error response:

	{
	    "success": false,
	    "errors": "Cannot edit bookmark not belonging to current user"
	}
	
<h3 id="bookmarks-create">POST /bookmarks/:id/star and POST /bookmarks/:id/unstar</h3>

Use this endpoint to star or unstar a bookmark.

Sample request:

	POST https://markpond.com/api/v1/bookmarks/12/star
	POST https://markpond.com/api/v1/bookmarks/12/unstar
	
A successful request will give a response of the edited bookmark in the same format as above.

Example error response:

	{
	    "success": false,
	    "errors": "Cannot star bookmark not belonging to current user"
	}
	
<h3 id="bookmarks-add_tags">POST /bookmarks/:id/add_tags</h3>

Use this endpoint to add tag(s) to a bookmark without having to request the current tags first

Sample request:

	POST https://markpond.com/api/v1/bookmarks/12/add_tags
		tags = 'newtag'
		
A successful request will give a response of the edited bookmark in the same format as above.

Example error response:

	{
	    "success": false,
	    "errors": "Cannot add tags to a bookmark not belonging to current user"
	}
	
<h3 id="bookmarks-remove_tags">POST /bookmarks/:id/remove_tags</h3>

Use this endpoint to remove tag(s) from a bookmark without having to request the current tags first

Sample request:

	POST https://markpond.com/api/v1/bookmarks/12/remove_tags
		tags = 'newtag'
		
A successful request will give a response of the edited bookmark in the same format as above.

If you try to remove a tag that is not on the list, no error will be thrown.

Example error response:

	{
	    "success": false,
	    "errors": "Cannot remove tags to a bookmark not belonging to current user"
	}
	
<h2 id='feed-endpoints'>Feed</h2>

This endpoint returns bookmarks in the same format as above, and the only required parameter is the access token.

Sample request:

	GET https://markpond.com/api/v1/feed