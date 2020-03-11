# weather-app
This is a weather app built using node.js, express, bootstrap, and OpenWeather API.

This is based on the codeburst.io article by Brandon Morelli.

Check him out here: https://github.com/bmorelli25

Original article: https://codeburst.io/build-a-weather-website-in-30-minutes-with-node-js-express-openweather-a317f904897b

1.  Pre-Project Setup
    <br>
    Here’s what you’ll need:

    - OpenWeatherMap.org account for API key
    - Node.js: Visit the official Node.js website to download and install Node if you haven’t already
    - Express: Visit the official express site and install
    - Bootstrap: Visit the official bootstrap site and install

2.  Create an empty directory named weather-app:
    
    - Open up your console, navigate to the new directory and run npm init
    - Fill out the required information to initialize the project
    - Within the weather-app directory, create a file named server.js — this file will house the code for the application.

3.  Creating a server with express:
    
    - Run ```npm install --save express```
    - Copy the boilerplate Express starter app from the official express documentation:
        ```
        const express = require('express')
        const app = express()

        app.get('/', function (req, res) {
          res.send('Hello World!')
        })

        app.listen(3000, function () {
          console.log('Weather app listening on port 3000!')
        })
        ```
    - You can test your server by running ```node server.js``` and visiting ```http://localhost:3000/```
    
4.  Setting up the index view:

    - We will be using ejs so we can respond to root requests with a file instead of just text
    - First, install ejs in terminal by running ```npm install ejs --save```
    - Set up the template engine with this line of code (just below our require statements) in our server.js file:
        ```app.set('view engine', 'ejs')```
    - Make a new directory called views, and within that directory, create a file called index.ejs:
        ```mkdir views```
        ```touch index.ejs```
    - Here is our boilerplate ejs file:
        ```
        <!DOCTYPE html>
        <html>
          <head>
            <meta charset="utf-8">
            <title>Test</title>
            <link rel="stylesheet" type="text/css" href="/css/style.css">
            <link href='https://fonts.googleapis.com/css?family=Open+Sans:300' rel='stylesheet' type='text/css'>
          </head>
          <body>
            <div class="container">
              <fieldset>
                <form action="/" method="post">
                  <input name="city" type="text" class="ghost-input" placeholder="Enter a City" required>
                  <input type="submit" class="ghost-button" value="Get Weather">
                </form>
              </fieldset>
            </div>
          </body>
        </html>
        ```
    - We now have to install bootstrap in terminal by running ```npm install bootstrap --save```

5.  Edit the server.js file:
    
    - The next thing we need to do is change this line of code in the server.js file:
        ```
        app.get('/', function (req, res) {
              // OLD CODE
              res.send('Hello World!')
            })
        ```
    - To something that will render a file instead of outputting hello, world:
        ```
        app.get('/', function (req, res) {
              // NEW CODE
              res.render('index');
            })
        ``` 
    - At this point, we can test our server again by running ```node server.js``` and visiting ```http://localhost:3000/```
    
6.  Adding custom CSS:
    
    - Even though we care using the bootstrap framework, you may want to add custom CSS
    - You can do this by making a new directory in weather-app: 
        ```
        mkdir public
        ```
    - Then, a folder called CSS
        ```
        mkdir CSS
        ```
    - Then, a file called styles.css
        ```
        touch styles.css
        ```
    - Express wont allow access to this file by default, so we need to expose it with the following line of code in server.js:
        ```
        app.use(express.static('public'));
        ```
