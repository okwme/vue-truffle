# vue-truffle

> Vue.js version of Truffle & Zeppelin's Tutorial [Robust Smart Contracts with Openzeppelin](http://truffleframework.com/tutorials/robust-smart-contracts-with-openzeppelin)

## Build Setup

``` bash
# install dependencies
npm install -g truffle
npm install -g testrpc
npm install

# serve test network at localhost:8545 in a separate terminal
testrpc

# build and deploy contract onto network
truffle compile
truffle migrate

# serve with hot reload at localhost:8080
npm run dev

# build for production with minification
npm run build

# build for production and view the bundle analyzer report
npm run build --report

# run unit tests
npm run unit

# run e2e tests
npm run e2e

# run all tests
npm test
```

For detailed explanation on how things work, checkout the [guide](http://vuejs-templates.github.io/webpack/) and [docs for vue-loader](http://vuejs.github.io/vue-loader).
