# Back-End

Heroku link:

https://spidergraph-backend.herokuapp.com/

Endpoints:

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


### TO DO

- [x] - Create CRUD endpoints for Users
- [ ] - Write middleware for Users endpoints (validate user id, etc.)
- [ ] - Add tables for handling graph data
- [ ] - Write endpoints for graphs
- [ ] - Wire up authentication middleware
- [ ] - Write tests
