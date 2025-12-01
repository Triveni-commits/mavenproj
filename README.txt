DOCKER-IMAGE CREATION
---------------------------------------------------------------------------------------
Method 1: Image Creation Using docker commit

Step 1: Start a new container
docker run -it --name ubuntu-con-1 ubuntu:latest

Step 2: Make changes inside the container
apt update
apt install -y git

Step 3: Exit the container
exit

Step 4: Create image from container
docker commit ubuntu-con-1 img-commit-1

Step 5: Check images
docker images

Step 6 (optional): Tag and push
docker tag img-commit-1 triveni27/img-commit-1
docker push triveni27/img-commit-1

✅ Method 2: Image Creation Using Dockerfile

Step 1: Create folder
mkdir image-creation
cd image-creation

Step 2: Create Dockerfile
notepad Dockerfile

Step 3: Add instructions in Dockerfile

FROM ubuntu:latest
RUN apt-get update && apt-get install -y git
CMD ["bash"]


Step 4: Build image
docker build -t img-dockerfile-1 .

Step 5: Test the image
docker run -it img-dockerfile-1

Step 6: Check images
docker images

Step 7 (optional): Tag and push
docker tag img-dockerfile-1 triveni27/img-dockerfile-1
docker push triveni27/img-dockerfile-1

---------------------------------------------------------------------------------------------------------------
-----------------------------------------------------------------------------------------------------------
DOCKER-compose
----------------------------------------------------------------------------------
PART 1 – Docker-compose with two servers (nginx + tomee)
Step 1 – Run two containers using docker run (simple way)

In PowerShell, run:

docker run -d -p 8010:80 nginx
docker run -d -p 8020:8080 tomee


Step 2 – Check in browser
Open:
http://localhost:8010 → Nginx default page
http://localhost:8020 → Tomee page (if app is there)
This proves two containers can run simultaneously on different ports.
Step 3 – Put same setup into a docker-compose file
Make a folder:
mkdir comp-1-server
cd comp-1-server
Create docker-compose.yml and put:
services:
  web:
    image: nginx
    ports:
      - "8060:80"

  db:
    image: tomee
    ports:
      - "8050:8080"

Step 4 – Start using docker-compose
From inside comp-1-server folder:
docker-compose up -d

(or docker compose up -d)
Docker will create a network (comp-1-server_default) and start both containers:
Step 5 – Open in browser

http://localhost:8060 → nginx page
http://localhost:8050 → tomee page
-------------------------------------------------------------------------------------------------------
PART 2 – WordPress + MySQL using docker-compose
Step 1 – Create folder and file
In PowerShell:
cd C:\Users\NekshaSrinivas\SE-1
mkdir mysql
cd mysql
Create docker-compose.yml with:
notepad docker-compose.yml

services:
  wordpress:        # WordPress service
    image: wordpress:latest
    ports:
      - "9080:80"   # host 9080 -> container 80
    environment:
      WORDPRESS_DB_HOST: db:3306
      WORDPRESS_DB_USER: wordpress
      WORDPRESS_DB_PASSWORD: wordpress
      WORDPRESS_DB_NAME: wordpress
    depends_on:
      - db          # start DB first

  db:               # MySQL service
    image: mysql:5.7
    environment:
      MYSQL_ROOT_PASSWORD: rootpassword
      MYSQL_DATABASE: wordpress
      MYSQL_USER: wordpress
      MYSQL_PASSWORD: wordpress


Step 2 – Start the stack

Inside C:\Users\NekshaSrinivas\SE-1\mysql:

docker-compose up -d
You will see:

network mysql_default created

containers mysql-db-1 and mysql-wordpress-1 started

Step 3 – Open WordPress site
In browser, open: http://localhost:9080
Steps on screen:

Select language.

Fill details (site title, admin username, password, email).

Click Install WordPress → you get success message.

