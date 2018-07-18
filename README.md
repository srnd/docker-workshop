# IMPORTANT NOTE: We are no longer hosting the communal Docker daemon. You should get docker from [here](https://www.docker.com/get-docker).

# Docker Workshop Notes
## Installing Docker

### Windows

Open PowerShell and paste this:

```powershell
Invoke-WebRequest -Uri "https://horner.tj/dockerwin" -OutFile "docker.exe"; $env:DOCKER_HOST = "tcp://40.84.236.92:1234"
```

### Linux

Open a terminal and paste this:

```shell
wget https://horner.tj/docker && chmod +x docker && export DOCKER_HOST=tcp://40.84.236.92:1234
```

### macOS

Open a terminal and paste this:

```shell
curl https://horner.tj/dockermac -o docker && chmod +x docker && export DOCKER_HOST=tcp://40.84.236.92:1234
```

## Invoking Docker

```shell
./docker [your-command]
```

If you get an error that looks like this:

```
Cannot connect to the Docker daemon at unix:///var/run/docker.sock. Is the docker daemon running?
```

Then run the following command:

- Windows: `$env:DOCKER_HOST = "tcp://40.84.236.92:1234"`
- Linux/macOS: `export DOCKER_HOST=tcp://40.84.236.92:1234`

This will tell Docker to connect to our hosted Docker daemon.

## Testing Docker

To see if Docker works, run the command `./docker info`. If you don't get any errors, then you're good to go for the rest of this demo!
