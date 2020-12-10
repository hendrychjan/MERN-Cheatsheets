# React deploy to Heroku
### ExpressJS server serving app
- since Heroku can't host React app directly, you have to wrap it with Node.js application
- in root of project repo, create file ```server.js```
```javascript
//server.js
const express = require('express');
const favicon = require('express-favicon');
const path = require('path');
const port = process.env.PORT || 8080;
const app = express();
app.use(favicon(__dirname + '/build/favicon.ico'));
// the __dirname is the current directory from where the script is running
app.use(express.static(__dirname));
app.use(express.static(path.join(__dirname, 'build')));
app.get('/ping', function (req, res) {
 return res.send('pong');
});
app.get('/*', function (req, res) {
  res.sendFile(path.join(__dirname, 'build', 'index.html'));
});
console.log("DEV: running on port " + port);
app.listen(port);
```
- note: /ping is a meaningless path just for testing whether the node server is up and running

### Install dependencies
- npm install following dependencies:
```
express
express-favicon
path
```

### Configurate package.json script
- now, some changes in object "scripts" within package.json are necessary to be made - in the end, it should look like so: 
```json
"scripts": {
  "prestart": "npm run build", // to build the app before "start"
  "start": "node server.js",  // to run the app as if it was on production
  "devrun": "react-scripts start", // to run the app in development mode
  "build": "react-scripts build", // stays the same as before (builds only React app)
  "test": "react-scripts test", // stays the same as before
  "eject": "react-script eject" // stats the same as before
}
```
- now, use these scripts to run the app:
  - ```npm run start``` 
    - this will run the app as if it was on production
    - head over to browser and find your page under localhost:{{port}}
  - ```npm run devrun``` 
    - this will run the app in development mode
    - browser with your website will open automatically
  - IMPORTANT!!! - pay attention on log in console - it will let you know on which port is the app running

### Prevent sourcecode from being deployed
- in root of repository, create file ```.env``` and add following:
```python
#.env
GENERATE_SOURCEMAP=false
```
- source maps allow to access source code in the production environment, which makes debugging easier; however, source maps also allow anyone from the public to view your source code

### Deploy to heroku
#### Prepare heroku
- login to heroku:
```heroku login```
- in case the project doesn't exist yet on heroku, create it with:
```heroku create {{app_name}}```
#### Deploy
- to make sure all dependencies are installed:
```npm install```
- prepare heroku code repository
    - ```git add .``` track all changes
    - ```git commit -m {{commit_message}}``` commit changes
    - ```heroku git:remote -a {{app_name}}``` add git remote for heroku repository
    - ```git push heroku master``` push code to heroku

> Now, everything is done. You can execute ```heroku deploy``` in terminal to view the website

### Resources
[Tutorial on Medium.com](https://medium.com/jeremy-gottfrieds-tech-blog/tutorial-how-to-deploy-a-production-react-app-to-heroku-c4831dfcfa08#:~:text=%20Tutorial%3A%20how%20to%20deploy%20a%20production%20React,to%20serve%20your%20production%20build%0AIn%20your...%20More%20)