# Hekp with debugging
If your are facing any issues while setting up or using Jenkins, I have tried to cover those in this file.

### Jenkins container does not come up
Use `docker ps` to see all the containers that are up. If jenkins container is not up, use `docker ps -a` to see the exit code. 
Use `docker logs jenkins` to see volume permission errors.

Inside of the Jenkins container, there's a user named "jenkins" which has a Linux uid of 1000. You are mounting a docker volume to 
`/var/jenkins_home` which is the home directory of that user. If the directory doesn't have 1000 permissions, then the user won't be able to write/delete files, 
which causes the container to exit.
