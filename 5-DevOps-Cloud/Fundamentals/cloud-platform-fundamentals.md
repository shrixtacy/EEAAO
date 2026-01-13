# Cloud Platform Fundamentals

## Overview

Cloud platforms provide on-demand computing resources without managing physical hardware. Instead of buying servers, you rent computing power, storage, and services from providers like AWS, Azure, or Google Cloud.

**Why Cloud Matters:**
- Scale resources up or down based on demand
- Pay only for what you use
- Access to managed services (databases, monitoring, etc.)
- Global infrastructure without physical presence
- Disaster recovery and backup capabilities

**What Problems Cloud Solves:**
- High upfront infrastructure costs
- Server maintenance and hardware failures
- Scaling challenges during traffic spikes
- Geographic distribution of applications
- Disaster recovery complexity

## Choosing Your First Cloud Provider

### Start with ONE Provider

**The Mistake:** Trying to learn AWS, Azure, and Google Cloud simultaneously.

**The Reality:** Cloud providers are 80% similar in core concepts. Master one deeply, then apply knowledge to others.

**Recommendation Order:**
1. **AWS** - Largest market share, most learning resources, best for general web applications
2. **Azure** - Best if you're in Microsoft ecosystem (Windows, .NET, Office 365)
3. **Google Cloud** - Best for data/AI workloads, Kubernetes-native applications

### Decision Framework

**Choose AWS if:**
- You're building general web applications or APIs
- You want the most learning resources and community support
- You're working at a startup (most common choice)
- You need the widest variety of services

**Choose Azure if:**
- Your organization uses Microsoft technologies (.NET, Windows Server, Office 365)
- You need strong enterprise integration
- You're in a regulated industry (strong compliance offerings)

**Choose Google Cloud if:**
- You're building data-heavy or AI/ML applications
- You prefer Kubernetes-native container orchestration
- You need advanced networking capabilities
- You want simpler pricing models

## AWS Fundamentals

### Core AWS Services You Actually Need

**Don't Learn Everything:** AWS has 200+ services. Focus on the core 10-15 that solve 90% of problems.

### Essential Compute Services

**1. EC2 (Elastic Compute Cloud) - Virtual Servers**
```bash
# Launch a basic web server
aws ec2 run-instances \
  --image-id ami-0abcdef1234567890 \
  --instance-type t3.micro \
  --key-name my-key-pair \
  --security-group-ids sg-903004f8 \
  --subnet-id subnet-6e7f829e \
  --user-data file://setup-script.sh

# setup-script.sh
#!/bin/bash
yum update -y
yum install -y httpd
systemctl start httpd
systemctl enable httpd
echo "<h1>Hello from AWS EC2</h1>" > /var/www/html/index.html
```

**When to Use EC2:**
- You need full control over the operating system
- Running legacy applications that can't be containerized
- Specific software that requires custom configuration
- Learning cloud fundamentals (start here)

**When NOT to Use EC2:**
- Simple web applications (use App Runner or Elastic Beanstalk)
- Serverless workloads (use Lambda)
- Container workloads (use ECS or EKS)

**2. Lambda - Serverless Functions**
```javascript
// lambda-function.js
exports.handler = async (event) => {
    const name = event.queryStringParameters?.name || 'World';
    
    return {
        statusCode: 200,
        headers: {
            'Content-Type': 'application/json',
            'Access-Control-Allow-Origin': '*'
        },
        body: JSON.stringify({
            message: `Hello, ${name}!`,
            timestamp: new Date().toISOString()
        })
    };
};
```

```bash
# Deploy with AWS CLI
zip function.zip lambda-function.js

aws lambda create-function \
  --function-name hello-world \
  --runtime nodejs18.x \
  --role arn:aws:iam::123456789012:role/lambda-execution-role \
  --handler lambda-function.handler \
  --zip-file fileb://function.zip

# Test the function
aws lambda invoke \
  --function-name hello-world \
  --payload '{"queryStringParameters":{"name":"AWS"}}' \
  response.json
```

