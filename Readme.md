# Openvas 10 Docker Image

This docker image is based on Openvas 10 but with a few package modifications. After years of successfully using the OpenVAS 8/9 package, maintained by the Kali project, we started having scanning and performance issues. After months of trying to tweak/stablize OpenVAS, with varying and short lived success, we decided to maintain our own modified version of OpenVAS 10 to streamline the installation and cleanup while greatly increasing reliability.

## Deployment

**Install docker**

If you have Kali or Ubuntu you can use the docker.io package.
```
apt install docker.io
```

If you are using any debian based OS that does not have the docker.io package, you can follow [this guide](https://docs.docker.com/install/linux/docker-ce/debian/) 

You can also use the docker install script by running:
```
curl https://get.docker.com | sh
```

**Run our container**

This command will pull, create, and start the container:
```
docker run -d -p 8080:9392 securecompliance/openvas --name openvas
```
You can use whatever `--name` you'd like but for the sake of this guide we're using openvas.

The `-p 8080:9392` switch will port forward 8080 on the host to 9392 (OpenVAS default web interface) in the docker container. Port 8080 was chosen only to avoid conflicts with any existing OpenVAS installation. You can change 8080 to any available port that you'd like.

Depending on your hardware, it can take anyhwere from a few seconds to 10 minutes while the NVTs are scanned and the database is rebuilt. **The default user account is created after this process has completed. If you are unable to login, it means it is still loading (be patient).**

**Checking Deployment Progress**

There is no easy way to estimate the remaining NVT loading time, but you can check if the NVTs have finished loading by running:
```
docker logs openvas
```

If you see "Your OpenVAS container is now ready to use!" then, you guessed it, your container is ready to use.

## Accessing Web Interface

Access web interface using the IP address of the docker host on port 8080 - `https://<IP address>:8080`

```
Username: admin
Password: admin
```

## Monitoring Scan Progress

This command will show you the OpenVAS processes running inside the container:
```
docker top openvas
```

## Checking the OpenVAS Logs

All the logs from /usr/local/var/log/gvm/* can be viewed by running:
```
docker logs openvas
```

## Updating the NVTs

The NVTs will update every time the container starts. Even if you leave your container running 24/7, the easiest way to update your NVTs is to restart the container.
```
docker restart openvas
```
