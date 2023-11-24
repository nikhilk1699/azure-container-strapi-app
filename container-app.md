## 🚀 deploy strapi in azure containers

github link: https://github.com/nikhilk1699/azure-container-strapi-app.git

- install java openjdk-11
- install docker
- install nodesjs
- install yarn
-  create strapi application using yarn 

### create Dockerfile
```
FROM node:16

# Installing libvips-dev for sharp Compatibility

RUN apt-get update && apt-get install libvips-dev -y

ARG NODE_ENV=development

ENV NODE_ENV=${NODE_ENV}

WORKDIR /opt/

COPY ./package.json  ./

ENV PATH /opt/node_modules/.bin:$PATH

RUN npm install

WORKDIR /opt/app

COPY ./ .

RUN npm run build

EXPOSE 1337

CMD ["npm", "run", "develop"]
```
- the base image for your container is the official Node.js image version 16.
- updates the package index inside the container and installs the libvips-dev package.
- the Node.js environment to the default value "development" or any value passed during the build process using the --build-arg option.
- sets the working directory inside the container to /opt/.
- copies the package.json file from the local directory to the working directory in the container.
- node_modules/.bin directory to the PATH environment variable, allowing you to run locally-installed npm binaries without specifying their full path.
- installs the Node.js dependencies based on the information provided in the package.json file.
- the working directory to /opt/app/
- copies the local application code to the current working directory inside the container.
- runs the specified build command for your Node.js application.
- exposes port 1337 from the container, indicating that the application inside the container will be listening on this port.

## create image from Dockerfile
```
$ docker build -t strapi:latest .
````
## push the image to azure container registries
```
$ az acr login --name nikhilstrapi # login to azure container registry
$ docker tag strapi:latest nikhilstrapi.azurecr.io/strapi:latest # tags the local Docker image named "strapi:latest" with a new tag and registry location
$ docker push  nikhilstrapi.azurecr.io/strapi:latest #  push docker image to the Azure Container Registry.
```
![image](https://github.com/nikhilk1699/azure-container-strapi-app/assets/109533285/68658bc5-34ca-426f-a4e4-9ad982d312f6)

## create azure Container app
- create resouce group or use existing resources group: nstrapi.  
- Container app name :n-strapi. # container app is set to n-strapi.
- region: East US
- create new Container Apps Environment: constrapi # deploying, managing, and scaling containerized applications.
- add Container details:
* Image source: azure container resgistry # container image is sourced from an Azure Container Registry (ACR).
* Registry : nikhilstrapi.aurecr.io # store and manage container images.
* Image: strapi # The container image
* Image tag: latest
- enabled ingress # Ingress is enabled to allow external traffic to reach your application.
* Ingress traffic: accept traffic from anywhere
* Ingress type: HTTP # HTTP is specified as the ingress type, indicating that the application communicates over the HTTP protocol.
* Client certificate mode: ignore # the application does not require clients to present certificates for authentication.
* Target port: 1337 #uses port 1337 for HTTP communication.
![image](https://github.com/nikhilk1699/azure-container-strapi-app/assets/109533285/bf52f339-9423-4e94-9a79-13afdc422037)

![image](https://github.com/nikhilk1699/azure-container-strapi-app/assets/109533285/b4977586-dc2e-468b-b1a0-d475ce53828f)

![image](https://github.com/nikhilk1699/azure-container-strapi-app/assets/109533285/cc590a58-cc52-4c3b-a68e-f39c2e03f6b5)

- copy the Application Url: https://n-strapi.greensand-dc0350e4.eastus.azurecontainerapps.io/

![image](https://github.com/nikhilk1699/azure-container-strapi-app/assets/109533285/de98cc5a-c713-49cc-8141-c90a262bac6b)

  






