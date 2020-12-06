# Coolestproject
Main repository for the coolestproject application. Contains the Docker Compose development setup.

## History

During 2019 we decided to create a website to support the organization of Coolest Project (BE). Coolest Project is a annual event where children ages 5 up to 18 can present their project.

The main idea is to allow the participants to register create a project and invite collaborators. The backend component also helps the administration in planning the event by providing reports and we planned tooling for our physical event eg seating charts, voting application, ... But then Corona hit and we shifted to an online event. This change had an impact on the planned features and that is why we stopped working on the extra features and shifted support the new configuration.

In the 2020 release we plan to update the frameworks to the newest versions and continue with the planned features. Main focus is code cleanup and simplification. We are moving to Docker for the development setup and setting up CI/CD deploy pipeline for Azure.

## Application Structure

The application is a JAMstack 3 tier design.

### Database

Any DB supporting Sequelize.

### Application server

Based on Swagger with Express, PassportJS, Adminbro, JWT and Sequelize.

### Frontend

Developed in NuxtJS with Bootstrap.
