# Back-End

Heroku link:

https://spidergraph-backend.herokuapp.com/

Summary of Endpoints:

| Type | Route  | Description   |
|---|---|---|
| POST  | /api/auth/register   | Register user  |
| POST  | /api/auth/login  | Login user  |
| GET  | /api/users/:id  | Get a specific user  |
| PUT   | /api/users/:id  | Update a specific user  |
| DELETE  | /api/users/:id  | Delete a specific user   |
| GET  | /api/graphs  | Will return all of the graphs for a specific user.  |
| POST  | /api/graphs  | Add a graph  |
| GET  | /api/graphs/:id  | Retrieve a specific graph   |
| PUT  | /api/graphs/:id   | Update a specific graph  |
| DELETE  | /api/graphs/:id   | Delete a specific graph  |


Endpoint Details:

### POST to /api/auth/register

```
{
    "username": "string", // required
    "password": "", // required
    "name": "string", // required
    "email": "string" // unique, required
}
```
- Will return a JSON object containing all of the information in the request, the User ID generated by the database, and an authorization token. The token will be generated upon registration so that they can immediately be redirected to a restricted page without having to be re-authorized.

Sample response:
```
{
    "created_user": {
        "id": 2,
        "username": "newUser",
        "password": "$2a$10$M.cWBDGdJ21zSBm3vOO7sOjP.7FAKrlrL8qwJbhBo.1CfF.yeXVeq",
        "name": "patrick",
        "email": "test@test.com"
    },
    "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VyaWQiOjIsInVzZXJuYW1lIjoiY3NodXNoZXJlYmEiLCJpYXQiOjE1NzgxODkwMjEsImV4cCI6MTU3ODE5MjYyMX0.Sfmt0liQSfhbbhaZ4Oso9G9CgP_tYVgUZpNFAJcy6g0"
}
```

### POST to /api/auth/login

```
{
	"username": "string",
	"password": "string"
}
```
- Will return a JSON object containing the username and an authorization token.

### GET to /api/users/:id

- Will return a JSON object with the following structure:

```
{
    "id": 1,
    "username": "patrick",
    "password": "$2a$10$8wVEOJTAY4WcFA.7zA0tLefRr1tbgVG9sZUdwKwosOeiaXtBg6HQ2",
    "name": "Patrick",
    "email": "patrick@test.com"
}
```

### PUT to /api/users/:id

- Send request with desired changes. Server will return a JSON object with all of the user information. The changes will be reflected in the fields that were updated.

### DELETE to /api/users/:id

- Send request to delete user. If the request was successful the server will send the following response:

```
{
    "message": "The user was successfully deleted"
}
```
### POST to /api/graphs

- Send the request with the following format. Note that a "*" designates a required field. The `graph_info` column in the `graphs` table will store a JSON object. You will need to use `JSON.stringify` on that section before sending the request.

```
{
	* "graph_name": "test",
	* "graph_info":  "{
		"labels": ['Axis1', 'Axis2', 'Axis3'],
		"datasets": [
	        {
	          label: 'Dataset1',
	          borderDash: [0, 0],
	          backgroundColor: '#fff',
	          data: [25, 14, 22],
	        },
    			],
			title: 'Graph1'
		}"
	  "image": "http://www.samplewebsite.com/image",
    * "user_id": "1"
}
```

- The database will return a JSON object with the following structure:

```
{
    "id": 2,
    "graph_name": "project graph 1",
    "graph_info": {
        "title": "Graph1",
        "labels": [
            "Axis1",
            "Axis2",
            "Axis3"
        ],
        "datasets": [
            {
                "data": [
                    25,
                    14,
                    22
                ],
                "label": "Dataset1",
                "borderDash": [
                    0,
                    0
                ],
                "backgroundColor": "#fff"
            }
        ]
    },
    "image": null,
    "user_id": 1
}
```

### GET to /api/graphs/:id

- Will return a JSON object with the following structure:

```
{
	"graph_name": "project graph 1",
	"graph_info": {
		"labels": ['Axis1', 'Axis2', 'Axis3'],
		"datasets": [
	        {
	          label: 'Dataset1',
	          borderDash: [0, 0],
	          backgroundColor: '#fff',
	          data: [25, 14, 22],
	        },
    			],
			title: 'Graph1'
		}),
	"user_id": "1"
}
```

### PUT to /api/graphs/:id

- Send request with desired changes. Server will return a JSON object with all of the graph information. The changes will be reflected in the fields that were updated.

### DELETE to /api/graphs/:id

- Send request to delete graph. If the request was successful the server will send the following response:

```
{
    "message": "The graph was successfully deleted"
}
```

### Changelog

1/4/20

- Added GET endpoint for graphs table `/api/graphs`
- Added column for images to graphs table.

1/7/20

- Started to write middleware to validate that a user exists.

### Current Issues

- validateUser middleware is not working. Initially I had the middleware in server.js. That was throwing an error because the `id` was always undefined. After asking for help in WEBPT7 help channel, I moved the middleware to the router, and tried to implement it on all of the routes that included `/:id`, but now the request hangs and does not resolve.

- Need direction on `/api/graphs` endpoint: I wanted to keep my users and graphs endpoints separate, because it seemed like bad practice to mix routers/models. My intention was to have the `/graphs/:id` endpoint return all of the graphs for the user with that id. I set the graphs table up to have the `user_id` as a foreign key, and I wrote the helper function to try and query the database to match the `user_id` with the id that is pulled out of the params. But it doesn't return all of the graphs. As a test I added two different graphs with the `user_id` of 1. When I tried to access `/api/graphs/1`, only one graph was returned to me from the database.


### TO DO

- [x] - Create CRUD endpoints for Users
- [ ] - Write middleware for Users endpoints (validate user id, etc.)
- [x] - Add tables for handling graph data
- [x] - Write endpoints for graphs
- [x] - Wire up authentication middleware
- [ ] - Get tests to pass.
- [ ] - Set up test database for testing