# Super Pomodoro

## Requirements

- Docker / Docker desktop
- [Frontend App](https://github.com/DiosDuck/super-pomodoro-frontend)
- [Backend App](https://github.com/DiosDuck/super-pomodoro-backend)

## Installation

Download Backend App and put it inside of a new folder called 'backend'.
Download Frontend App and put it inside of a new folder called 'frontend'.
Example inside the current folder:
```
Super Pomodoro
|
|_backend
|   |_...
|_frontend
|   |_...
|_docker
|   |_...
|_.gitignore
|_docker-compose.yml
|_README.md
```

Run the following commands:

```
docker compose build
docker compose up -d
```

After that use the 'composer' container to execute the following scripts (add sudo if required):
```
# composer install
docker compose --profile tools run --rm php_tools composer install
# update database
docker compose --profile tools run --rm php_tools php bin/console doctrine:migration:migrate
# adding the JWT tokens
docker compose --profile tools run --rm php_tools php bin/console lexik:jwt:generate-keypair
```

Wait till 'frontend' container is finished executing the build, the access the page through `localhost:8081`

## Walkthrough the app

### Locations

- Angular/FE application can be accessed through `localhost:8081` url
- Symfony/BE application is reached through `localhost:8081/api/{url}`, where on `localhost:8081/api/doc` is the Swagger API documentation of all used endpoints
- Mailbox interface can be reached through `localhost:8025`
- MySQL's database can be reacher though `localhost:3307` with credentials `root:root`

### Frontend

- authentication
    - is accessed through the navbar section
    - registration is done by completing the form data, then reaching the mailbox to confirm the account created
    - all registered accounts are done with the role of user (to add another role, manual database changes are required)
    - sign in is done throught the respective page, on success a JWT token is stored in the local storage
- status page/health check
    - only admins are allowed on respective page
    - a list is shown, representing the state of the applicaton and if all the containers work as expected
- pomodoro
    - can be customized through multiple settings in the settings page
    - visually the settings can be changed when a cycle was not started (time progress is not saved through the pages and it should not be done)
    - if logged in, the settings are stored in database for the respective user
    - if no settings are configured, a default set is placed instead
    - a normal pomodoro cycle is structured the following: work -> break -> work -> break... and at X ammount of work sessions a longer break will happen
    - for visual purpose, the number of current work sessions is stored and saved for the respective day, the next day the cycles gets reseted
- profile page
    - can be accessed after signing in
    - there are three sections:
        - profile details
        - pomodor session history in the past 7 days
        - edit user data buttons, which will trigger a dialog to pop up

### Backend

- APIs:
    - all APIs are documented under `/api/doc` Swagger page
- Commands:
    - `app:pomodoro:cleaner`
        - clear pomodoro sessions older than 8 days
    - `app:token:cleaner`
        - clear all tokens generated older than 24 hours, if unused register tokens then delete the respective unactive accounts
