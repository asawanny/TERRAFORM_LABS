## In today's Lab work, We will configure Amazon S3 to host our Serveless static website using EC2 Instance and S3 console ##

- [ ] Login to AWS Management Console, with your credentials. *URL* https://amazon-bootcamp.signin.aws.amazon.com/console
- [ ] Search for *EC2* in search bar *New*.
- [ ] Click *Instances* -> *Launch Instance* on the left side menu.
- [ ] Give your instance a name such as *yourname_surname_webserver* 
        -> Use *Amazon Linux* as OS  
        -> *t2.micro* as Instance type  
        -> Use *Amazon Linux*  
        -> Proceed without a key pair

    ## This role will give access permissions to access S3 service from EC2 instance ##

        -> Under Advanced details, IAM Instance profile-> choose EC2Instance-Role
        -> Click *Launch Instance* button.
- [ ] Click *View all instances* and find your *instance ID*

## Create S3 Bucket using AWS CLI from the EC2 Instance ##

- [ ] aws s3 mb --region eu-central-1 "s3://your-bucket-name" 
 
Replace S3 URI "s3://your-bucket-name" to name of your bucket such as "s3://amazon-bootcamp-YOUR_NAME" 

If you get a access denied error, Click on the EC2 instance-> Actions ->Security -> Modify IAM Role -> Add EC2Instance-Role and save. Now, try again to create a bucket
 
## Enable Public access to S3 Bucket from EC2 Instance ##

- [ ] aws s3api put-public-access-block \
    --bucket your-bucket-name \
    --public-access-block-configuration "BlockPublicAcls=false,IgnorePublicAcls=false,BlockPublicPolicy=false,RestrictPublicBuckets=false"

    Replace your-bucket-name to name of your bucket. Use Notepad to modify Bucket name 

## Create index.html file inside the EC2 instance using vim editor ##

- [ ] touch index.html

- [ ] vi index.html -> Enter i to go to INSERT mode and copy paste below HTML content then press ESC, type :wq and press ENTER. It will quit and the contents will be saved in the file

- [ ] cat index.html ## To verify ##

## content of index.html ##

```
<body>
<h1>Hello World,  Welcome to my first Serverless Static Website using Amazon S3.</h1>
<h2>YOUR_NAME</h2>
</body>
```


## Create error.html file inside the EC2 instance using vim editor ##

- [ ] touch error.html 

- [ ] vi error.html-> Enter i to go to INSERT mode and copy paste below HTML content then press ESC, type :wq and press ENTER. It will quit and the contents will be saved in the file

- [ ] cat error.html ## To verify ##

## content of error.html ##

```
<body>
<h1>There is something wrong, please try again</h1>
</body>
```


## Upload both index.html and error.html to Amazon S3 from EC2 Instance ##

```
- [ ] aws s3 cp /home/ssm-user/ s3://amazon-bootcamp-siva/ --recursive --exclude "*" --include "*.html"
```

Use Notepad and replace /home/ssm-user/ directory to the directory where .html files are located and run this command. This command will copy both index.html and error.html to desired S3 bucket

## Edit Static Web Hosting settings using AWS Console ##

- [ ] Click on the Bucket, Navigate to Properties and scroll down to the Static Web Hosting - Click Edit
- [ ] Enable Static Web hosting 
- [ ] Hosting Type -> Host a static website
- [ ] enter "index.html" in Index document and enter name "error.html" in Error document
- [ ] save changes

## Configure Bucket Policy using AWS Console

- [ ] Search for S3 in top search bar, Click on S3 - redirects to S3 Console

- [ ] Click on the Bucket created during the previous step, Navigate to Permissions tab, scroll down to Bucket policy - click Edit, paste the below json policy and click save changes

## JSON Bucket Policy ##

Under Resource section of Bucket policy, replace "arn:aws:s3:::Bucket-Name/*" with ARN of your Bucket. Include trailing slash and * at the end 

ARN of the bucket can be found under properties section in S3 Console 

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "PublicReadGetObject",
            "Effect": "Allow",
            "Principal": "*",
            "Action": [
                "s3:GetObject"
            ],
            "Resource": [
                "arn:aws:s3:::Bucket-Name/*"
            ]
        }
    ]
}
```


## S3 static website is ready ##

- [ ] The S3 Serveless Static website should be available.

- [ ] Click on the Bucket, Navigate to Properties and scroll down to the Static Web Hosting - Copy the Bucket website endpoint and paste it in the Browser.

- 🎉 Awesome, You have successfully created your first Serverless Static Website using Amazon S3.
