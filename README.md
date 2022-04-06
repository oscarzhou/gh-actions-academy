# Welcome! 

This is the sample code for the [GitHub Partner Workshop on Snyk Academy](https://solutions.snyk.io/partner-workshops/github).

It uses Snyk's [Goof](https://snyk.io/test/github/snyk/goof) vulnerable demo app. More on Goof below: 

# Goof - Snyk's vulnerable demo app
[![Known Vulnerabilities](https://snyk.io/test/github/snyk/goof/badge.svg?style=flat-square)](https://snyk.io/test/github/snyk/goof)


# Goof - Snyk's vulnerable demo app
A vulnerable Node.js demo application, based on the [Dreamers Lab tutorial](http://dreamerslab.com/blog/en/write-a-todo-list-with-express-and-mongodb/).

[![Known Vulnerabilities](https://snyk.io/test/github/snyk/goof/badge.svg?style=flat-square)](https://snyk.io/test/github/snyk/goof)

## Features

This vulnerable app includes the following capabilities to experiment with:
* [Exploitable packages](#exploiting-the-vulnerabilities) with known vulnerabilities
* [Docker Image Scanning](#docker-image-scanning) for base images with known vulnerabilities in system libraries
* [Runtime alerts](#runtime-alerts) for detecting an invocation of vulnerable functions in open source dependencies

## Running

Clone the repo to your local environment then run the following to start the app. 
```bash
mongod &

npm install
npm start
```
This will run Goof locally, using a local mongo on the default port and listening on port 3001 (http://localhost:3001).

## Running with docker-compose
```bash
docker-compose up --build
docker-compose down
```

### Cleanup
To bulk delete the current list of TODO items from the DB run:
```bash
npm run cleanup
```

## Exploiting the vulnerabilities

This app uses npm dependencies holding known vulnerabilities.

Here are the exploitable vulnerable packages:
- [Mongoose - Buffer Memory Exposure](https://snyk.io/vuln/npm:mongoose:20160116) - requires a version <= Node.js 8. For the exploit demo purposes, one can update the Dockerfile `node` base image to use `FROM node:6-stretch`.
- [st - Directory Traversal](https://snyk.io/vuln/npm:st:20140206)
- [ms - ReDoS](https://snyk.io/vuln/npm:ms:20151024)
- [marked - XSS](https://snyk.io/vuln/npm:marked:20150520)

The `exploits/` directory includes a series of steps to demonstrate each one.

## Docker Image Scanning

The `Dockerfile` makes use of a base image (`node:6-stretch`) that is known to have system libraries with vulnerabilities.

To scan the image for vulnerabilities, run:
```bash
snyk container test node:6-stretch --file=Dockerfile
```

To monitor this image and receive alerts with Snyk:
```bash
snyk container monitor node:6-stretch
```

## Runtime Alerts

Snyk provides the ability to monitor application runtime behavior and detect an invocation of a function is known to be vulnerable and used within open source dependencies that the application makes use of.

To run the Node.js app with runtime monitoring, visit [Install the Snyk Runtime Monitoring agent for Node.js](https://support.snyk.io/hc/en-us/articles/360003699058-Snyk-runtime-monitoring-install-the-Snyk-agent-for-Node-js).

## Fixing the issues
To find these flaws in this application (and in your own apps), run:
```
npm install -g snyk
snyk wizard
```

In this application, the default `snyk wizard` answers will fix all the issues.
When the wizard is done, restart the application and run the exploits again to confirm they are fixed.
