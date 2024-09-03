## Repository

https://github.com/mdisumma/docker-app

## Resoucers

https://docs.docker.com/get-started/

# Project plan

## Repository set up

Create and initialise an app with next.js called docker-app ✅
Create a repository on gitHub and publish the docker-app file ✅
Add team member to the project repository ✅
Run the application and test the settings ✅

## Docker set up

Install the Docker VSCode Plugin
Create a Dockerfile in your application's folder at the top level

Fill in the Dockerfile 
 - What base image do you need? (maybe node:18)
    - FROM
 - Have you copied in stuff like package.json and package-lock.json?
    - WORKDIR
 - Have installed your npm packages?
    - RUN
 - Have you copied in the rest of your application files?
    - COPY
 - Have you exposed the right port?
    - EXPOSE
 - Have you set up a default command to run the application when someone tries to start a running container?
    - CMD

run the `docker build` command from the same folder as the Dockerfile.
    (you should see that the docker build command is successful and if you go to Docker Desktop you should see it in the "Images" with the name you've tagged it with.)

If all of that has worked then try to "docker run" your container. You might need to think about "port forwarding" 

goto https://localhost/3000

## Objective 2

## Working with Husky and Linting
 - Install Husky
 - Run Husky