**When to Use Lambda:**
- Event-driven processing (file uploads, API requests)
- Scheduled tasks (cron jobs)
- Microservices with simple logic
- Cost optimization for infrequent workloads

### Essential Storage Services

**1. S3 (Simple Storage Service) - Object Storage**
```bash
# Create a bucket
aws s3 mb s3://my-unique-bucket-name-12345

# Upload files
aws s3 cp index.html s3://my-unique-bucket-name-12345/
aws s3 cp assets/ s3://my-unique-bucket-name-12345/assets/ --recursive

# Enable static website hosting
aws s3 website s3://my-unique-bucket-name-12345 \
  --index-document index.html \
  --error-document error.html

# Set public read permissions (for static websites)
aws s3api put-bucket-policy \
  --bucket my-unique-bucket-name-12345 \
  --policy file://bucket-policy.json
```

```json
// bucket-policy.json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "PublicReadGetObject",
      "Effect": "Allow",
      "Principal": "*",
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::my-unique-bucket-name-12345/*"
    }
  ]
}
```

**S3 Use Cases:**
- Static website hosting
- File uploads and downloads
- Data backup and archiving
- Content distribution (with CloudFront)

**2. EBS (Elastic Block Store) - Persistent Storage for EC2**
```bash
# Create a volume
aws ec2 create-volume \
  --size 20 \
  --volume-type gp3 \
  --availability-zone us-west-2a

# Attach to EC2 instance
aws ec2 attach-volume \
  --volume-id vol-1234567890abcdef0 \
  --instance-id i-1234567890abcdef0 \
  --device /dev/sdf

# Format and mount (on the EC2 instance)
sudo mkfs -t xfs /dev/xvdf
sudo mkdir /data
sudo mount /dev/xvdf /data
```

### Essential Database Services

**1. RDS (Relational Database Service) - Managed SQL Databases**
```bash
# Create a PostgreSQL database
aws rds create-db-instance \
  --db-instance-identifier myapp-db \
  --db-instance-class db.t3.micro \
  --engine postgres \
  --engine-version 15.4 \
  --master-username dbadmin \
  --master-user-password SecurePassword123 \
  --allocated-storage 20 \
  --vpc-security-group-ids sg-12345678 \
  --db-subnet-group-name my-db-subnet-group \
  --backup-retention-period 7 \
  --storage-encrypted
```

**Connection Example:**
```javascript
// Node.js connection to RDS
const { Pool } = require('pg');

const pool = new Pool({
  host: 'myapp-db.cluster-xyz.us-west-2.rds.amazonaws.com',
  port: 5432,
  database: 'myapp',
  user: 'dbadmin',
  password: process.env.DB_PASSWORD,
  ssl: { rejectUnauthorized: false }
});

async function getUsers() {
  const client = await pool.connect();
  try {
    const result = await client.query('SELECT * FROM users');
    return result.rows;
  } finally {
    client.release();
  }
}
```

**2. DynamoDB - NoSQL Database**
```javascript
// DynamoDB operations with AWS SDK
const { DynamoDBClient } = require('@aws-sdk/client-dynamodb');
const { DynamoDBDocumentClient, PutCommand, GetCommand, QueryCommand } = require('@aws-sdk/lib-dynamodb');

const client = new DynamoDBClient({ region: 'us-west-2' });
const docClient = DynamoDBDocumentClient.from(client);

// Create item
async function createUser(userId, email, name) {
  const command = new PutCommand({
    TableName: 'Users',
    Item: {
      userId: userId,
      email: email,
      name: name,
      createdAt: new Date().toISOString()
    }
  });
  
  return await docClient.send(command);
}

// Get item
async function getUser(userId) {
  const command = new GetCommand({
    TableName: 'Users',
    Key: { userId: userId }
  });
  
  const response = await docClient.send(command);
  return response.Item;
}

// Query items
async function getUsersByEmail(email) {
  const command = new QueryCommand({
    TableName: 'Users',
    IndexName: 'EmailIndex',
    KeyConditionExpression: 'email = :email',
    ExpressionAttributeValues: {
      ':email': email
    }
  });
  
  const response = await docClient.send(command);
  return response.Items;
}
```

