# cloudfront-s3-route53-website-automation-pipeline
Hosting static website on AWS S3 with CloudFront, creating CI/CD pipeline and SSL with domain name


# Architecture Diagram:


![arch](https://github.com/user-attachments/assets/d31a41f3-8842-4d1a-b760-3190426a3a19)





# OverView:
Hosting static websites on cloud platforms like Amazon Web Services (AWS) provides numerous benefits such as scalability, high availability, and ease of maintenance. In this article, we will explore how to host a static website on AWS S3 (Simple Storage Service), implement Continuous Integration and Continuous Deployment (CI/CD) using AWS services, set up a Content Delivery Network (CDN) for improved performance, and configure a custom domain.


# Prerequisites
-  Before diving into the main content, ensure you have the following prerequisites in place:

- AWS Account: You'll need an AWS account to access the services required for this setup. Sign up for an AWS account if you don't have one already.

- GitHub Account: Set up a GitHub account for version control and CI/CD integration.

- Static Website: Have a static website ready, consisting of HTML, CSS, JavaScript, and other assets.

- A registered domain: You should have a registered domain name from a domain registrar like Route 53, GoDaddy, or Namecheap.

- Basic Knowledge: Familiarity with AWS concepts, static website structure, and CI/CD principles will be beneficial.



# Implementation Workflow:
- The architecture diagram presented above showcases the system we will be discussing in this repo. It outlines the flow of a user's request when they enter the website URL.

- The process begins with the user's request being checked by the AWS Web Application Firewall (WAF). If the check is successful, the request is then forwarded to Amazon Route53, which translates the website address into the corresponding CloudFront distribution name.

- To ensure a secure connection, the AWS Certificate Manager is employed to handle SSL encryption. This ensures that the user's data remains protected during transmission.

- The CloudFront distribution, in turn, retrieves its contents from an S3 bucket. This bucket serves as a storage location for the website's files and resources.

- To facilitate continuous integration and deployment, the S3 bucket is connected to a Git repository through the use of AWS CodePipeline. The pipeline automates the process of fetching the latest code changes from the Git repository and deploying them to the S3 bucket. This allows for seamless updates and changes to the website's content, ensuring that the latest version is always available to users.



# Steps:

# Create an S3 Bucket for Website Hosting:
- The first step is to create an S3 bucket and enable it for static web hosting. To do that, follow the steps below:

- Log in to your AWS Management Console.

- Search for and navigate to the S3 service.

- Click on "Create bucket", then provide a globally unique name (With your domain) for your bucket.





# Make sure all public access to the bucket is blocked.

- Leave the remaining settings as default and create the bucket.

- To ensure the bucket can host our static website, we need to enable static hosting.

- Navigate to the "Properties" tab of the newly created bucket




![image](https://github.com/user-attachments/assets/e8306596-b7ab-4e44-b81b-84c8b8304b06)









- Enable static website hosting and select the hosting type as "Host a static website".

- Enter the default index document and error document (if needed), then save the changes.





![image](https://github.com/user-attachments/assets/c7bf1f9e-224a-495e-867a-ee9ec769c1e8)








- After making these changes, a bucket URL (as depicted in the second diagram) will be generated. This URL links to the website. However, clicking on it won't give any positive result because of two reasons: 1) Public access to the bucket is blocked, and 2) The bucket is empty.

In the following section, we will proceed to populate the S3 bucket with content from our GitHub repository.




# Configure CI/CD Pipeline:

- Choose a CI/CD tool. In this article, we will be making use of AWS CodepipelinSearch for and navigate to the AWS Codepipeline service.

- Create a new pipeline

- Give the pipleine a name, and create a new service role or use an existing one.

- In the source stage, select GitHub as the source provider and authorize AWS to access your GitHub account.


- Select the repository containing the application code and the branch
  
- Then click on Next, skip the build stage, and proceed to the deploy stage.

- At the deploy stage, select Amazon S3 as the deploy provider, choose a region, and select the S3 bucket with the website content. Then click Next.

- Once the pipeline has been successfully created, as shown in the image below, the content of our GitHub repository will be in the S3 bucket, and any changes made to the code will be updated in the S3 bucket.





![aws-cicd-pipeline](https://github.com/user-attachments/assets/f0be3597-a35a-4e93-8e39-bc76274edfe5)







############################################################################################################


- Now that our bucket contains the website code, we still can't access the website since all public access has been blocked.


- To allow this, let's configure our CloudFront distribution to access the contents of the S3 bucket.










# Configure CDN (Amazon CloudFront):

- Navigate to the CloudFront service in the AWS Management Console.

- Click "Create Distribution" and select the S3 bucket as your origin domain. Also, give a name for the origin domain.



















