# Three-Tier-Architecture

Deployed an simple react-app 

## Three Tier Architecture Overview: 
Web Server: This is the front part of a web application that displays the website to users. It handles how the site looks and interacts with users, like showing product pages or allowing people to sign up.

Application Server: This middle part manages the business logic and processes user actions. For example, it checks the inventory to see if a product is in stock or updates a user's account details.

Database Server: This is the back-end part that stores all the data for the application. It uses databases like MySQL or PostgreSQL to keep track of information.

## Overview:
In this architecture, we have three main layers:

1. Web Tier: Handles client requests and serves the front-end website.
2. Application Tier: Processes API requests and handles the business logic.
3. Database Tier: Manages data storage and retrieval.

## Components:
#### 1. External Load Balancer (Public-Facing Application Load Balancer)
- **Role**: This acts as the entry point for all client traffic.
- **Functionality**:
  - Distributes incoming client requests to the web tier EC2 instances.
  - Ensures even distribution of traffic for better performance and reliability.
  - Performs health checks to ensure only healthy instances receive traffic.
#### 2. Web Tier
- **Role**: Serves the front-end of the application and redirects API calls.
- **Components**:
  - **Nginx Webservers**: Running on EC2 instances.
  - **React.js Website**: The front-end application served by Nginx.
- **Functionality**:
  - **Serving the Website**: Nginx serves the static files for the React.js application to the clients.
  - **Redirecting API Calls**: Nginx is configured to route API requests to the internal-facing load balancer of the application tier.

#### 3. Internal Load Balancer (Application Tier Load Balancer)
- **Role**: Manages traffic between the web tier and the application tier.
- **Functionality**:
  - Receives API requests from the web tier.
  - Distributes these requests to the appropriate EC2 instances in the application tier.
  - Ensures high availability and load balancing within the application tier.

#### 4. Application Tier
- **Role**: Handles the application logic and processes API requests.
- **Components**:
  - **Node.js Application**: Running on EC2 instances.
- **Functionality**:
  - **Processing Requests**: The Node.js application receives API requests, performs necessary computations or data manipulations.
  - **Database Interaction**: Interacts with the Aurora MySQL database to fetch or update data.
  - **Returning Responses**: Sends the processed data back to the web tier via the internal load balancer.

#### 5. Database Tier (Aurora MySQL Multi-AZ Database)
- **Role**: Provides reliable and scalable data storage.
- **Functionality**:
  - **Data Storage**: Stores all the application data in a structured format.
  - **Multi-AZ Setup**: Ensures high availability and fault tolerance by replicating data across multiple availability zones.
  - **Data Retrieval and Manipulation**: Handles queries and transactions from the application tier to manage the data.
    
### Additional Components

#### Load Balancing
- **Purpose**: Distributes incoming traffic evenly across multiple instances to prevent any single instance from becoming a bottleneck.
- **Implementation**:
  - **Web Tier**: The external load balancer distributes traffic to web servers.
  - **Application Tier**: The internal load balancer distributes API requests to application servers.

#### Health Checks
- **Purpose**: Continuously monitors the health of instances to ensure only healthy instances receive traffic.
- **Implementation**:
  - **Web Tier**: Health checks by the external load balancer to ensure web servers are responsive.
  - **Application Tier**: Health checks by the internal load balancer to ensure application servers are operational.

#### Auto Scaling Groups
- **Purpose**: Automatically adjusts the number of running instances based on traffic load to maintain performance and cost efficiency.
- **Implementation**:
  - **Web Tier**: Auto-scaling based on metrics like CPU usage or request count to add or remove web server instances.
  - **Application Tier**: Auto-scaling based on similar metrics to adjust the number of application server instances.

 ### Stages For Creating Resources
1. Create a vpc for project
2. create s3 bucket and upload code, creae IAM role to attach to ec2 instance
   
Note: Create Security Groups for resources (1 webserver, 1 web internet facing ALB, 1 Appserver, 1 app internal, 1DB)

4. create DB subnet groups and create rds Database. (mysql db) , optinally we can enable multiAZ's 
5. create application tier resources including internal load balancer
6. create web tier resources including external load balancer 
 
### Architecture: 

![Screenshot (131)](https://github.com/user-attachments/assets/4f986180-285e-4321-a1ac-591098ae41c1)