### Essential Networking Services

**1. VPC (Virtual Private Cloud) - Network Isolation**
```bash
# Create VPC
aws ec2 create-vpc --cidr-block 10.0.0.0/16

# Create subnets
aws ec2 create-subnet \
  --vpc-id vpc-12345678 \
  --cidr-block 10.0.1.0/24 \
  --availability-zone us-west-2a

aws ec2 create-subnet \
  --vpc-id vpc-12345678 \
  --cidr-block 10.0.2.0/24 \
  --availability-zone us-west-2b

# Create Internet Gateway
aws ec2 create-internet-gateway
aws ec2 attach-internet-gateway \
  --internet-gateway-id igw-12345678 \
  --vpc-id vpc-12345678

# Create route table
aws ec2 create-route-table --vpc-id vpc-12345678
aws ec2 create-route \
  --route-table-id rtb-12345678 \
  --destination-cidr-block 0.0.0.0/0 \
  --gateway-id igw-12345678
```

**2. Application Load Balancer - Traffic Distribution**
```bash
# Create Application Load Balancer
aws elbv2 create-load-balancer \
  --name my-app-alb \
  --subnets subnet-12345678 subnet-87654321 \
  --security-groups sg-12345678

# Create target group
aws elbv2 create-target-group \
  --name my-app-targets \
  --protocol HTTP \
  --port 80 \
  --vpc-id vpc-12345678 \
  --health-check-path /health

# Register targets
aws elbv2 register-targets \
  --target-group-arn arn:aws:elasticloadbalancing:us-west-2:123456789012:targetgroup/my-app-targets/1234567890123456 \
  --targets Id=i-1234567890abcdef0,Port=80

# Create listener
aws elbv2 create-listener \
  --load-balancer-arn arn:aws:elasticloadbalancing:us-west-2:123456789012:loadbalancer/app/my-app-alb/1234567890123456 \
  --protocol HTTP \
  --port 80 \
  --default-actions Type=forward,TargetGroupArn=arn:aws:elasticloadbalancing:us-west-2:123456789012:targetgroup/my-app-targets/1234567890123456
```

## Azure Fundamentals

### Core Azure Services

**1. Virtual Machines - Compute**
```bash
# Create resource group
az group create --name myResourceGroup --location eastus

# Create virtual machine
az vm create \
  --resource-group myResourceGroup \
  --name myVM \
  --image Ubuntu2204 \
  --admin-username azureuser \
  --generate-ssh-keys \
  --size Standard_B1s

# Open port 80
az vm open-port --port 80 --resource-group myResourceGroup --name myVM

# Connect to VM
ssh azureuser@<public-ip-address>
```

**2. App Service - Managed Web Apps**
```bash
# Create App Service plan
az appservice plan create \
  --name myAppServicePlan \
  --resource-group myResourceGroup \
  --sku B1 \
  --is-linux

# Create web app
az webapp create \
  --resource-group myResourceGroup \
  --plan myAppServicePlan \
  --name myUniqueWebApp \
  --runtime "NODE|18-lts"

# Deploy from local Git
az webapp deployment source config-local-git \
  --name myUniqueWebApp \
  --resource-group myResourceGroup

# Configure app settings
az webapp config appsettings set \
  --resource-group myResourceGroup \
  --name myUniqueWebApp \
  --settings DATABASE_URL="postgresql://..." NODE_ENV="production"
```

