## Angular 2 Frontent with SpringBoot (Java) Backend
Application to demonstrate various parts of a service oriented RESTfull application. 

### Technology Stack
Component         | Technology
---               | ---
Frontend          | [Angular 2](https://github.com/angular/angular) , [Material-Design](https://github.com/angular/material2),[ReactiveX]()  
Backend (REST)    | [SpringBoot 1.4](https://projects.spring.io/spring-boot) (Java)
Security          | Token Based (Spring Security and [JWT](https://github.com/auth0/java-jwt) )
REST Documentation| [Swagger UI / Springfox](https://github.com/springfox/springfox) and [ReDoc](https://github.com/Rebilly/ReDoc)
REST Spec         | [Open API Standard](https://www.openapis.org/) 
In Memory DB      | H2 
Persistence       | JPA
Client Build Tools| [angular-cli](https://github.com/angular/angular-cli), Webpack, npm
Server Build Tools| Maven(Java)
Gateway Service   | [Netflix zuul](https://github.com/Netflix/zuul)
Localization      | <Pending>     

## Folder Structure
```bash
PROJECT_FOLDER
│  README.md
│  pom.xml           
│
└──[src]      
│  └──[java]      
│  └──[resources]
│     └────[public]    #keep all html,css etc, resources that needs to be exposed to user without security
│
└──[config]            #contains SpringBoot application.properties
│
└──[target]            #Java build files, auto-created after running java build: mvn install
│
└──[webui]
   │  package.json     
   │  angular-cli.json #ng build configurations)
   └──[node_modules]
   └──[src]            #frontend source files
   └──[dist]           #frontend build files, auto-created after running angular build: ng -build

```

## Install Instruction

### Prerequisites
Ensure you have this installed before proceeding further
- Java 8       (`sudo apt-get install default-jdk`)
- Maven 3.3.9  (`sudo apt-get install maven`) 
- Node 7.2.1,  (`sudo apt-get install nodejs`) 
- npm 3.9.5,   (`sudo apt-get install npm`) 

- Angular-cli (install using `sudo npm install -g angular-cli@latest`)
- local-web-server ( install using `sudo npm install -g local-web-server`)


## App Installation
- Clone the repo in a folder
- You must follow the installation sequence 
    1. First Install Frontend (WebUI)
    2. Then Install Backend  (Java Springboot)


### Install Frontend (Angular)

```bash
# Navigate to the folder where package.json is present (webui folder)
npm install
# build the project (this will put the files under dist folder)
ng build
```
The above steps will generate `dist` folder under `PROJECT_FOLDER\webui`
while building the Java app content of this folder is copied onto `PROJECT_FOLDER\webui`


### Install Backend (SpringBoot Java)
```bash
# Navigate to the root folder where pom.xml is present 
mvn clean install
```


### Start the API and WebUI server ###
```bash
# Start the server (9119)
# port and other configurations for API servere is in [./cofig/application.properties](https://github.com/mrin9/Angular2_SpringBoot/blob/master/config/application.properties) file
java -jar ./target/sonicwall-1.0.0.jar

```

- Access Server at <http://localhost:9119/index.html>

**Login Credentials**
```
demo:demo
admin:admin
inactive:inactive
```

**To get an authentication token** 

```bash
curl -X POST --header 'Content-Type: application/json' --header 'Accept: application/json' -d '{"username": "demo", "password": "demo" }' 'http://localhost:9119/session'
```
or POST the username and password to http://localhost:9119/session


after you get the authentication token you must provide this in the header for all the protected urls 

```bash
curl -X GET --header 'Accept: application/json' --header 'Authorization: [replace this with token ]' 'http://localhost:9119/version'
```


## Building docker images

if you want to host the application in a container you can use the Dockerfile provided here 
to build a docker image (you should have docker installed)
```bash
# execute this from a folder where Dockerfile is present
docker build -t msw:node_java_python .

# msw:node_java_python is just a tag name to identify the image
# after the image is built , you can run the container as follows
docker run -e "SPRING_PROFILES_ACTIVE=dev" -p 9119:9119 -p 3000:3000 -t msw:node_java_python
```
now you can visit http://localhost:9119
