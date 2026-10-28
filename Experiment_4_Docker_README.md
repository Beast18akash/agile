# ASD & DevOps --- Experiment 4: Docker

**Experiment title:** To containerize applications using Docker\
**Aim:** To containerize applications using Docker.

> This guide is based on the Experiment 4 material in your lab manual.
> The manual focuses on Docker architecture, container lifecycle,
> installing Docker Desktop, and essential commands for images and
> containers. Follow the steps in order and verify the outputs on the
> college PC.

## 1. Check whether Docker is installed

Open **Command Prompt** and run:

``` cmd
docker --version
```

Then run:

``` cmd
docker info
```

-   If `docker --version` prints a version and `docker info` displays
    server details, Docker is installed and running. Continue to Section
    3.
-   If the command is not recognized, Docker is not installed or is not
    on PATH. Continue to Section 2.
-   If the client version appears but `docker info` says it cannot
    connect to the daemon, open Docker Desktop and wait until it says
    Docker is running.

## 2. Install Docker Desktop (only if needed and permitted)

1.  Open the official Docker Desktop page:
    <https://www.docker.com/products/docker-desktop/>.
2.  Download Docker Desktop for Windows.
3.  Run the installer. Keep the recommended options unless your lab
    instructor specifies otherwise.
4.  Restart Windows if the installer requests it.
5.  Open **Docker Desktop** from the Start menu.
6.  Wait until Docker Desktop reports that the engine is running.
7.  Open Command Prompt and verify:

``` cmd
docker --version
docker info
```

**College PC note:** Docker Desktop may require administrator
permission, virtualization support, and WSL 2. If installation is
blocked, ask the lab instructor or lab administrator. Do not try to
bypass PC restrictions.

## 3. Download a test image

Run:

``` cmd
docker pull hello-world
```

Expected result: Docker downloads the `hello-world` image. If it says
the image is up to date, that is also fine.

## 4. Run a container

Run:

``` cmd
docker run hello-world
```

Expected result: output includes a message similar to:

``` text
Hello from Docker!
This message shows that your installation appears to be working correctly.
```

This verifies that Docker can download an image and run a container.

## 5. List Docker images

Run:

``` cmd
docker images
```

Expected result: the image list includes `hello-world`.

## 6. List containers

Run:

``` cmd
docker ps
```

This shows currently running containers. The `hello-world` container
usually exits after printing its message, so it may not appear here.

Now run:

``` cmd
docker ps -a
```

Expected result: the completed `hello-world` container appears with an
`Exited` status.

## 7. Create and run a simple application container

This optional demonstration uses the official Nginx image to serve a web
page.

### A. Download and run Nginx

``` cmd
docker pull nginx
docker run -d --name exp4-nginx -p 8081:80 nginx
```

Expected result: Docker prints a container ID.

### B. Verify the container

``` cmd
docker ps
```

Expected result: `exp4-nginx` appears as running, with port mapping
`8081->80`.

### C. Open the application

Open a browser and visit:

`http://localhost:8081`

Expected result: the **Welcome to nginx!** page appears.

### D. Stop and remove the container

After verifying the page, run:

``` cmd
docker stop exp4-nginx
docker rm exp4-nginx
```

If Docker reports that the name is already in use before you start the
container, check it with `docker ps -a`. If it is your old `exp4-nginx`
container, stop and remove it before repeating the run command.

## 8. Essential Docker commands for viva

  Command                     Purpose
  --------------------------- ----------------------------------------------
  `docker --version`          Shows Docker CLI version
  `docker info`               Shows Docker engine/system information
  `docker pull hello-world`   Downloads an image
  `docker images`             Lists local images
  `docker run hello-world`    Creates/runs a container from an image
  `docker ps`                 Lists running containers
  `docker ps -a`              Lists all containers, including stopped ones
  `docker start CONTAINER`    Starts an existing stopped container
  `docker stop CONTAINER`     Stops a running container
  `docker rm CONTAINER`       Removes a container
  `docker rmi IMAGE`          Removes an image when it is no longer in use

## 9. Final verification checklist

-   [ ] Docker CLI version displayed successfully.
-   [ ] Docker engine is running (`docker info` succeeds).
-   [ ] `hello-world` image downloaded or reported up to date.
-   [ ] `docker run hello-world` displayed the success message.
-   [ ] `docker images` displayed the image.
-   [ ] `docker ps -a` displayed the completed container.
-   [ ] If the Nginx demonstration was performed,
    `http://localhost:8081` displayed the Welcome page.
-   [ ] Screenshots of commands and outputs were saved for the journal.

Do not mark a step complete until you have verified its output.

## 10. Conclusion

Docker was used to download an image and run it as a container. Docker
commands were used to inspect images and containers. The optional Nginx
demonstration showed how a containerized web application can be accessed
through a browser.

## Troubleshooting

-   **`docker` is not recognized:** Docker Desktop may not be installed,
    or the terminal needs to be reopened after installation.
-   **Cannot connect to the Docker daemon:** Open Docker Desktop and
    wait for the engine to start.
-   **Docker Desktop fails to start:** Check the error message; WSL 2,
    virtualization, permissions, or a restart may be required. Ask the
    lab administrator on a managed PC.
-   **Port 8081 is already in use:** Use a different host port, such as
    `8082`, with `-p 8082:80`, and open `http://localhost:8082`.
-   **Image download fails:** Check the internet connection and Docker
    Hub access. Do not repeatedly retry without checking the error.

## Official reference

-   Docker Desktop: <https://www.docker.com/products/docker-desktop/>
