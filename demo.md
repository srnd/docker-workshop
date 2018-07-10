# Docker Demo

We'll be running a few commands to get a feel for Docker and containers.

## Hello World

Docker provides a "hello world" container to test your Docker installation and to get a feel for running containers.

```shell
./docker run --rm hello-world
```

- `run` is a Docker command that tells it to start a container with the specified image
- `--rm` is a flag that tells Docker to remove the container after it is done running
- `hello-world` is the image we are using with the `run` command

## Ubuntu

Let's try something a bit more fun by running a full Ubuntu image on Docker!

```shell
./docker run --rm -it ubuntu
```

- You might notice we added `-it` to this command. This is actually a combination of 2 flags: `i` tells Docker to keep the shell interactive and `t` tells it to allocate a TTY. This lets us actually type commands in the container.

You should be dropped into a new shell. Try running `uname -a`. This is a real Linux system! Play around a bit with commands then run `exit` to get out of the shell. Since we specified `--rm`, the container will automatically be removed for us.

## Docker Chat

By default, Docker containers share the same virtual network, which means they can talk to each other! Since we are all running from the same Docker daemon, they all share the same network. So, to demonstrate this, we can use Docker Chat! Run this command to start up Docker Chat, which will let you talk to other containers also running Docker Chat:

```sh
./docker run --rm -it tjhorner/docker-chat
```

- The image name is a bit different this time. It includes my username, which indicates that it's a community image and not an official one. You can view the image on [Docker Hub](https://hub.docker.com/r/tjhorner/docker-chat/).

No server is required since it's all peer-to-peer. If anyone else is on at the moment, you will see their chat messages.

# Dockerfiles

Dockerfiles are sets of commands that tell Docker how to build images. Let's take a look at Docker Chat's `Dockerfile`.

```dockerfile
FROM node:carbon

WORKDIR /app

COPY package*.json ./
RUN npm install

COPY . .

CMD [ "node", "index" ]
```

It's relatively small! Images are built step by step, with a temporary container associated with each. Let's go through each instruction.

- `FROM` tells Docker to use a certain image as a base. In this case, we are using `node:carbon`, which is the official image for Node.js. Since node is already installed for us, we don't need to do anything extra except copy our code.
- `WORKDIR` tells Docker which directory to use as the working directory while building the image. In this case, we are using `/app`, but it can be anything as long as it exists.
- `COPY` tells Docker to copy some files to your image from your host's working directory. We are copying our `package.json` and `package-lock.json` so we can install the dependencies later.
  - After our dependencies are installed, we also copy the rest of the code (`.` refers to our current working directory)
- `RUN` tells Docker to run a command inside of a temporary container with your image so far. In this case, we are running `npm install` to ensure that our dependencies are satisfied.
- `CMD` tells Docker which command to run inside of the container when it starts up. In this case, it's `node index`.

You might have noticed something: we are copying our `package.json` separately from the rest of the code. Why? Docker has "smart" building, so it will only re-run the first `COPY` and consequently `RUN` if our host's `package.json` has changed. This avoids having to re-install dependencies over and over again, even if they haven't changed.

# Docker Compose

Docker Compose is one of my favorite Docker features. It lets you declare services you want to run in containers, and with a simple `docker-compose up`, it creates them how you'd like and orchestrates everything automatically.

Services are defined in a `docker-compose.yml` file, and looks something like this:

```yaml
version: '3'
services:
  web:
    build: .
```

This is a very simple Docker Compose file. It's telling Docker Compose to create a container for a service named `web` that is built locally from the theoretical `Dockerfile`.

If you want to get more complex, you can do stuff like this:

```yaml
version: '3'
services:
  bot:
    build: .
    restart: unless-stopped
    depends_on:
      - db
    environment:
      - PRODUCTION=true
      - BOT_ROOT=/home/tj/CompileBot
      - MONGO_URL=mongodb://db/compilebot
      - WEB_ROOT=https://compilebot.horner.tj
    volumes:
      - ./temp:/app/temp
      - /var/run/docker.sock:/var/run/docker.sock
  web:
    build: ./web
    restart: unless-stopped
    ports:
      - "21732:3000"
    environment:
      - MONGO_URL=mongodb://db/compilebot
    depends_on:
      - db
  db:
    image: mongo:3.2.20-jessie
    restart: unless-stopped
    volumes:
      - db:/data/db

volumes:
  db:
```

This is actually the the `docker-compose.yml` file for [my Telegram bot that compiles code](https://t.me/CompileBot), and it's relatively complex. Here we see 3 services defined: `bot`, `web`, and `db`. Each service has a bunch of parameters that, without Docker Compose, I'd have to type manually every time or put in a script (which wouldn't be pretty). Instead, you just define all the parameters you want for each container in a single file and Docker Compose maintains those.

# Further Reading

If you'd like to learn more about Docker (I recommend you do!), you can follow these wonderful guides to gain a better understanding:

- [Official Docker getting started guide](https://docs.docker.com/get-started/)
- [Play with Docker](https://training.play-with-docker.com/)
- [Docker Labs](https://github.com/docker/labs)