Log in with the credentials you just created → dashboard page appears.
---------------------------------------------------------------------------------------------------------
PART 3 – Custom Flask app + MySQL using docker-compose
Step 1 – Create new project folder

In PowerShell:

cd C:\Users\NekshaSrinivas\SE-1
mkdir custom_flask
cd custom_flask
You will create three files here:

app.py – Flask application code

Dockerfile – how to build the Flask image
docker-compose.yml – runs Flask + MySQL together

Step 2 – Write app.py

Open/ create app.py and add:

from flask import Flask

app = Flask(__name__)

@app.route("/")
def home():
    return "Hello from 24BD5A0503 - NEKSHASRINIVAS"

if __name__ == "__main__":
    app.run(host="0.0.0.0", port=5000)


Step 3 – Write Dockerfile(notepad Dockerfile and ren Dockerfile.txt Dockerfile)


Create Dockerfile in same folder:

# Base image with Python
FROM python:3.10-slim

# Set working directory inside container
WORKDIR /app

# Copy app code into container
COPY app.py /app

# Install Flask
RUN pip install flask

# Run the Flask app
CMD ["python", "app.py"]


(We are not yet using MySQL from code, DB is just running alongside.)

Step 4 – Write docker-compose.yml

Create docker-compose.yml:

version: "3.9"

services:
  web:
    build: .
    ports:
      - "5000:5000"
    depends_on:
      - db

  db:
    image: mysql:8.0
    restart: always
    environment:
      MYSQL_ROOT_PASSWORD: root
      MYSQL_DATABASE: mydb
    ports:
      - "3306:3306"


Step 5 – Build and run with docker compose

From inside C:\Users\NekshaSrinivas\SE-1\custom_flask:

docker compose up --build
--build forces Docker to build the web image using the Dockerfile.
change port 3306 to 3307 if there is used already

If you want them in background, use:
docker compose up --build -d
Step 6 – View your custom Flask page
Open your browser and go to:
http://localhost:5000
You should see:
Hello from 24BD5A0503 - NEKSHASRINIVAS

---------------------------------------------------------------------------------------------
-------------------------------------------------------------------------------------------
-------------------------------------------------------------------------------------------------

5. Docker CLI Commands
Installing Docker and Setting up Nginx inside Ubuntu Container

Step 1: Check Docker installation & Pull Ubuntu image
Check that Docker is installed and running:
docker --version

Pull the latest Ubuntu image from Docker Hub:
docker pull ubuntu:latest

Step 2: Run an Ubuntu container
Create and start a new container named myubuntu, mapping host port 9090 to container port 80:
docker run -it -p 9090:80 --name myubuntu ubuntu:latest


Step 3: Update packages and install Nginx (and nano)
Inside the container, run:

apt update
apt install nginx nano -y

Step 4: Start Nginx and go to index.html
Start the Nginx service:

service nginx start

Go to the folder where the default Nginx page is stored:
cd /usr/share/nginx/html
ls

You will see index.html here.
Open index.html in nano editor:

nano index.html

Edit the file – for example, change the <h1> line to:
<h1>Welcome user, I'm Neksha Srinivas!</h1>

Save and exit nano:
Press Ctrl + O → Enter (to save)
Press Ctrl + X (to exit)

Step 5: View the page from browser (localhost)

On your Windows host, open a browser (Chrome/Edge) and go to:
http://localhost:9090

You should see your customized Nginx page:
Welcome user, I'm Neksha Srinivas!
--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
🌟 FULL GIT COMMAND LIST WITH CLEAR PURPOSE

I will group them logically so your brain remembers everything easily.

1️⃣ Git Configuration — (Only once per laptop)
git config --global user.name "Abhinav"
git config --global user.email "abhinav@example.com"


Purpose → Tells Git who you are.

2️⃣ Creating a New Project Locally
git init


Purpose → Start Git tracking in an empty folder.

3️⃣ Cloning Existing GitHub Repository
git clone https://github.com/abhinav/reelongo-project.git


