# Operationalizing-an-AWS-ML-Project

## Step 1: Training and deployment on Sagemaker:

### Initial setup:

![s3](https://user-images.githubusercontent.com/30090343/234039014-63e8eea9-02e5-453f-a576-6bef6648b289.PNG)

![Notebook instances](https://user-images.githubusercontent.com/30090343/234039027-7a3e736b-d5e9-420b-b7ab-ce51f50d44a3.PNG)


### Training:

![training_jops](https://user-images.githubusercontent.com/30090343/234039200-10e72f8e-273f-465a-978f-2829474a19e9.PNG)

![training](https://user-images.githubusercontent.com/30090343/234039306-cdce7836-e141-445b-99b9-07a1a4f37a03.PNG)

![training03](https://user-images.githubusercontent.com/30090343/234039320-9774c31f-84b4-44ba-a687-9c9585807fae.PNG)

![training04](https://user-images.githubusercontent.com/30090343/234039323-d56b9243-2e99-4ffb-8e34-3066d1dec7b8.PNG)

![training05](https://user-images.githubusercontent.com/30090343/234039330-058da136-b24d-4f1d-adf6-b3cf9212bbb2.PNG)

![training06](https://user-images.githubusercontent.com/30090343/234039337-29b8aae8-c17e-4695-ba09-0a9e6e127786.PNG)



### Multi-instance training:

### Deployment:

![endpoint01-single-instance](https://user-images.githubusercontent.com/30090343/234039743-9c31ef4b-aaee-4871-958c-c18608b988d5.PNG)

![endpoint01-multi-instance](https://user-images.githubusercontent.com/30090343/234039751-2c46716f-4f1b-455d-8368-203be744e9f6.PNG)

## Step 2: EC2 Training:

### EC2 Setup:

### Preparing for EC2 model training:

![ec2-01](https://user-images.githubusercontent.com/30090343/234039996-1474488d-626a-4e81-adb8-d56599710807.PNG)

![ec2-02](https://user-images.githubusercontent.com/30090343/234040082-e552b812-8a21-42c4-832f-722bc19a21fe.PNG)

### Training and saving on EC2:

![ec2-04](https://user-images.githubusercontent.com/30090343/234040247-066f275e-0990-448d-a06a-7c59522354f0.PNG)

![ec2-05](https://user-images.githubusercontent.com/30090343/234040251-d3cd6c61-d76d-43a2-95aa-6571b1b01143.PNG)

![ec2-03](https://user-images.githubusercontent.com/30090343/234040243-85fbec6f-f56e-4de8-ba0e-529df923586c.PNG)


In EC2 training, we need to set up our own EC2 instance and then install the necessary software packages, libraries, and frameworks to train the machine learning model. We then need to upload the training data to the EC2 instance, and then run the training script on the EC2 instance. The training script should be written in a way that it can read the training data from the instance, perform the necessary computations, and then write the trained model to the instance.

On the other hand, in SageMaker, we don't need to set up our own instance, and we don't need to worry about installing any software or libraries. SageMaker provides us with pre-built Docker images with pre-installed libraries and frameworks. We can directly upload our training data to SageMaker, and then use the pre-built Docker image to train our model. We can also use SageMaker's built-in algorithms to train our model.

Overall, SageMaker provides a more convenient and streamlined process for training machine learning models, while EC2 provides more flexibility and control over the training process. The choice of which service to use depends on the specific needs of the project.

## Step 3: Lambda function setup:

### Setting up a Lambda function:

![lambda-invoke-02](https://user-images.githubusercontent.com/30090343/233828832-b9ecb058-edb2-4218-b2e5-f26a74822572.PNG)

This Lambda function is designed to handle requests and responses for a SageMaker endpoint. It uses the boto3 library to create a runtime object that can invoke the endpoint. The endpoint name is passed as an argument to the function, and the function then sets up a JSON payload with the input data, which is passed to the endpoint using the invoke_endpoint() method. The response from the endpoint is read and decoded, and then the function sets up a JSON response with the result.

![lambda-invoke-01](https://user-images.githubusercontent.com/30090343/233828850-ad51de64-943e-49f1-a864-e7a734c59a71.PNG)


The function is triggered when an event is received, which could be a request from a client application or another Lambda function. The input payload is passed as an argument to the lambda_handler() function, and the context object is also passed for additional information about the execution environment.

The function sets up the response headers and body using the statusCode, headers, and body keys in the returned dictionary. The body key is set up as a JSON string containing the output from the endpoint, which is decoded from the response body using the json library. Finally, the function returns the response dictionary to the client application or the calling Lambda function.

## Step 4: Security and testing:

![security-01](https://user-images.githubusercontent.com/30090343/233828892-e8000b83-db55-475b-becf-b7376b99cd8b.PNG)

AWS provides various security measures to ensure the security and privacy of data in AWS workspaces. These include data encryption at rest and in transit, network isolation, and access control. AWS also offers Identity and Access Management (IAM) to manage access to AWS services and resources. IAM allows administrators to create and manage users, groups, and roles to control access to AWS resources.

However, despite these measures, AWS workspaces can still be vulnerable to security breaches if not appropriately managed. One of the most common security vulnerabilities is overly permissive roles or policies. When roles have too many permissions, they can provide attackers with access to sensitive data and resources. It is essential to ensure that each role has the minimum set of permissions required for a user to perform their tasks. Inactive or old roles that are not necessary anymore should be deleted or deactivated to prevent unauthorized access.

Another common vulnerability is weak passwords or authentication mechanisms. AWS provides several authentication mechanisms, including username and password, Multi-Factor Authentication (MFA), and Single Sign-On (SSO). To secure AWS workspaces, it is essential to enforce strong password policies, enable MFA or SSO, and use secure protocols such as HTTPS and SSL/TLS to secure communication.

In conclusion, while AWS workspaces provide robust security measures, they can still be vulnerable to security breaches. Administrators must ensure that they follow best practices for access control, password policies, and authentication mechanisms to secure AWS workspaces. Regular security audits and assessments can also help identify and mitigate security vulnerabilities before they are exploited by attackers.

## Step 5: Concurrency and auto-scaling:

In configuring the concurrency and auto-scaling for the deployed endpoint, I made some choices based on the requirements of the application and the available resources. For concurrency, I chose to use On-demand concurrency. This choice was made because the On-demand concurrency model provides a flexible and scalable option for managing function invocations in response to varying levels of traffic. With On-demand concurrency, AWS automatically provisions the necessary compute capacity to handle the incoming requests and manages the scaling of the function to meet the demand.

For auto-scaling, I chose to use the Step scaling policy. The Step scaling policy works by setting up thresholds for CPU utilization, which triggers a scaling event to add or remove instances based on the predefined steps. I chose this policy because it provides the flexibility to define custom scaling behavior and allows for more precise control over scaling based on changes in CPU utilization. This means that if the traffic increases suddenly, the auto-scaling will add more instances to the endpoint to handle the traffic, and if the traffic decreases, the scaling will remove the instances to save cost.

Another reason why I chose the Step scaling policy is that it provides an efficient way to manage the endpoint's compute resources. Since the Step scaling policy is based on CPU utilization, it provides a good measure of the endpoint's performance and ensures that the endpoint has sufficient compute resources to handle the incoming requests without being over-provisioned.

In summary, my choices in setting up On-demand concurrency and Step scaling for auto-scaling were based on the need for a flexible and scalable option for managing function invocations in response to varying levels of traffic, and the need for an efficient way to manage the endpoint's compute resources. These choices provide a good balance between performance and cost-effectiveness, ensuring that the endpoint can handle the incoming requests efficiently while minimizing the cost of computing resources.
