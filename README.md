# Jenkins Cheatsheet
[Jenkins](https://www.jenkins.io/) is an open source automation server. It is majorly used for CICD pipelines. 

I am writing this cheatsheet while learning about Jenkins from multiple platforms like Udemy, Pluralsight, LinkedIn Learning and multiple articles.
Download Jenkins using homebrew in MacOS and Linux, and exe file from the official website for Windows.

Install Docker and Docker compose
Install jenkins in docker using `docker pull jenkins/jenkins`. 

Docker compose file content
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
Create a directory. Place the docker compose file in it. Create a new directory named `jenkins_home` here. Docker compose is used for making the container persistnt. 
