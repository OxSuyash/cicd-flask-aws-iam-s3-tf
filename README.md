provision ec2 instance  ->  https://github.com/OxSuyash/create-ec2-iam-s3-tf/tree/main
clone project repo   ->  https://github.com/OxSuyash/cicd-flask-aws-iam-s3-tf

A. if you are running app in local env then create .env file
  ```
    AWS_ACCESS_KEY_ID=
    AWS_SECRET_ACCESS_KEY=
    AWS_REGION=
    S3_BUCKET_NAME=
    FLASK_HOST=127.0.0.1
    FLASK_PORT=5000
  ```

  Install dependecies from requirements.txt

  ```python run.py```
  -> App is runing on localhost, connected to s3 bucket using aws credentials.


B. Run app in local env as docker container  (prereq: wsl2 must be running)
  Install docker desktop if you're using windows.   ->   https://github.com/OxSuyash/docker-notes/blob/main/docker-desktop.md

  Open DOcker desktop   ->   Docker engine -> running   

  If your dockerfile installs the dependecies (requirements.txt), no need to install separately in local env. Only create .env file

  ```
  git clone repo_url
  cd repo
  ```
  .env file
  ```
    AWS_ACCESS_KEY_ID=
    AWS_SECRET_ACCESS_KEY=
    AWS_REGION=
    S3_BUCKET_NAME=
    FLASK_HOST=0.0.0.0
    FLASK_PORT=5000
  ```
  run docker docker container form project root 
  ```
  docker build -f docker-file/Dockerfile -t flask-app:latest .
  ```

  Check in container  ```docker images```

   Run container using above image: 
  ```
  docker run -p 5000:5000 -e FLASK_HOST=0.0.0.0 -e FLASK_PORT=5000 -e S3_BUCKET_NAME=s3-bucket-25-3def5b7f -e AWS_REGION=us-east-1 --name flask-app-container flask-app:v1
  docker ps
  ```
   For running docker container -> FLASK_HOST=0.0.0.0  

   Request flows : browser -> host port 5000 -> container port 5000 -> flask app port 5000

   Now you can access your app on 127.0.0.1:5000

   Also check in terminal  ```docker ps```

C. Run app as container on instance with iam attached as docker with cicd 
   create instance with vpc, subnets, security group, iam role, igw, etc -> Ref  https://github.com/OxSuyash/create-ec2-iam-s3-tf/tree/main


  1. Push app to github
  2. install Jenkins on instance   ->   https://github.com/OxSuyash/jenkins-notes/blob/main/install-jenkins.md
  3. install docker on instance   ->    https://github.com/OxSuyash/docker-notes/blob/main/install-docker.md
  4. Make sure users have access to docker  ubuntu and jenkins  and don't forget to re-ssh after modifying user
  5. configure jenkins job      ->      https://github.com/OxSuyash/docker-notes/blob/main/complete-jenkins-docker-app.md
  6. configure webhook          ->     https://github.com/OxSuyash/jenkins-notes/blob/main/webhook-github-jenkins.md
  7. provide env vars for running docker containers : manage jenkins->system->global properties->env vars 
      ```
      AWS_REGION=
      FLASK_HOST=
      FLASK_PORT=
      HOST_PORT=
      S3_BUCKET_NAME=
      ```
  
  8. manual build -> from jenkins ui
  9. add inbound rule for specified ports on instance (HOST_PORT)
  10. Access app at ip:HOST_PORT   ->  access diff routes
  11. Test upload and download routes
  12. after that for evey push a new build is generated.
   











   