**3. Azure Functions - Serverless**
```javascript
// Azure Function (HTTP trigger)
module.exports = async function (context, req) {
    const name = req.query.name || (req.body && req.body.name) || 'World';
    
    context.res = {
        status: 200,
        headers: {
            'Content-Type': 'application/json'
        },
        body: {
            message: `Hello, ${name}!`,
            timestamp: new Date().toISOString()
        }
    };
};
```

```bash
# Create Function App
az functionapp create \
  --resource-group myResourceGroup \
  --consumption-plan-location eastus \
  --runtime node \
  --runtime-version 18 \
  --functions-version 4 \
  --name myUniqueFunctionApp \
  --storage-account mystorageaccount

# Deploy function
func azure functionapp publish myUniqueFunctionApp
```

### Azure Storage Services

**1. Blob Storage - Object Storage**
```bash
# Create storage account
az storage account create \
  --name mystorageaccount \
  --resource-group myResourceGroup \
  --location eastus \
  --sku Standard_LRS

# Create container
az storage container create \
  --name mycontainer \
  --account-name mystorageaccount \
  --public-access blob

# Upload file
az storage blob upload \
  --file index.html \
  --container-name mycontainer \
  --name index.html \
  --account-name mystorageaccount
```

**2. Azure SQL Database**
```bash
# Create SQL server
az sql server create \
  --name myserver \
  --resource-group myResourceGroup \
  --location eastus \
  --admin-user myadmin \
  --admin-password MyPassword123!

# Create database
az sql db create \
  --resource-group myResourceGroup \
  --server myserver \
  --name mydatabase \
  --service-objective Basic

# Configure firewall
az sql server firewall-rule create \
  --resource-group myResourceGroup \
  --server myserver \
  --name AllowAzureIps \
  --start-ip-address 0.0.0.0 \
  --end-ip-address 0.0.0.0
```

## Google Cloud Fundamentals

### Core GCP Services

**1. Compute Engine - Virtual Machines**
```bash
# Create VM instance
gcloud compute instances create my-vm \
  --zone=us-central1-a \
  --machine-type=e2-micro \
  --image-family=ubuntu-2004-lts \
  --image-project=ubuntu-os-cloud \
  --boot-disk-size=10GB \
  --tags=http-server

# Create firewall rule
gcloud compute firewall-rules create allow-http \
  --allow tcp:80 \
  --source-ranges 0.0.0.0/0 \
  --target-tags http-server

# SSH into instance
gcloud compute ssh my-vm --zone=us-central1-a
```

**2. Cloud Run - Containerized Applications**
```dockerfile
# Dockerfile
FROM node:18-alpine
WORKDIR /app
COPY package*.json ./
RUN npm ci --only=production
COPY . .
EXPOSE 8080
CMD ["npm", "start"]
```

```bash
# Build and deploy to Cloud Run
gcloud builds submit --tag gcr.io/PROJECT_ID/my-app
gcloud run deploy my-app \
  --image gcr.io/PROJECT_ID/my-app \
  --platform managed \
  --region us-central1 \
  --allow-unauthenticated
```

**3. Cloud Functions - Serverless**
```javascript
// Cloud Function (HTTP)
const functions = require('@google-cloud/functions-framework');

functions.http('helloWorld', (req, res) => {
  const name = req.query.name || req.body.name || 'World';
  res.json({
    message: `Hello, ${name}!`,
    timestamp: new Date().toISOString()
  });
});
```

```bash
# Deploy function
gcloud functions deploy helloWorld \
  --runtime nodejs18 \
  --trigger-http \
  --allow-unauthenticated \
  --source .
```

### GCP Storage and Database Services

**1. Cloud Storage - Object Storage**
```bash
# Create bucket
gsutil mb gs://my-unique-bucket-name

# Upload files
gsutil cp index.html gs://my-unique-bucket-name/
gsutil cp -r assets/ gs://my-unique-bucket-name/

# Make bucket public
gsutil iam ch allUsers:objectViewer gs://my-unique-bucket-name

# Enable website configuration
gsutil web set -m index.html -e 404.html gs://my-unique-bucket-name
```

