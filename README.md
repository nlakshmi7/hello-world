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

![image](https://github.com/user-attachments/assets/16888b83-6e55-42a9-9b8f-3f45c4bfe759)

![image](https://github.com/user-attachments/assets/4486dcc4-dc2e-455d-822d-e581b66c0f67)

![image](https://github.com/user-attachments/assets/30c1045b-fe55-498e-905b-fbb986acd53e)





S3_BUCKET_NAME with the name of your S3 bucket.



