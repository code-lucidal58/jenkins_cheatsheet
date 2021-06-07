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

To start a container, `docker-compose start` and to stop it use `docker-compose stop`.
To restart `docker-compose restart jenkins`
To delete the container, `dcoker-compose down`. This will not delete the container data.

## Jenkins UI
* `New Item`: Create a new jenkins job. This has a number of templates.
* `People`: List of people who can access this account along with their permission levels.
* `Build History`: History of builds. Very intuitive :P
* `Credentials`: Manage credentials to be used in the jenkins jobs
* `Manage Jenkins`: Manage the jenkins account.

### Creating First Job
* Click on `New Item`. Give a name to this job and select Freestyle Project.
* Under `Build` section, there is an option `Execute shell`. This will be executed in the same shell as `docker exec -ti jenkins bash` i.e. the container's shell.
* A text box appears. Here type `echo Hello World`. And save.
* On the left side, there will be an option `Build Now`. This will trigger the job and you can see the output in `Console Output` option on the left in the build's page.
Reconfigure the job by clicking `Configure` in the jobs page. Please take not of the words `jobs` and `builds`. Jobs produce builds. In the place you entered shell script, you can use other commands or env variables compatible to the container shell, like `date, who ami, etc`. Few examples are as follows:
```shell
NAME=aanimish
echo "Hello aanimish. Current date is $(date)"
echo "Hello aanimish. Current date is $(date)" > /temp/data
```
The file written in the last line resides in the container.

###
You execute a script file in a job, the file should be present in the container. The Jenkins container does not vi ninstalled in it. To move a file from local system,
to the container use `docker cp <filepath in local system> jenkins:<full filepath in container>`. For example, `docker cp script.sh jenkins:/tmp/script.sh`. 

The script might need parameters from the user. To add parameters to the job, go to `Configure`. Under the general tab, there must be an option similar to `This 
project is parameterised`. Here, the parameters name, default value and description is specified. There a number of different data type variables that can be 
defined. Most commonly used to `string type`. Adding parameters, change the `Build` option in the left side of the job changes to `Build with Parameters`.

To create a list parameter, choose `Choice Parameter`. This creates a dropdown from the user. Mention each choice in a new line while adding parameters. 
While setting boolean parameters, there is a checkboc `Default Value`. If checked, the default value will be True, otherwise no default values. Boolean parameter is shown as checkbox to the user. 
