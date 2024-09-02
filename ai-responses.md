## Objective 1

# Step-by-Step Plan to Set Up a Next.js Application with Docker

## Step 1: Create a New Git Repository

1. **One Team Member Creates an Empty Repository**  
   - **What is a Repository?**  
     A repository (or "repo") is like a special folder where all your code and project files are stored. It keeps track of all the changes you make to your code.
   - **Why Create an Empty Repository?**  
     Starting with an empty repo gives everyone on your team a central place to store and work on the project together.
   - **How to Create a Repository?**  
     One team member goes to a platform like GitHub and creates a new, empty repository. After that, they invite the rest of the team as collaborators so everyone can access and work on the same code.

## Step 2: Clone the Repository and Create a Next.js Application

1. **Clone the Empty Repository**  
   - **What Does "Clone" Mean?**  
     Cloning means making a copy of the repository from GitHub (or another platform) onto your computer. This way, you have all the files and changes locally.
   - **How to Clone?**  
     Use the `git clone <repository_url>` command in your terminal, where `<repository_url>` is the URL of the GitHub repository. This will create a local copy of the repo on your computer.

2. **Create a New Next.js Application**  
   - **What is Next.js?**  
     Next.js is a tool that helps you build web applications easily. It provides a structure and tools that make coding faster and simpler.
   - **How to Create a Next.js App?**  
     Run the following command in your terminal inside the cloned repository folder:  
     ```bash
     npx create-next-app@latest ./
     ```
   - **Why Use `npx` and `create-next-app`?**  
     `npx` is a tool that helps you run commands without needing to install them first. `create-next-app` is a command that sets up a new Next.js application with all the files and settings you need.

3. **Answer the Installer Questions**  
   - The Next.js installer will ask you some questions. Here’s how you should answer:  
     - **TypeScript:** No  
     - **ESLint:** Yes  
     - **All Other Questions:** Use the default answers by pressing Enter  
   - **Why These Choices?**  
     These answers set up the project in a simple way that will work for everyone on the team, without adding extra complexity.

