# BLOG STAR
 
Installing Jenkins
-   check for a file named dockerfile and rename to Dockerfile. 
    undo the naming after 
- run this command `docker build -t myjenkins-blueoceas:2.332.3-1 .`
    after a successful build, run this command
- `docker network create jenkins`
- Run the container command

    ```docker run --name jenkins-blueocean --restart=on-failure --detach `
  --network jenkins --env DOCKER_HOST=tcp://docker:2376 `
  --env DOCKER_CERT_PATH=/certs/client --env DOCKER_TLS_VERIFY=1 `
  --volume jenkins-data:/var/jenkins_home `
  --volume jenkins-docker-certs:/certs/client:ro `
  --publish 8080:8080 --publish 50000:50000 myjenkins-blueocean:2.414.2```
- Visit localhost:8080
- Jenkins will prompt for password. use this command in console ` docker exec jenkins-blueocean cat /var/jenkins_home/secrets/initialAdminPassword`
- alpine/socat container to forward traffic from Jenkins to Docker Desktop on Host Machine
    `docker run -d --restart=always -p 127.0.0.1:2376:2375 --network jenkins -v /var/run/docker.sock:/var/run/docker.sock alpine/socat tcp-listen:2375,fork,reuseaddr unix-connect:/var/run/docker.sock
  docker inspect <container_id> | grep IPAddress`


