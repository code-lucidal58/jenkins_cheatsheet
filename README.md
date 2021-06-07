# Jenkins Cheatsheet
[Jenkins](https://www.jenkins.io/) is an open source automation server. It is majorly used for CICD pipelines. 

I am writing this cheatsheet while learning about Jenkins from multiple platforms like Udemy, Pluralsight, LinkedIn Learning and multiple articles.
Download Jenkins using homebrew in MacOS and Linux, and exe file from the official website for Windows.

## Installation of Jenkins

### Prerequisites
* Install Docker and Docker compose
* Pull docker images using `docker pull jenkins/jenkins`. 

### Steps for jenkins installation using docker
* Create a docker compose file that has the following content. `volumes` should have the path where the data from the container is to be stored. 
Docker compose is used for making the container persistent. 
```
version: '3'
services:
  jenkins:
    container_name: jenkins
    image: jenkins/jenkins
    ports:
      - "8080:8080"
    volumes:
      - $PWD/jenkins_home:/var/jenkins_home
    networks:
      - net
networks:
  net:
```

* Create a directory `jenkins`. Place the docker compose file in it. 
* Create a new directory named `jenkins_home` inside `jenkins` dir. 
* Inside `jenkins` dir, do the following:
  * `sudo chown 502:502 jenkins_home -R` give permissions to write to the folder.
  * `docker-compose up -d` spin up the container
  * `docker logs -f jenkins` gives you password

* Hit localhost:8080 in a browser and login using this password.
* The next screen asks for which plugins to install. Choose custom or recommended install (as per your wish). This takes a while.
* After installation is complete, Jenks will ask you to create the first admin account. Assign username and password to the admin account. It will also ask for Jenkins URL. Preferrable leave as it is.
* Once setup is complete, Jenkins might ask you to login using the admin credentials.
