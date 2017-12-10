rpi-node
======

introduction
--------------------------------------

本场景继续探索如何构建 Docker 容器和自动化部署您的应用程序。这个案例将介绍如何在容器中部署一个 Node.js 应用程序。

In this case, an ideal directory would be `/src/app` as the environment user has read/write access to this directory.


Base Image
--------------------

Define a working directory using `WORKDIR <directory>` to ensure that all future commands are executed from the directory relative to our application.

```Dockerfile
FROM node:7-alpine
RUN mkdir -p /src/app
WORKDIR /src/app
```
NPM Install
----------------------------------

Install the dependencies required to run the application
```Dockerfile
COPY package.json /src/app/package.json
RUN npm install
```


Configuring Application
----------------------------

We can copy the entire directory where our Dockerfi le is using `COPY . <dest dir>`.
Once the source code has been copied, the ports the application requires to be accessed is
defi ned using `EXPOSE <port>`.
Finally, the application needs to be started. Once neat trick when using Node.js is to use the
`npm start` command. This looks in the package.json fi le to know how to launch the application
saving duplication of commands.

```Dockerfile
COPY . /src/app
EXPOSE 3000
CMD [ ”npm”, ”start” ]
```

Building Launching Container
-----------------------------------

To launch your application inside the container you fi rst need to build an image.

The command to build the image is

```bash
docker build -t my-nodejs-app .
```

The command to launch the built image is

```bash
docker run -d –name my-running-app -p 3000:3000 my-nodejs-app
```





