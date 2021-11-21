# Coolestproject

Main repository for the coolestproject application. Contains the Docker Compose development setup.

## History

During 2019 we decided to create a website to support the organization of Coolest Project (BE). Coolest Project is a annual event where children ages 5 up to 18 can present their project.

The main idea is to allow the participants to register create a project and invite collaborators. The backend component also helps the administration in planning the event by providing reports and we planned tooling for our physical event eg seating charts, voting application, ... But then Corona hit and we shifted to an online event. This change had an impact on the planned features and that is why we stopped working on the extra features and shifted support the new configuration.

In the 2021 release we plan to update the frameworks to the newest versions and continue with the planned features. Main focus is code cleanup and simplification. We are moving to Docker for the development setup and setting up CI/CD deploy pipeline for Azure.

## Application Structure

The application is a JAMstack 3 tier design.

### Database

Any DB supporting Sequelize.

```console
mysql -u coolestproject -p Se84KCCCJlnfkdfv
use database coolestproject;
show tables;
```

### Application server

Based on Swagger with Express, PassportJS, Adminbro, JWT and Sequelize.

- https://swagger.io/tools/swagger-codegen/
- http://www.passportjs.org/
- https://adminbro.com/
- https://jwt.io/
- https://sequelize.org/
- https://expressjs.com/
- https://www.npmjs.com/package/email-templates

### Frontend

Developed in NuxtJS with BootstrapVue.

- https://nuxtjs.org/
- https://bootstrap-vue.org/
- https://www.npmjs.com/package/vuex-persist

## Development Setup

Create a configuration.env file, adapt the mail and secrets variable, NODE_ENV cannot be DEVELOPMENT !! (we have a bug in the mailing lib)

### configuration.env

```.env
EMAIL=info@coolestprojects.be
NODE_ENV=production

LANG=nl

MAIL_HOST=**
MAIL_PORT=**
MAIL_USER=**
MAIL_PASS=**

SECRET_KEY=**

# In seconds
TOKEN_VALID_TIME=172800
TOKEN_RESEND_TIME=1

# needed for CORS headers backend -> frontend
URL=https://app.coolestprojects.localhost:1234
NUXT_ENV_DOMAIN=coolestprojects.localhost
DOMAIN_COOKIE=coolestprojects.localhost

DEBUG=email-templates

SECURE_COOKIE=false
SAMESITE_COOKIE=Lax

AZURE_STORAGE_CONNECTION_STRING=DefaultEndpointsProtocol=https;AccountName=account1.blob.coolestazure;AccountKey=key1;BlobEndpoint=https://account1.blob.coolestazure.localhost:1234;
AZURE_STORAGE_CONTAINER=movies

NODE_TLS_REJECT_UNAUTHORIZED=0
```

```console
docker-compose up
# connect to the backend container
npm install -g .
cli init_db
npx sequelize db:seed:all
npx sequelize db:migrate

```

Testing the emails
Setup Mailtrap or similar, out credentials in configuration.env
```console
docker-compose up
# connect to the backend container
npm install -g .
cli mail welcomeMailOwner 1
cli mail activationMail 2
etc...
```

### URL's
We are using treafik as a proxy server. You can view the configuration:
- <http://localhost:8080>

Main url:
- <http://localhost:1234>

## you need to adapt your hostfile

```console
127.0.0.1  backend.coolestprojects.localhost
127.0.0.1  app.coolestprojects.localhost
127.0.0.1  coolestazure.localhost
```

for PHPMyAdmin you need to lookup the password on in the docker-compose file.

| Name     | Value            |
|----------|------------------|
| Username | coolestproject   |
| password | Se84KCCCJlnfkdfv |

### Backend Administration

Accounts are created with the \*accounts.js seeder file.

| Email | Password |
| ------------------------ | -------- |
| admin@coolestprojects.be | admin |
| super@coolestprojects.be | super |
| jury@coolestprojects.be | jury |

### Created Containers

| Name                                    |
| --------------------------------------- |
| coolestproject-frontend                 |
| coolestproject-backend                  |
| mysql/mysql-server                      |
| phpmyadmin/phpmyadmin                   |
| traefik:latest                          |
| mcr.microsoft.com/azure-storage/azurite |

### Known issues

In case of problems with containers do the following go to the bash terminal of the system and remove the containers

```console
docker stop $(docker ps -a -q)
docker rm $(docker ps -a -q)
```

restart then with the ... console commands above

to check running sql pids

```console
ps aux | grep -i sql
```

Azureite doesn't support Content-Disposition headers.

### Azure setup

- NodeJS Version : v14.15.0

### Installation needed for VSCode Linting

```console
npm -g install eslint-plugin-nuxt
sudo npm -g install eslint
sudo npm -g install babel-eslint
```
