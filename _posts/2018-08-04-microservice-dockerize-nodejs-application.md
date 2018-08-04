---
layout: post
title: "Microservice: Dockerize nodejs application"
categories: backend
image: "https://buddy.works/guides/thumbnails/docker-nodejs-cover.png"
tags: [backend, nodejs, docker, contanier, microservice]
---
## I. Prerequisite
1.  Knowledge node.js basic
2.  Understand Docker basic

## II. Concept
**Docker**: is a tool designed to make it easier to create, deploy, and run applications by using containers. It is a bit like a virtual machine but It is not create a whole operating system.

<!--more-->

**Image**: is a static specification of what the container should be in runtime, including the application code inside the container and runtime configuration settings. Docker images contain read-only layers, which means that once an image is created it is never modified.

**Container**:  A running Docker container is an instantiation of an image. Unlike images, which are read-only, running containers include a writable layer.

**Dockerfile**: is a text document that contains all of the configuration information and commands needed to assemble a container image. With a Dockerfile, the Docker daemon can automatically build a container image.

**Microservice**:

## III. Why you should dockerize your application

## IV. Setup
### 1. Create simple nodejs application
```bash
mkdir simplejs && cd simplejs
npm init -y
npm install express --save
touch index.js
```
Edit index.js file using following code:
```javascript
let express = require('express');
let app = express();
let routes = express.Router();

routes.get('/', function (req, res) {
    res.send('Hello world!!');
});

app.use('/simplejs', routes);

app.listen(3000, () => {
    console.log('running')
});
```
Then you can run your application using:
```bash
node index.js
```
Now, you can open your browser and open http://localhost:3000/simplejs. It will reponse *Hello World!*

### 2. Dockerfile
Create a file called: Dockerfile then put the following code inside.
```yaml
FROM mhart/alpine-node:8.9.4
# Create new image from a base image alpine-node
WORKDIR /app
ADD . /app
# Copy all files to /app folder
RUN npm install
# Install all package: express
EXPOSE 3000
CMD ["node", "index.js"]
```

### 3. Build Image and run application
```bash
docker build -t simplejs:v1.0 .
docker run -p 3000:3000 simplejs:v1.0
```
## V. Publish

# REFERENCE
https://github.com/congnt24/samplejs
