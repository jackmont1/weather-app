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
    - Bootstrap will also require ```npm install jquery --save``` and ```npm install popper.js --save```

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
            <title>Weather App</title>
            <link rel="stylesheet" type="text/css" href="/css/style.css">
            <link href='https://fonts.googleapis.com/css?family=Open+Sans:300' rel='stylesheet' type='text/css'>
          </head>
          <body>
            <div class="container-fluid">
              <form class="form-group" action="/" method="post">
                <input name="city" type="text" class="form-control" placeholder="Enter a City" required>
                <input type="submit" class="btn btn-submit" value="Get Weather">
              </form>
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
    - By now, server.js should look like this:
        ```
        const express = require('express');
        const app = express()

        app.use(express.static('public'));
        app.set('view engine', 'ejs')

        app.get('/', function (req, res) {
          res.render('index');
        })
        app.listen(3000, function () {
          console.log('Weather app listening on port 3000!')
        })
        ```

7.  Create a POST route

    - Our app already has a GET request, now we need a POST request:
        ```
        app.post('/', function (req, res) {
          res.render('index');
        })
        ```
    - Install express middleware body-parser. This will allow us to make use of the key-value pairs stored on the req-body object. In this case, the city name inputted by the user.
        ```
        npm install body-parser --save
        ```
    - Once installed, we can require body-parser and make use of it in server.js:
        ```
        const bodyParser = require('body-parser');
        // ...
        // ...
        app.use(bodyParser.urlencoded({ extended: true }));
        ```
    - Finally, update the POST route:
        ```
        app.post('/', function (req, res) {
          res.render('index');
          console.log(req.body.city);
        })
        ```
    - Now we can test our server by running ```node server.js``` and visiting ```http://localhost:3000/```
    - Now type a city name into the field and hit enter! You should see the city name displayed in your command prompt! You’ve now successfully passed data from the client to the server!
    
8.  Make an OpenWeather API request

    - Set up the URL. The first thing we do when we receive the post request is grab the city off of req.body. Then we create a url string that we’ll use to access the OpenWeatherMap API with:
        ```
        app.post('/', function (req, res) {
          let city = req.body.city;
          let url = `api.openweathermap.org/data/2.5/weather?q={city name}&appid={your api key}`
        ```
    - Now that we have our URL, we can make our API call:
        ```
        request(url, function (err, response, body) {
        if(err){
          res.render('index', {weather: null, error: 'Error, please try again'});
        ```
    - Now that we know we have no API error, we can parse our JSON into a usable JavaScript object:
        ```
        } else {
          let weather = JSON.parse(body)
          if(weather.main == undefined){
            res.render('index', {weather: null, error: 'Error, please try again'});
          } else {
            let weatherText = `It's ${weather.main.temp} degrees in ${weather.name}!`;
            res.render('index', {weather: weatherText, error: null});
          }
        }
        ```
    - Make use of all those variables we sent back with our res.render call:
        ```
        <% if(locals.weather !== null){ %>
        <p><%= locals.weather %></p>
        <% } %>

        <% if(locals.error !== null){ %>
        <p><%= locals.error %></p>
        <% } %>
        ```
    - The full index.ejs should look like this:
        ```
        <!DOCTYPE html>
        <html>
          <head>
            <meta charset="utf-8">
            <title>Weather App</title>
            <link rel="stylesheet" type="text/css" href="/css/style.css">
            <link href='https://fonts.googleapis.com/css?family=Open+Sans:300' rel='stylesheet' type='text/css'>
          </head>
          <body>
            <div class="container-fluid">
              <form class="form-group" action="/" method="post">
                <input name="city" type="text" class="form-control" placeholder="Enter a City" required>
                <input type="submit" class="btn btn-submit" value="Get Weather">
              </form>
              <% if(locals.weather !== null){ %>
              <p><%= locals.weather %></p>
              <% } %>
              <% if(locals.error !== null){ %>
              <p><%= locals.error %></p>
              <% } %>
            </div>
          </body>
        </html>
        ```
    - The full server.js should look like this:
        ```
        const express = require('express');
        const bodyParser = require('body-parser');
        const request = require('request');
        const app = express()

        const apiKey = '*****************';

        app.use(express.static('public'));
        app.use(bodyParser.urlencoded({ extended: true }));
        app.set('view engine', 'ejs')

        app.get('/', function (req, res) {
          res.render('index', {weather: null, error: null});
        })

        app.post('/', function (req, res) {
          let city = req.body.city;
          let url = `api.openweathermap.org/data/2.5/weather?q={city name}&appid={your api key}`

          request(url, function (err, response, body) {
            if(err){
              res.render('index', {weather: null, error: 'Error, please try again'});
            } else {
              let weather = JSON.parse(body)
              if(weather.main == undefined){
                res.render('index', {weather: null, error: 'Error, please try again'});
              } else {
                let weatherText = `It's ${weather.main.temp} degrees in ${weather.name}!`;
                res.render('index', {weather: weatherText, error: null});
              }
            }
          });
        })

        app.listen(3000, function () {
          console.log('Weather app listening on port 3000!')
        })
        ```
    - Obviously, replace ```const apiKey = '*****************';``` with your own API key
    - Before we run the program, you will need to run ```npm install request --save```
    - Now run ```node server.js``` and visit ```http://localhost:3000/```
    
