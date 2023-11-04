# weather-db-backup

## About this repository

Docker container with cronjob, that runs python script once a day. The script creates pg_dump of database that is running on the same docker network, zips it and uploads it to google drive.

## How to run this

-   Create .env file - see `.example.env`
-   Create service account and get the [JSON key](https://medium.com/@matheodaly.md/using-google-drive-api-with-python-and-a-service-account-d6ae1f6456c2) file
-   Place the JSON key file in the same directory as the docker-compose and rename it to `SA_key.json` (can be change in docker-compose)
-   Add the image to your docker-compose:

```yaml
services:
    db-backup:
        image: ghcr.io/weather-blade/weather-db-backup:latest
        restart: unless-stopped
        depends_on:
            - db
        environment:
            DATABASE_URL: ${DATABASE_URL}
            GDRIVE_FOLDER_ID: ${GDRIVE_FOLDER_ID}
        volumes:
            - ./SA_key.json:/app/SA_key.json:ro

    db:
        image: postgres:latest
```

-   Run Docker

```sh
docker compose up
```
