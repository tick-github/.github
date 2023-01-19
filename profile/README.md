# Welcome to *Tick!*

<p align="center">
    <img src="https://github.com/tick-github/.github/blob/main/images/landing-cat.gif"></img>
</p>

Now that I have got your attention with this adorable sushi cat (*thanks [Cindy Suen](https://dribbble.com/shots/13737434-Tamago-Sushi-Cat/attachments/5343321?mode=media)!*), welcome to *Tick!*

*Tick!* is an open-source school project, providing students with a 'one-glace' dashboard to get through their days.

## Running the project

To run the project, you will need `docker` and its `compose` installed (see [here](https://docs.docker.com/engine/install/)). The compose file is as follows:

```yml
name: tick
services:

  eureka-server:
    container_name: eureka-server
    build:
      context: tick-service-discovery
      dockerfile: Dockerfile
    ports:
      - 8761:8761
    networks:
      - eureka
    restart: unless-stopped

  gateway:
    depends_on:
      - eureka-server
    container_name: gateway
    build:
      context: tick-gateway
      dockerfile: Dockerfile
    ports:
      - 8000:8000
    networks:
      - eureka
    restart: unless-stopped

  settings-api:
    depends_on:
      - eureka-server
      - gateway
    container_name: settings-api
    build:
      context: tick-settings-api
      dockerfile: Dockerfile
    networks:
      - eureka
      - settings-network
    restart: unless-stopped

  settings-db:
    depends_on:
      - settings-api
    container_name: settings-db
    image: mongo
    restart: unless-stopped
    networks:
      - settings-network
    volumes:
      - ./data/settings-db/:/data/db
    environment:
      - MONGO_INITDB_DATABASE=settings

networks:
  eureka:
    name: eureka
    driver: bridge
  settings-network:
    name: settings-network
    driver: bridge
```

First, you clone all of the following GIT repositories from this organization:

* Tick Frontend
* Tick Gateway
* Tick Service Discovery
* Tick Settings API

Unzip the files and place them so that you have a file tree like this:

```
root-folder/
    ∟ docker-compose.yml
    ∟ tick-frontend/
        ∟ src/
        ∟ Dockerfile
        ∟ ...
    ∟ tick-gateway/
        ∟ src/
        ∟ Dockerfile
        ∟ ...
    ∟ tick-service-discovery/
        ∟ src/
        ∟ Dockerfile
        ∟ ...
    ∟ tick-settings-api/
        ∟ src/
        ∟ Dockerfile
        ∟ ...
```

Go to root folder and run `docker compose up --build` via your terminal. You may need to start the Docker Daemon first.

The frontend application is now reachable via the default location `http://localhost:4200`.

Then go to the `tick-frontend` folder and run `npm i` and `npm start` via your terminal. Give it a bit of time to install the npm-packages.

The settings API has a paired MongoDB document datastore attached to it. This datastore has data persistence. You may have noticed a new folder in the folder root, called `data`. Herein resides the data for the settings-db.
