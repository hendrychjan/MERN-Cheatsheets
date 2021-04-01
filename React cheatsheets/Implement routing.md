# Routing in React
### Install dependencies
- following dependencies are necessary:
```
npm i react-router-dom
npm i query-string
```

### Setup index.js
- inside ```index.js``` wrap ```<App />``` with ```BrowserRouter```, just like so:
```javascript
ReactDOM.render(
  <BrowserRouter>
    <App />
  </BrowserRouter>,
  document.getElementById('root')
);
```

### Setup routing itself
- now, wherever you need, you can use ```<Switch>``` for routing 
- assuming you already have components ready, here is example of simple usage of ```<Switch>``` inside App.js:
```javascript
function App() {
  return (
    <div>
      <Navbar />
      <div className="container">
        <Switch>
          <Route path="/home" component={HomePage} />
          <Route path="/about" component={AboutPage} />
          <Route path="/projects" component={ProjectsPage} />
          <Route path="/contact" component={ContactPage} />

          <Redirect from="/" exact to="/home" />
        </Switch>
      </div>
    </div>
  );
}
```

### Using links
- you can now use ```<Link />``` or ```<NavLink />``` to navigate between routes
- ```<Link />``` is used like so:
```jsx
<Link to="{{route goes here}}" />
```
- for example:
```jsx
<Link className="nav-link" to="/home">Home</Link>
```
- to understand the difference between Link and NavLink, head over [here](https://stackoverflow.com/questions/47338077/react-router-v4-navlink-vs-link-benefits)

> This cheatsheet assumes you already know at least basics of how routing in React works. If you dont have such knowledge, I recommend heading over to [here](https://medium.com/the-andela-way/understanding-the-fundamentals-of-routing-in-react-b29f806b157e), so you can learn it. 