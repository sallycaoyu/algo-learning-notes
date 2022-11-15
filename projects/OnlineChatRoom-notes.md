# Notes of creating an online chat room

## Preparing for server

### Project initialization
- In backend folder, do `npm init`.
- In package.json, change `main: index.js` to `main: server.js`, then do `npm install --save express express-joi-validation joi jsonwebtoken mongoose cors dotenv bcryptjs nodemon` to install all the dependencies.

### Creating the server
- In server.js: import packages, initialize port, register middlewares, create server
- Create .env file and define a API_PORT variable for local port.
- Run `node server.js` to test if the server can run by listening to the right port.

### Add nodemon to automatically restart server after changes
- Under "script" of `package.json`, add `start: "nodemon server.js"`, then run `npm start` instead of `node server.js` to run things under "script".

### Prepare MongoDB
- In MongoDB Atlas, I created a cluster and added 0.0.0.0/0 to IP access list so it can be accessed anywhere.
- After created a cluster, do "browse collections" -> "add my own data" to create a database, then do "connect" to get a link that can connect our app to mongodb.
- In .env, create MONGO_URI to store that link. Create a new set of username and password and fill them into blanks of the link.

### Connect to MongoDB from server
- In server.js: connect to mongodb

### Create server directory structure and authentication routes
- Under chatroom-backend, create the following directories: routes, middleware, controllers, models
- Under routes, add authRoutes.js. Add some post request like register and login to test if router can function well in Postman.
- In server.js: register the routes
- In authRoutes.js, remember to export router so that other modules can access it.

### Move auth handler to controllers
- Under controllers, add auth directory, then add authControllers, postLogin and postRegister in auth.
- Move login and register handlers in authRoutes.js to controllers/auth/postLogin.js and postRegister.js respectively. Make them asynchronous and remember to export.
- After moving, in authRoutes, import authControllers and replace the handlers with `authControllers.controllers.postRegister` or xxx.xxxxx.postLogin.

### Request validation with Joi
- In authRoutes.js, create schemas to set some validation rules of registration and login(using Joi, express-joi-validation).
- Validate first, then send post requests through registration and login routers.
- Add `.required()` to make the validation rules always work.
- Test Joi validation by Postman: Postman -> Body -> raw -> JSON, add a JSON sample of username, password and mail.

### Create a user model
- Under models dir, create user.js
- Import mongoose, create a userSchema, export this mongoose userSchema

### Add functionality to register
- In postRegister.js, try-catch, catch error and return 500
- Collect username, mail, pw from req.body
- In postRegister.js: check if user exists, return 409 if true; encrypt password, create user document and save in database
- Create JWT token and return it to client. If no valid JWT token or it expired, account will be automatically logged out.
- Test register route with Postman:
    - remember to give read AND write access to the user connected to mongodb (in MONGO_URI)!!!!

### Add functionality to login
- See postLogin.js

### Create JWT Token
- Use jwt.sign
- Create a TOKEN_KEY in .env
- Set expire time for the token. If token expired, the user will be automatically logged out.

### Create middleware to check if token is valid or expired
- Create middleware -> auth.js
- If token not found, return 403 status, which automatically log out account and return back to login screen.
- Else if token found, replace "Bearer " in the front, then decode token using jwt.verify
- In authRoutes, see "test route to verify if our middleware is working"