Purpose → Download project from GitHub.

4️⃣ Checking Current Status
git status


Purpose → See changed files, staged files, branch name.

5️⃣ Staging Files
git add .
# or
git add <filename>


Purpose → Add changes to staging area.

6️⃣ Commit Your Work
git commit -m "Initial project setup"


Purpose → Save a snapshot with a message.

7️⃣ Push Code to GitHub
git push origin main


Purpose → Upload commits to GitHub on branch main.

8️⃣ Pull Latest Code
git pull origin main


Purpose → Download + merge latest remote changes.

9️⃣ Create & Switch to New Branch
git checkout -b feature/login-page


Purpose → Create & move to your feature branch.

🔟 Switch Back to Main
git checkout main

1️⃣1️⃣ Merge Feature Branch into Main
git merge feature/login-page


Purpose → Bring feature work into main.

1️⃣2️⃣ Fix “rejected – non-fast-forward” Error
git pull --rebase origin main


Purpose → Rebase your work on latest main.

1️⃣3️⃣ Push Feature Branch Without Affecting Main
git push origin feature/feat-1

1️⃣4️⃣ Fetch All New Remote Branches
git fetch origin


Purpose → Update local repo with remote branches (without merging).

1️⃣5️⃣ Avoid Merge Conflicts While Pulling
git stash
git pull --rebase origin main
git stash pop


Purpose → Safely update your branch with new changes.

1️⃣6️⃣ Remove Sensitive File (Stop Tracking)
git rm --cached api-keys.env
git commit -m "Remove sensitive file"
git push origin main

1️⃣7️⃣ Force Remove Sensitive File from Entire Git History
git filter-branch --force --index-filter \
"git rm --cached --ignore-unmatch api-keys.env" \
--prune-empty --tag-name-filter cat -- --all


⚠️ Advanced. Use only when necessary.

1️⃣8️⃣ Update Your Feature Branch with Latest Main
git checkout feature/feat-1
git fetch origin
git rebase origin/main

1️⃣9️⃣ Push Code to New Remote Repository
git push origin feature/feat-1

2️⃣0️⃣ Bring Your Branch Up to Date (with your changes safely stored)
git stash
git pull --rebase origin feature/feat-1
git stash pop

2️⃣1️⃣ Resolve Merge Conflicts After Editing Files
git add .
git commit -m "Resolve merge conflicts"

2️⃣2️⃣ Delete Remote Branch
git push origin --delete feature/old-feature

⚡️ NOW THE COLLABORATIVE CODING PART ⚡️

These are normally used during team work, labs, and organization repos.

2️⃣3️⃣ Create a GitHub Organization

(UI-based — no command)

Steps:

GitHub → Profile → "Your Organizations"

Create New Organization → Free plan

Invite members

(No terminal commands for this.)

2️⃣4️⃣ Add Remote for Shared Team Repository
git remote add origin https://github.com/org-name/repo-name.git


If origin already exists and you want to change it:

git remote set-url origin https://github.com/org-name/repo-name.git

2️⃣5️⃣ Working on Shared Repository (Team Flow)
Step 1: Clone shared repo
git clone https://github.com/org-name/repo-name.git
cd repo-name

Step 2: Create your own branch
git checkout -b feature/task1

Step 3: Push your work
git add .
git commit -m "Task 1 done"
git push origin feature/task1

Step 4: Create Pull Request (UI-based)
2️⃣6️⃣ Resolving Conflicts When Working on the Same File
git pull origin main
# fix conflict markers (<<<<<<< ======= >>>>>>>)
git add <file>
git commit -m "Resolved conflict"
git push origin main

2️⃣7️⃣ Create Patch (for sending code changes)
Create a patch from last commit:
git format-patch -1

Create patch for multiple commits:
git format-patch <commit1>..<commit2>

Apply patch received from someone:
git apply file.patch