**2. Cloud SQL - Managed Databases**
```bash
# Create Cloud SQL instance
gcloud sql instances create myinstance \
  --database-version=POSTGRES_15 \
  --tier=db-f1-micro \
  --region=us-central1

# Create database
gcloud sql databases create mydatabase --instance=myinstance

# Create user
gcloud sql users create myuser \
  --instance=myinstance \
  --password=mypassword

# Connect to instance
gcloud sql connect myinstance --user=myuser --database=mydatabase
```

## Cost Optimization Strategies

### Understanding Cloud Pricing

**The Reality:** Cloud can be expensive if you don't understand the pricing model.

**Common Cost Traps:**
- Leaving resources running when not needed
- Over-provisioning compute resources
- Not using reserved instances for predictable workloads
- Ignoring data transfer costs
- Using expensive services for simple tasks

### AWS Cost Optimization

**1. Right-Sizing Instances**
```bash
# Use AWS Cost Explorer API to analyze usage
aws ce get-usage-and-costs \
  --time-period Start=2024-01-01,End=2024-01-31 \
  --granularity MONTHLY \
  --metrics BlendedCost \
  --group-by Type=DIMENSION,Key=SERVICE

# Use AWS Compute Optimizer recommendations
aws compute-optimizer get-ec2-instance-recommendations \
  --instance-arns arn:aws:ec2:us-west-2:123456789012:instance/i-1234567890abcdef0
```

**2. Reserved Instances and Savings Plans**
```bash
# Purchase Reserved Instance
aws ec2 purchase-reserved-instances-offering \
  --reserved-instances-offering-id 12345678-1234-1234-1234-123456789012 \
  --instance-count 1

# Create Savings Plan
aws savingsplans create-savings-plan \
  --savings-plan-type Compute \
  --term-duration-in-years 1 \
  --payment-option AllUpfront \
  --commitment 10.00
```

**3. Auto Scaling**
```bash
# Create Auto Scaling Group
aws autoscaling create-auto-scaling-group \
  --auto-scaling-group-name my-asg \
  --launch-template LaunchTemplateName=my-template,Version=1 \
  --min-size 1 \
  --max-size 10 \
  --desired-capacity 2 \
  --vpc-zone-identifier "subnet-12345678,subnet-87654321"

# Create scaling policies
aws autoscaling put-scaling-policy \
  --auto-scaling-group-name my-asg \
  --policy-name scale-up \
  --scaling-adjustment 1 \
  --adjustment-type ChangeInCapacity
```

### Azure Cost Optimization

**1. Azure Advisor Recommendations**
```bash
# Get cost recommendations
az advisor recommendation list \
  --category Cost \
  --output table

# Get right-sizing recommendations
az advisor recommendation list \
  --category Cost \
  --query "[?contains(shortDescription.solution, 'right-size')]"
```

**2. Reserved Instances**
```bash
# Purchase Reserved VM Instance
az reservations reservation-order purchase \
  --reservation-order-id "12345678-1234-1234-1234-123456789012" \
  --sku "Standard_D2s_v3" \
  --location "East US" \
  --quantity 1 \
  --term P1Y
```

### Google Cloud Cost Optimization

**1. Committed Use Discounts**
```bash
# Create committed use contract
gcloud compute commitments create my-commitment \
  --plan 12-month \
  --region us-central1 \
  --resources type=VCPU,amount=10 \
  --resources type=MEMORY,amount=40GB
```

**2. Preemptible Instances**
```bash
# Create preemptible instance (up to 80% cost savings)
gcloud compute instances create my-preemptible-vm \
  --zone us-central1-a \
  --machine-type n1-standard-1 \
  --preemptible \
  --image-family ubuntu-2004-lts \
  --image-project ubuntu-os-cloud
```

## Security Best Practices

