# Operationalizing-an-AWS-ML-Project

## Step 1: Training and deployment on Sagemaker:

### Initial setup:

### Training and deployment:

### Multi-instance training:

## Step 2: EC2 Training:

### EC2 Setup:

### Preparing for EC2 model training:

### Training and saving on EC2:

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
