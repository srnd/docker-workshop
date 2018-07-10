# Docker Workshop Notes
## Windows

#### Installing Docker

Open PowerShell and paste this:

```powershell
Invoke-WebRequest -Uri "https://horner.tj/dockerwin" -OutFile "docker.exe"; $env:DOCKER_HOST = "tcp://40.84.140.135:1234"
```

## Linux

#### Installing Docker

Open a terminal and paste this:

```shell
wget https://horner.tj/docker && chmod +x docker && export DOCKER_HOST=tcp://40.84.140.135:1234
```

## macOS

#### Installing Docker

Open a terminal and paste this:

```shell
curl https://horner.tj/docker -o docker && chmod +x docker && export DOCKER_HOST=tcp://40.84.140.135:1234
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

- Windows: `$env:DOCKER_HOST = "tcp://40.84.140.135:1234"`
- Linux/macOS: `export DOCKER_HOST=tcp://40.84.140.135:1234`

This will tell Docker to connect to our hosted Docker daemon.