### Identity and Access Management (IAM)

**Principle of Least Privilege:** Give users and services only the minimum permissions they need.

**AWS IAM Example:**
```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "s3:GetObject",
        "s3:PutObject"
      ],
      "Resource": "arn:aws:s3:::my-app-bucket/*"
    },
    {
      "Effect": "Allow",
      "Action": "s3:ListBucket",
      "Resource": "arn:aws:s3:::my-app-bucket"
    }
  ]
}
```

**Azure RBAC Example:**
```bash
# Create custom role
az role definition create --role-definition '{
  "Name": "App Storage Contributor",
  "Description": "Can read and write to specific storage account",
  "Actions": [
    "Microsoft.Storage/storageAccounts/blobServices/containers/read",
    "Microsoft.Storage/storageAccounts/blobServices/containers/write"
  ],
  "AssignableScopes": ["/subscriptions/12345678-1234-1234-1234-123456789012"]
}'

# Assign role to user
az role assignment create \
  --assignee user@example.com \
  --role "App Storage Contributor" \
  --scope "/subscriptions/12345678-1234-1234-1234-123456789012/resourceGroups/myResourceGroup"
```

### Network Security

**1. Security Groups (AWS) / Network Security Groups (Azure)**
```bash
# AWS Security Group
aws ec2 create-security-group \
  --group-name web-server-sg \
  --description "Security group for web servers"

# Allow HTTP from anywhere
aws ec2 authorize-security-group-ingress \
  --group-id sg-12345678 \
  --protocol tcp \
  --port 80 \
  --cidr 0.0.0.0/0

# Allow SSH from specific IP
aws ec2 authorize-security-group-ingress \
  --group-id sg-12345678 \
  --protocol tcp \
  --port 22 \
  --cidr 203.0.113.0/24
```

**2. Private Subnets and NAT Gateways**
```bash
# Create private subnet
aws ec2 create-subnet \
  --vpc-id vpc-12345678 \
  --cidr-block 10.0.3.0/24 \
  --availability-zone us-west-2a

# Create NAT Gateway for outbound internet access
aws ec2 create-nat-gateway \
  --subnet-id subnet-12345678 \
  --allocation-id eipalloc-12345678

# Route private subnet traffic through NAT Gateway
aws ec2 create-route \
  --route-table-id rtb-87654321 \
  --destination-cidr-block 0.0.0.0/0 \
  --nat-gateway-id nat-12345678
```

### Secrets Management

**AWS Secrets Manager:**
```bash
# Store secret
aws secretsmanager create-secret \
  --name "myapp/database" \
  --description "Database credentials for MyApp" \
  --secret-string '{"username":"admin","password":"SecurePassword123"}'

# Retrieve secret in application
aws secretsmanager get-secret-value \
  --secret-id "myapp/database" \
  --query SecretString \
  --output text
```

```javascript
// Node.js example
const { SecretsManagerClient, GetSecretValueCommand } = require('@aws-sdk/client-secrets-manager');

async function getDatabaseCredentials() {
  const client = new SecretsManagerClient({ region: 'us-west-2' });
  const command = new GetSecretValueCommand({ SecretId: 'myapp/database' });
  
  const response = await client.send(command);
  return JSON.parse(response.SecretString);
}
```

**Azure Key Vault:**
```bash
# Create Key Vault
az keyvault create \
  --name myKeyVault \
  --resource-group myResourceGroup \
  --location eastus

# Store secret
az keyvault secret set \
  --vault-name myKeyVault \
  --name "database-password" \
  --value "SecurePassword123"

# Retrieve secret
az keyvault secret show \
  --vault-name myKeyVault \
  --name "database-password" \
  --query value \
  --output tsv
```

## Common Cloud Mistakes and How to Avoid Them

### Cost Management Mistakes

