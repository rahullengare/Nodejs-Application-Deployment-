# Nodejs Application Deployment
## **Node.js Introduction:**

Node.js is a free and open-source runtime that lets you run JavaScript code outside the browser. It's built on Google’s V8 engine, making it fast and efficient for building web servers, APIs, and real-time apps.

It’s popular because it handles many requests at once without slowing down, supports a huge library of packages through NPM, and works on all major systems. Plus, using JavaScript for both frontend and backend keeps development simple.

## About Project:

This project demonstrates how to deploy a basic **Node.js application** on an **AWS EC2 instance**. The main goal is to take a simple Node.js app and host it on a cloud server so it can be accessed through a public IP.

The process includes setting up an EC2 instance, installing Node.js, running the app, managing it in the background using **PM2**, and using **Nginx** as a reverse proxy to expose the app on the default HTTP port (80). This is a great hands-on way to learn the basics of deploying backend applications to the cloud.

## Technologies Used:

- **Node.js** – Backend runtime
- **NPM Packages** – For adding helpful tools and libraries
- **PM2**: Process manager that keeps the Node.js app running in the background
- **Nginx**: Web server used to forward HTTP requests to the Node.js app.
- **AWS EC2 (Amazon Elastic Compute Cloud)**: Virtual server where the app is hosted.
- **Amazon Linux**: The operating system used on the EC2 instance.
## Step 1: Launch an EC2 Instance

1. Go to AWS Console → EC2 
2. Launch Instance.
    - Name → nodejs
    - Choose AMI → Amazon Linux.
    - Instance type → t2.micro
    - Key pair → pem_server_key
    - security group → launch-wizard-1

## Step 2: SSH to connect the EC2 Instance

```bash
ssh -i "pem-server-key.pem" ec2-user@ec2-54-144-113-139.compute-1.amazonaws.com
```

## Step 3: Installing node app

1. update your system

```bash
sudo yum update
```

2. install nodejs in server 

```bash
sudo yum install nodejs -y
```

3. checking node app & npm is installed vesion

```bash
node --version
npm --version
```

4. make a **directory** nodeapp and go inside

```bash
mkdir nodeapp
cd nodeapp/
```

5. create package.json with code 


```bash
sudo vim package.json
```

```bash
{
    "name": "node-app",
    "version": "1.0.0",
    "description": "Simple Node.js app",
    "main": "app.js",
    "scripts": {
      "start": "node app.js"
    },
    "dependencies": {
      "express": "^4.18.2"
    }
  }
```

6. create app.js with code 

```bash
sudo vim app.js
```

```bash
{
    "name": "node-app",
    "version": "1.0.0",
    "description": "Simple Node.js app",
    "main": "app.js",
    "scripts": {
      "start": "node app.js"
    },
    "dependencies": {
      "express": "^4.18.2"
    }
  }
```

7. install packages in nodeapp **directory**

```bash
sudo npm install package.json
```

8. start node app

```bash
sudo npm start app.js
```

9. Now node app is Runing in Port 3000

```bash
<enter-your-public-ip>:3000
```

10. Terminal not present that why not any changes is possible → background nodeapp run

```bash
sudo npm install -g pm2
```

11. Now start again using pm2 service 

```bash
sudo pm2 start app.js
```

12. Now node app is Runing in Port 3000 & also possible to background changes

```bash
<enter-your-public-ip>:3000
```

13. Now port 3000 is not secure to always run using 3000 any time now create proxy_pass that to install nginx 

```bash
sudo yum install nginx
```

14. start the nginx & enable 

```bash
sudo systemctl start nginx
sudo systemctl enable nginx
```

15. now edit configuration file that to go 

```bash
cd /etc/nginx/
sudo chmod +x nginx.conf
sudo vim nginx.conf
```

16. add location block 

```bash
location /{
proxy_pass http://localhost:3000;
}
```

17. restart nginx server 

```bash
sudo systemctl restart nginx
```

18. now checking your node app 

```bash
<enter-your-public-ip-only-not-port>
```

1. Now Your **Node.js app** has successfully deployed on your EC2 instance
2. Terminate Your instance 
- go to AWS console → click on instance
- select your instance
- click on Instance state
- click on terminate (delete) instance