4. **Run and Test the Application**  
   - **How to Run the Application?**  
     Run the following command in your terminal to start the application:  
     ```bash
     npm run dev
     ```
   - **Check in the Browser**  
     Open your web browser and go to [http://localhost:3000](http://localhost:3000). If everything is set up correctly, you should see your new Next.js application running.

## Step 3: Containerize the Application Using Docker

1. **Install Docker and Docker VSCode Plugin**  
   - **What is Docker?**  
     Docker is a tool that lets you package your application and all its dependencies into a single "container." Think of a container as a box that holds your entire application, so it can run consistently on any computer.
   - **Why Use the Docker VSCode Plugin?**  
     The Docker plugin in Visual Studio Code (VSCode) helps you manage Docker containers and files more easily. It provides helpful shortcuts and visual tools for beginners.

2. **Create a `Dockerfile` in Your Application's Folder**  
   - **What is a Dockerfile?**  
     A `Dockerfile` is like a recipe that tells Docker how to create a container for your application. It contains all the steps needed to set up the environment, install dependencies, and start the app.
   - **Where to Place the `Dockerfile`?**  
     Create the `Dockerfile` at the top level of your project folder (the same place where the `package.json` file is).

3. **Write Your Dockerfile Step-by-Step**

   Here’s a guide to writing your Dockerfile:

   - **Choose a Base Image**  
     At the top of the Dockerfile, write:  
     ```Dockerfile
     FROM node:18
     ```
     - **Why `node:18`?**  
       `node:18` is an official image that has Node.js version 18 installed, which is required to run Next.js.

   - **Copy Necessary Files**  
     Add the following lines to copy the essential files into the Docker image:  
     ```Dockerfile
     COPY package.json package-lock.json ./
     ```
     - **Why Copy `package.json` and `package-lock.json`?**  
       These files list all the dependencies (other libraries) your application needs to run.

   - **Install npm Packages**  
     Add this line to install all the necessary packages:  
     ```Dockerfile
     RUN npm install
     ```
     - **Why Install npm Packages?**  
       `npm install` installs all the packages listed in `package.json` that your app needs to run.

   - **Copy the Rest of Your Application Files**  
     Add this line to copy the rest of your application files into the image:  
     ```Dockerfile
     COPY . .
     ```
     - **Why Copy the Rest of the Files?**  
       To ensure all your application files are included in the container.

   - **Expose the Correct Port**  
     Add this line to expose the port that your application uses:  
     ```Dockerfile
     EXPOSE 3000
     ```
     - **Why Expose Port 3000?**  
       Your application is set to run on port `3000`, so you need to make this port accessible from outside the container.

   - **Set the Default Command to Run the Application**  
     Add this line to set the default command that will run when the container starts:  
     ```Dockerfile
     CMD ["npm", "run", "dev"]
     ```
     - **Why Use This Command?**  
       `npm run dev` starts the Next.js application in development mode, which is suitable for testing and debugging.

## Step 4: Build the Docker Image

1. **Run the `docker build` Command**  
   - **How to Build the Docker Image?**  
     Open your terminal and navigate to the folder where the `Dockerfile` is located. Run the following command:  
     ```bash
     docker build -t my-nextjs-app .
     ```
     - **What Does This Command Do?**  
       - `docker build` tells Docker to create a new image based on the `Dockerfile`.
       - `-t my-nextjs-app` names (tags) the image as `my-nextjs-app`.
       - `.` means to look for the `Dockerfile` in the current directory.

2. **Check if the Docker Image is Created**  
   - **How to Check?**  
     Go to Docker Desktop and look under the "Images" tab. You should see the image with the name you gave it (`my-nextjs-app`).

## Step 5: Run the Docker Container

1. **Run the Docker Container Using `docker run` Command**  
   - **How to Run the Container?**  
     Run the following command in your terminal:  
     ```bash
     docker run -p 3000:3000 my-nextjs-app
     ```
     - **What Does This Command Do?**  
       - `docker run` starts a new container from the image.
       - `-p 3000:3000` maps your computer's port `3000` to the container's port `3000` so you can access it in your browser.
       - `my-nextjs-app` is the name of the image you want to run.

2. **Check if Your Application is Running**  
   - **Open Your Browser**  
     Go to [http://localhost:3000](http://localhost:3000). If your application is running, you have successfully containerized it!

## Step 6: Discuss and Understand What You've Done So Far

1. **Discuss with Your Team**  
   Before moving on, talk with your team about what you have done and what you've learned. Make sure everyone understands the steps and why they were necessary.

2. **Understand Docker "Layers" and "Caching"**  
   - **What are Docker Layers?**  
     Each line in your `Dockerfile` creates a "layer." Docker uses these layers to build images efficiently. If a layer hasn't changed, Docker can reuse it, which speeds up the build process.
   - **What is Docker Caching?**  
     Caching is like remembering things. Docker remembers layers from previous builds, so if you don't change something, it doesn't need to redo it. This makes building images faster!

---

By following these steps, you’ll learn how to set up a collaborative coding environment, create a new Next.js application, containerize it with Docker, and run it successfully. This practice will help you understand how to build, run, and manage web applications more efficiently! 🎉

Happy coding! 😊


## Objective 2

# Step-by-Step Plan to Debug Docker Application Changes

## **Step 1: Understand the Current Situation**

1. **What Do You Have?**  
   You have a running application inside a Docker container. A Docker container is like a little virtual computer that runs an app in isolation from your actual computer. Think of it as a sandbox where your application runs safely.

2. **What is Port Forwarding?**  
   - The application is running inside the container on port `3000`. Ports are like doors through which data comes in and out of your computer or a container.
   - The container is "exposing" port `3000`, which means it’s making that port available for communication.
   - Your computer's port `3000` is being forwarded to the container's port `3000`. This means if you go to `localhost:3000` in your browser, you’re actually talking to the app running inside the Docker container!

## **Step 2: Experiment with File Changes**

1. **Make a Change to `page.js` in VSCode**  
   Open the `page.js` file in Visual Studio Code (VSCode) and make a small change, like editing some text in your app.

2. **Check if the Change is Reflected**  
   Normally, when you run `npm run dev`, the application should automatically detect changes to files and rebuild itself. But in this case, it doesn't. You need to figure out why this is happening.

3. **Research Why the Change Isn’t Showing Up**  
   Before reading further, try to think about why your change didn't show up. Could it be that the Docker container isn’t aware of the file changes? Research online if you’re unsure.

## **Step 3: Test the Theory by Editing Files Inside the Container**

1. **"Exec" Into the Container**  
   - **What Does "Exec" Mean?**  
     To "exec" into a container means to open a terminal (a command line interface) inside that virtual computer (the container). This way, you can directly interact with the app running inside.
   - **How to Do It:**  
     - **Option 1:** Use Docker Desktop to open a terminal in the container.  
     - **Option 2:** Open your computer’s terminal and type `docker exec -it <container_id> bash` to get a terminal inside the container. Replace `<container_id>` with the actual ID of your container. (You can find the container ID by running `docker ps`.)

2. **Navigate to the Application Folder in the Container**  
   - Type `pwd` (Print Working Directory) to see where you are.  
   - Use `ls` to list files in the current directory.  
   - Use `cd` to move into the `app` folder where the `page.js` file is located.

3. **Edit the `page.js` File Using Nano**  
   - **Install Nano if Needed:**  
     If you get an error saying `nano: command not found`, you need to install Nano, a text editor you can use in the terminal. Run these two commands:  
     - `apt-get update`  
     - `apt-get install -y nano`

   - **Open the File in Nano:**  
     Type `nano page.js` to open the file. Use the arrow keys to move around the file and make a small change, like modifying some text.

   - **Save Changes in Nano:**  
     To save, press `Ctrl + X`, then press `Y`, and finally press `Enter`.

4. **Check if the Changes Are Reflected in the Browser**  
   After saving the changes, look at your browser to see if the app rebuilds and shows your change. If it does, then the app inside the container can detect changes to its files.

## **Step 4: Understand Why the Changes Aren’t Working from VSCode**

1. **Why Doesn’t the App Rebuild When You Change Files in VSCode?**  
   The files inside the Docker container are "copied" during the build stage when you create the container. This means the files inside the container are a snapshot of what was on your computer at that moment. They are not "live" files, so any changes you make outside the container (in VSCode) won't be detected.

2. **What is the Solution? "Volumes"!**  
   Docker Volumes allow you to "link" files from your computer to the Docker container in real time. When you use volumes, the changes you make to your files (in VSCode) will be immediately reflected inside the container.

## **Step 5: Stop and Remove the Current Container**

1. **Stop and Remove the Container Using Docker Desktop or Terminal**  
   - **Using Docker Desktop:**  
     Open Docker Desktop, find your running container, and stop and remove it.
   - **Using Terminal Commands:**  
     Run the following commands in your terminal:  
     - `docker ps` (Lists running containers)  
     - `docker stop <container_id>` (Stops the container; replace `<container_id>` with the actual ID)  
     - `docker rm <container_id>` (Removes the container)

## **Step 6: Run a New Container with Volumes**

1. **Use the "Volume" Flag with `docker run` Command**  
   When you restart the container, use a special flag to create a volume. This makes the files "live" so the container will see any changes you make. The command looks like this:  
   ```bash
   docker run -p 3000:3000 -v /path/to/your/app:/app <image_name>