**1. Forgetting to Stop Development Resources**
```bash
# WRONG: Leaving development instances running 24/7
# Cost: $50-200/month for unused resources

# RIGHT: Automate start/stop for development environments
# AWS Lambda function to stop instances at night
aws events put-rule \
  --name stop-dev-instances \
  --schedule-expression "cron(0 22 * * ? *)"

aws lambda add-permission \
  --function-name stop-dev-instances \
  --statement-id allow-cloudwatch \
  --action lambda:InvokeFunction \
  --principal events.amazonaws.com
```

**2. Not Using Appropriate Instance Sizes**
```bash
# WRONG: Using large instances for small workloads
# t3.large for a simple API that could run on t3.micro

# RIGHT: Start small and scale up based on actual usage
# Monitor CloudWatch metrics and resize accordingly
aws cloudwatch get-metric-statistics \
  --namespace AWS/EC2 \
  --metric-name CPUUtilization \
  --dimensions Name=InstanceId,Value=i-1234567890abcdef0 \
  --start-time 2024-01-01T00:00:00Z \
  --end-time 2024-01-31T23:59:59Z \
  --period 3600 \
  --statistics Average
```

### Security Mistakes

**1. Overly Permissive Security Groups**
```bash
# WRONG: Opening all ports to the world
aws ec2 authorize-security-group-ingress \
  --group-id sg-12345678 \
  --protocol tcp \
  --port 0-65535 \
  --cidr 0.0.0.0/0

# RIGHT: Specific ports and sources
aws ec2 authorize-security-group-ingress \
  --group-id sg-12345678 \
  --protocol tcp \
  --port 443 \
  --cidr 0.0.0.0/0  # HTTPS from anywhere

aws ec2 authorize-security-group-ingress \
  --group-id sg-12345678 \
  --protocol tcp \
  --port 22 \
  --cidr 10.0.0.0/8  # SSH from private network only
```

**2. Hardcoding Credentials**
```javascript
// WRONG: Credentials in code
const config = {
  accessKeyId: 'AKIAIOSFODNN7EXAMPLE',
  secretAccessKey: 'wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY',
  region: 'us-west-2'
};

// RIGHT: Use IAM roles or environment variables
const config = {
  region: process.env.AWS_REGION || 'us-west-2'
  // AWS SDK automatically uses IAM role credentials
};
```

### Architecture Mistakes

**1. Single Point of Failure**
```bash
# WRONG: Single instance in one availability zone
aws ec2 run-instances \
  --image-id ami-12345678 \
  --instance-type t3.micro \
  --subnet-id subnet-12345678  # Only one AZ

# RIGHT: Multi-AZ deployment with load balancer
aws ec2 run-instances \
  --image-id ami-12345678 \
  --instance-type t3.micro \
  --subnet-id subnet-12345678  # AZ-a

aws ec2 run-instances \
  --image-id ami-12345678 \
  --instance-type t3.micro \
  --subnet-id subnet-87654321  # AZ-b
```

**2. Not Using Managed Services**
```bash
# WRONG: Managing your own database on EC2
# Requires: backups, patching, monitoring, scaling, security

# RIGHT: Use managed database service
aws rds create-db-instance \
  --db-instance-identifier myapp-db \
  --db-instance-class db.t3.micro \
  --engine postgres \
  --multi-az  # Automatic failover
  --backup-retention-period 7  # Automatic backups
```

## Next Steps

1. **Choose One Cloud Provider**: Don't try to learn all three simultaneously
2. **Start with Core Services**: Focus on compute, storage, and networking basics
3. **Build Real Projects**: Deploy actual applications, not just tutorials
4. **Monitor Costs**: Set up billing alerts and review usage regularly
5. **Learn Security Fundamentals**: Understand IAM, network security, and secrets management
6. **Automate Everything**: Use Infrastructure as Code from the beginning

Remember: Cloud platforms are tools to solve business problems, not ends in themselves. Focus on understanding the problems you're solving and choose the simplest cloud services that meet your needs.