

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
