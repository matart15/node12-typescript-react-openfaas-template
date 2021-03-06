# React.js SPA on OpenFaaS With Dockferfile Template in Static mode

Minimal React.js Single Page Application packaged for OpenFaaS with the dockerfile template and the watchdog in static mode.
The client-side routing is done with React-Router.

The issue is that any page other than the homepage will return a 404 error when requested from the server (when refreshing the page).

A Single Page Application technically only have a single index.html, and should redirect every possible url pointed to the domain at
this index.html located at the root.

This is usually done with a catch-all rule on an Express server or Nginx reverse proxy.

Note that the website is served at https://DOMAIN_NAME/function/react-spa by adding `PUBLIC_URL=/function/react-spa` in .env and
setting the `basename` property passed to the Router component.

## instruction
1. download the template 
```sh
faas-cli template pull https://github.com/matart15/node12-typescript-react-openfaas-template.git
```

2. create new function
```sh
faas-cli new react-spa --lang node12-typescript-react --prefix someprefix
```

3. deploy function
```sh
faas up -f react-spa.yml --skip-push
```

Go to `http://127.0.0.1:8080/function/react-spa` +
