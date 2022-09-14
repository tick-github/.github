# Welcome to *Tick!*

<p align="center">
    <img src="https://github.com/tick-github/.github/blob/main/images/landing-cat.gif"></img>
</p>

Now that I have got your attention with this adorable sushi cat (*thanks [Cindy Suen](https://dribbble.com/shots/13737434-Tamago-Sushi-Cat/attachments/5343321?mode=media)!*), welcome to *Tick!*

*Tick!* is an open-source school project, providing students with a 'one-glace' dashboard to get through their days.

## Running the project

To run the project, you will need `docker` and its `compose` installed (see [here](https://docs.docker.com/engine/install/)). For now, the compose file is as follows:

```yml
name: tick
services:
 
  tick-frontend:
    container_name: tick-frontend
    restart: unless-stopped
    # for now, the versioning is not implemented. This is being looked at.
    image: ejjvandelaar/tick-frontend:main-df90cb8
    ports:
      - 7973:80
```

Then run the `docker-compose.yml` by executing this command in the directory that contains the `yml` file.

```
docker compose up --build
```