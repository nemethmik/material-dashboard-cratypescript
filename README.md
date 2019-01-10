# material-dashboard-cratypescript
A port of Creative Tim's material-dashboard-react over to a project initialized with CRA/TypeScript

## Installation
- npx create-react-app . --typescript
- npm install @material-ui/core @material-ui/icons @types/googlemaps @types/markerclustererplus ajv chartist classnames npm-run-all perfect-scrollbar react@next react-chartist react-dom@next react-google-maps react-router-dom @types/react-router-dom react-swipeable-views @types/history
- Copy the **.env** file from [Creative Tim's material-dashboard-react](https://github.com/creativetimofficial/material-dashboard-react) to get **NODE_PATH=./src** this is important since Createive Tim imports modules without dot-slash "./", and comment out the ignoration of .env in .gitignore: **#.env**
- Copy all the folders including their contents from the original Creative Tim project: **assests, components, layouts, routes, variables, views**.
- Optionally, delete App.css, App.test.tsx, App.tsx, index.css; of course, you can keep them and adjust App.tsx instead of index.tsx.
- Copy the imports and ReactDOM.render block into **index.tsx** from the index.js of the original material-dashboard-react, but don't copy the file index.js itself at all, it would cause confusion, our entry point is index.tsx:
```
import "assets/css/material-dashboard-react.css?v=1.5.0";
import { createBrowserHistory } from "history";
import { Router, Route, Switch } from "react-router-dom";
import indexRoutes from "./routes/index.jsx";
const hist = createBrowserHistory();
ReactDOM.render(
  <Router history={hist}>
    <Switch>
      {indexRoutes.map((prop:any, key:any) => {
        return <Route path={prop.path} component={prop.component} key={key} />;
      })}
    </Switch>
  </Router>,
  document.getElementById("root")
);
```
- Add a dot-slash to **"./routes/index.jsx"**. This is the only place where this have to be done. Actually, to me this is really weird, that we don't have to add dot-slash to assets/css/material-dasboard-react.css, and "./src/" doesn't work either, maybe because of the NODE_PATH=./src in .env. Anyhow, it works with dot-slash. 
- Add explicit type definitions to prop and key: **indexRoutes.map((prop:any, key:any)**. Creative Tim's teplates projects have jsx files, which are not enforced to have strickt typing; but **index.tsx** is expected to be a nicely implemented TypeScript program. 
- From index.html of the original Creative Tim project copy the Apple touch icon, Chartist, Roboto font and Material Icon links into our new index.html
```
    <link rel="apple-touch-icon" sizes="76x76" href="%PUBLIC_URL%/apple-icon.png"/>
    <link rel="stylesheet" href="//cdn.jsdelivr.net/chartist.js/latest/chartist.min.css"/>
    <script src="//cdn.jsdelivr.net/chartist.js/latest/chartist.min.js"></script>
    <link rel="stylesheet" href="https://fonts.googleapis.com/css?family=Roboto:300,400,500,700|Material+Icons"/>
    <link href="https://fonts.googleapis.com/icon?family=Material+Icons" rel="stylesheet"/>
```

## Handling isolatedModules TypeScript Issues
These are modifications enforced by TypeScript: Cannot compile namespaces when the '--isolatedModules' flag is provided
The tsconfig option isolatedModules cannot be disabled in a CRA/TypeScript project. Fortunately, modifying a couple of lines would solve the issue:
- In charts.jsx
  - Replace var Chartist = require("chartist"); with **import * as Chartist from 'chartist';**
  - Add the export word before all the three charts to be exported **export const dailySalesChart**
  - Comment out the **module.exports ...** block at the bottom.
- In general.jsx
  - Add the export word before all the three vars to be exported **export var bugs**
  - Comment out the **module.exports ...** block at the bottom of the file.


