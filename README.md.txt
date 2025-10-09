# SWE645 - Homework Assignment 1
# Name: Dhanush Neelakantan  
# Course: SWE645
# Homework 1 – Static Website Hosting on S3 and EC2  

---

##  Project Overview
This assignment demonstrates creating a static website with:
- A homepage (index.html)  
- A survey form (survey.html)  

The website has been deployed on:
1. Amazon S3 static website hosting  
2. Amazon EC2 instance running Nginx  

---

##  Deployed URLs
- S3 Website Endpoint:  
  http://swe645-dhanushn-bucket.s3-website-us-east-1.amazonaws.com
    

- EC2 Public URL:  
  http://54.147.149.228/  

---

##  Steps to Configure Amazon S3
1. Logged into AWS Management Console → S3 → Create bucket.  
2. Gave the bucket a unique name: swe645-dhanush-bucket.  
3. Disabled Block all public access and acknowledged the warning.  
4. Uploaded the files: index.html, survey.html.  
5. Enabled Static website hosting under bucket Properties.  
   - Index document: index.html  
   - Error document: left blank.  
6. Added a Bucket policy to allow public read access:  
   {
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Principal": "*",
            "Action": "s3:GetObject",
            "Resource": "arn:aws:s3:::swe645-dhanushn-bucket/*"
        }
    ]
}
