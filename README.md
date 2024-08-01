1. Take a hello world program from google and store it in github.
2. Using gitlab ci/cd pipeline push the code to AWS s3.
3. Build using maven
4.after hosting in S3 bucket, check the url weather you are accessing the web page or not(hello world)


from git hub to push the code to the git lab

1.the github repo should be public repo
2.loging with the github for the git lab
3.and click on the create project use the import option and provide the repo url from github.
4.login to IAM and create the user with attached policy s3FullAcess.
5.under the created user has security as shown below i.e access key
![image](https://github.com/user-attachments/assets/4a23a86b-89ea-4a92-ac37-bce31618c92f)
![image](https://github.com/user-attachments/assets/d631084d-9f72-46a7-b19b-e89a70e4de3f)

6.Store AWS Credentials in GitLab
Navigate to Your GitLab Project:

Go to your project on GitLab.
Access CI/CD Settings:

Go to "Settings" > "CI/CD".
Add Variables:

Expand the "Variables" section and add the following variables:
AWS_ACCESS_KEY_ID with your AWS access key.
AWS_SECRET_ACCESS_KEY with your AWS secret key.
AWS_REGION with the region where your S3 bucket is located (e.g., us-west-2).

![image](https://github.com/user-attachments/assets/220d8f22-b387-4b9a-baff-72b6bc1ad6ae)
here CICD is the projectname

![image](https://github.com/user-attachments/assets/518f6f78-8f07-4ca6-a635-bd40b44e73e5)


#.gitlab-ci.yaml file
stages:
    - create-s3-bucket
    - build
    - deploy

image: registry.gitlab.com/gitlab-org/cloud-deploy/aws-base:latest

variables:
    S3_BUCKET_NAME: gitlab-test-lucky

create-s3-bucket-job:
  stage: create-s3-bucket
  script:
    - echo "Creating S3 bucket if it doesn't exist"
    - aws s3 mb "s3://$S3_BUCKET_NAME" || echo "Bucket already exists"
    - aws s3 ls

build_job:
    stage: build
    image: maven:3.8.5-openjdk-17
    script: 
        - mvn clean package
    artifacts:
        paths:
            - target/*.jar

deploy_to_s3:
  stage: deploy
  script:
    - echo "Listing contents of target directory:"
    - ls -l target/
    - echo "Deploying JAR to S3 bucket:$S3_BUCKET_NAME"
    # Find the JAR file in the target directory
    - JAR_FILE=$(find target -name '*.jar')
    - echo "Deploying file:$JAR_FILE"
    # Upload the JAR file to S3
    - aws s3 cp "$JAR_FILE" s3://$S3_BUCKET_NAME/
    - aws s3 sync ./src/main/java/com/example s3://$S3_BUCKET_NAME/

trigger the build
![image](https://github.com/user-attachments/assets/901d24a1-bd5a-4277-b95e-1f01181e2cbd)

![image](https://github.com/user-attachments/assets/dc9bfe37-21c8-4a26-bfde-459bd0144371)

![image](https://github.com/user-attachments/assets/fcc99b72-31f0-4432-b5bb-a6665f5159ed)

the block user should be disable

provide the bucket policy as below under the permission tab.
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Principal": "*",
            "Action": "s3:GetObject",
            "Resource": "arn:aws:s3:::gitlab-test-lucky/*"
        }
    ]
}

![image](https://github.com/user-attachments/assets/bd82d322-e499-45dd-8a01-1ed403374ce2)

under properties tab need to enablt the static website and upload the index.html in the bucket

![image](https://github.com/user-attachments/assets/4998a0c1-4077-4274-991b-965e1b813a8c)

#index.html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Hello World</title>
</head>
<body>
    <h1>Hello, World!</h1>
    <p>This is a simple static website hosted on Amazon S3.</p>
	<a href="https://your-bucket-name.s3.amazonaws.com/helloworld-1.0-SNAPSHOT.jar" download>Download HelloWorld JAR</a>
</body>
</html>


final output is
![image](https://github.com/user-attachments/assets/ae881464-0e6b-4c94-a84e-4843901c5f17)





