# 2048-Devops-Project
This is a Mini Devops Project 

This repository provides a Dockerfile for setting up an Ubuntu 23.04-based container with Nginx and the popular game 2048.

## Instructions

To build the Docker image and run the container, follow these steps:

1. Install Docker on your machine if you haven't already.
2. Clone this repository or create a new directory for your project and create a new file called `Dockerfile`.
3. Open the `Dockerfile` in a text editor and copy the following content into it:

   ```dockerfile
   FROM ubuntu:23.04
   
   RUN apt-get update
   
   RUN apt-get install -y nginx zip curl
   
   RUN echo 'daemon off;' >>/etc/nginx/nginx.conf
   RUN curl -o /var/www/html/master.zip -L https://codeload.github.com/gabrielecirulli/2048/zip/master
   RUN cd /var/www/html && unzip master.zip && mv 2048-master/* . && rm -rf 2048-master master.zip
   
   EXPOSE 80
   
   CMD ["/usr/sbin/nginx", "-c" , "/etc/nginx/nginx.conf"]
   ```

4. Save the `Dockerfile`.
5. Open a terminal or command prompt and navigate to the directory containing the `Dockerfile`.
6. Build the Docker image by running the following command:

   ```bash
   docker build -t 2048-game .
   ```

   This command will create a Docker image with the name `2048-game` based on the `Dockerfile` in the current directory.

7. Once the image is built, you can run a container based on it using the following command:

   ```bash
   docker run -d -p 80:80 2048-game
   ```

   This command will start a container in the background (`-d` flag) and map port 80 of the container to port 80 of your local machine (`-p 80:80` flag).

8. Open a web browser and visit [http://localhost](http://localhost). You should see the 2048 game running.

It would look somewhat like this - 

![Localhost](image-1.png)


## Notes

- The Dockerfile starts with the `ubuntu:23.04` base image, updates the package lists, and installs Nginx, zip, and curl.
- It then modifies the Nginx configuration to run in the foreground.
- Next, it downloads the `master.zip` file from the GitHub repository of the 2048 game and extracts it into the `/var/www/html` directory.
- Finally, it exposes port 80 and starts Nginx using the modified configuration file.

Feel free to customize the Dockerfile or explore different versions of Ubuntu or Nginx based on your requirements.

A docker file is created which form the image.

---

When your docker file is successfully created and can be seen on your localhost. It is ready to be hosted.

I used AWS Elastic BeanStalk to host it.

It can be done in few easy steps - 

1. Sign in to the AWS Management Console and open the Elastic Beanstalk service.

2. Click on "Create a new environment" or "Create Environment" to start the environment creation process.

3. Select "Web server environment" as the environment tier.

![ConfigureEnviroment](image.png)

4. Fill out the Application name and Enviroment information  choose Docker as the platform.

![EnvInfo](image-2.png)

5. In Application Code Section add Version Lable (example - 2048-game-v1)

6. Select Upload code in Application Code Section. Click Local File and Click on Choose File then upload your Docker file.

7. Choose your desired Preset.

8. Create your Service Access.

9. For now just skip to review if you don't wnat to configure anything else.

10. Click "Submit" to start the environment creation process. Elastic Beanstalk will now create the necessary resources and deploy your Docker image.

11. Once the environment is successfully created and running, Elastic Beanstalk will provide you with a URL to access your application.

12. Open a web browser and visit the provided URL. You should be able to see the 2048 game running. 




*2048 game cloned from the repository - https://github.com/gabrielecirulli/2048*


