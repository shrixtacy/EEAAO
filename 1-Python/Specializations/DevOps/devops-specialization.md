# Python for DevOps - Infrastructure Automation Path

Build and manage infrastructure with code. This isn't about memorizing cloud services - it's about automating deployment, monitoring, and scaling systems that run reliably in production.

## Reality Check First

**What DevOps Actually Is:**
- 60% understanding infrastructure and system administration
- 25% automation and scripting
- 10% cloud platform specifics
- 5% the "cool" container orchestration you see in demos

**What You'll Actually Build:**
- Infrastructure as Code (IaC) templates
- Deployment automation pipelines
- Monitoring and alerting systems
- Log analysis and processing tools
- Configuration management scripts
- Container orchestration solutions

**What You Won't Become:**
- A cloud architect overnight (requires deep infrastructure knowledge)
- Someone who can ignore security (DevSecOps is essential)
- A replacement for proper system design

## Prerequisites

**From Python Fundamentals:**
- Comfortable with file operations, APIs, and error handling
- Understanding of modules, packages, and virtual environments
- Experience with JSON, YAML, and configuration files

**Additional Requirements:**
- **Linux/Unix Basics**: Command line, file permissions, process management
- **Networking Fundamentals**: TCP/IP, DNS, HTTP/HTTPS, load balancing
- **Version Control**: Git workflows, branching strategies
- **Basic Security**: SSH keys, certificates, secrets management

## Phase 1: Infrastructure as Code

### AWS Infrastructure with Boto3

**Why Boto3**: Direct programmatic access to AWS services

```python
import boto3
import json
import time
from botocore.exceptions import ClientError, NoCredentialsError
import logging

class AWSInfrastructureManager:
    def __init__(self, region='us-west-2'):
        self.region = region
        self.logger = logging.getLogger(__name__)
        
        try:
            # Initialize AWS clients
            self.ec2 = boto3.client('ec2', region_name=region)
            self.s3 = boto3.client('s3', region_name=region)
            self.iam = boto3.client('iam', region_name=region)
            self.cloudformation = boto3.client('cloudformation', region_name=region)
            self.logs = boto3.client('logs', region_name=region)
            
        except NoCredentialsError:
            self.logger.error("AWS credentials not found. Configure with 'aws configure'")
            raise
    
    def create_vpc_infrastructure(self, project_name):
        """Create a complete VPC infrastructure"""
        try:
            # Create VPC
            vpc_response = self.ec2.create_vpc(
                CidrBlock='10.0.0.0/16',
                TagSpecifications=[{
                    'ResourceType': 'vpc',
                    'Tags': [
                        {'Key': 'Name', 'Value': f'{project_name}-vpc'},
                        {'Key': 'Project', 'Value': project_name}
                    ]
                }]
            )
            vpc_id = vpc_response['Vpc']['VpcId']
            self.logger.info(f"Created VPC: {vpc_id}")
            
            # Enable DNS hostnames
            self.ec2.modify_vpc_attribute(
                VpcId=vpc_id,
                EnableDnsHostnames={'Value': True}
            )
            
            # Create Internet Gateway
            igw_response = self.ec2.create_internet_gateway(
                TagSpecifications=[{
                    'ResourceType': 'internet-gateway',
                    'Tags': [
                        {'Key': 'Name', 'Value': f'{project_name}-igw'},
                        {'Key': 'Project', 'Value': project_name}
                    ]
                }]
            )
            igw_id = igw_response['InternetGateway']['InternetGatewayId']
            
            # Attach Internet Gateway to VPC
            self.ec2.attach_internet_gateway(
                InternetGatewayId=igw_id,
                VpcId=vpc_id
            )
            
            # Create public subnet
            public_subnet_response = self.ec2.create_subnet(
                VpcId=vpc_id,
                CidrBlock='10.0.1.0/24',
                AvailabilityZone=f'{self.region}a',
                TagSpecifications=[{
                    'ResourceType': 'subnet',
                    'Tags': [
                        {'Key': 'Name', 'Value': f'{project_name}-public-subnet'},
                        {'Key': 'Project', 'Value': project_name},
                        {'Key': 'Type', 'Value': 'Public'}
                    ]
                }]
            )
            public_subnet_id = public_subnet_response['Subnet']['SubnetId']
            
            # Create private subnet
            private_subnet_response = self.ec2.create_subnet(
                VpcId=vpc_id,
                CidrBlock='10.0.2.0/24',
                AvailabilityZone=f'{self.region}b',
                TagSpecifications=[{
                    'ResourceType': 'subnet',
                    'Tags': [
                        {'Key': 'Name', 'Value': f'{project_name}-private-subnet'},
                        {'Key': 'Project', 'Value': project_name},
                        {'Key': 'Type', 'Value': 'Private'}
                    ]
                }]
            )
            private_subnet_id = private_subnet_response['Subnet']['SubnetId']
            
            # Create route table for public subnet
            route_table_response = self.ec2.create_route_table(
                VpcId=vpc_id,
                TagSpecifications=[{
                    'ResourceType': 'route-table',
                    'Tags': [
                        {'Key': 'Name', 'Value': f'{project_name}-public-rt'},
                        {'Key': 'Project', 'Value': project_name}
                    ]
                }]
            )
            route_table_id = route_table_response['RouteTable']['RouteTableId']
            
            # Add route to Internet Gateway
            self.ec2.create_route(
                RouteTableId=route_table_id,
                DestinationCidrBlock='0.0.0.0/0',
                GatewayId=igw_id
            )
            
            # Associate route table with public subnet
            self.ec2.associate_route_table(
                RouteTableId=route_table_id,
                SubnetId=public_subnet_id
            )
            
            infrastructure = {
                'vpc_id': vpc_id,
                'internet_gateway_id': igw_id,
                'public_subnet_id': public_subnet_id,
                'private_subnet_id': private_subnet_id,
                'route_table_id': route_table_id,
                'project_name': project_name
            }
            
            self.logger.info(f"VPC infrastructure created successfully for {project_name}")
            return infrastructure
            
        except ClientError as e:
            self.logger.error(f"Error creating VPC infrastructure: {e}")
            return None
    
    def create_security_groups(self, vpc_id, project_name):
        """Create security groups for web and database tiers"""
        try:
            # Web tier security group
            web_sg_response = self.ec2.create_security_group(
                GroupName=f'{project_name}-web-sg',
                Description='Security group for web servers',
                VpcId=vpc_id,
                TagSpecifications=[{
                    'ResourceType': 'security-group',
                    'Tags': [
                        {'Key': 'Name', 'Value': f'{project_name}-web-sg'},
                        {'Key': 'Project', 'Value': project_name}
                    ]
                }]
            )
            web_sg_id = web_sg_response['GroupId']
            
            # Add inbound rules for web tier
            self.ec2.authorize_security_group_ingress(
                GroupId=web_sg_id,
                IpPermissions=[
                    {
                        'IpProtocol': 'tcp',
                        'FromPort': 80,
                        'ToPort': 80,
                        'IpRanges': [{'CidrIp': '0.0.0.0/0', 'Description': 'HTTP access'}]
                    },
                    {
                        'IpProtocol': 'tcp',
                        'FromPort': 443,
                        'ToPort': 443,
                        'IpRanges': [{'CidrIp': '0.0.0.0/0', 'Description': 'HTTPS access'}]
                    },
                    {
                        'IpProtocol': 'tcp',
                        'FromPort': 22,
                        'ToPort': 22,
                        'IpRanges': [{'CidrIp': '0.0.0.0/0', 'Description': 'SSH access'}]
                    }
                ]
            )
            
            # Database tier security group
            db_sg_response = self.ec2.create_security_group(
                GroupName=f'{project_name}-db-sg',
                Description='Security group for database servers',
                VpcId=vpc_id,
                TagSpecifications=[{
                    'ResourceType': 'security-group',
                    'Tags': [
                        {'Key': 'Name', 'Value': f'{project_name}-db-sg'},
                        {'Key': 'Project', 'Value': project_name}
                    ]
                }]
            )
            db_sg_id = db_sg_response['GroupId']
            
            # Add inbound rule for database tier (only from web tier)
            self.ec2.authorize_security_group_ingress(
                GroupId=db_sg_id,
                IpPermissions=[
                    {
                        'IpProtocol': 'tcp',
                        'FromPort': 3306,
                        'ToPort': 3306,
                        'UserIdGroupPairs': [{'GroupId': web_sg_id, 'Description': 'MySQL from web tier'}]
                    }
                ]
            )
            
            return {
                'web_security_group_id': web_sg_id,
                'db_security_group_id': db_sg_id
            }
            
        except ClientError as e:
            self.logger.error(f"Error creating security groups: {e}")
            return None
    
    def launch_ec2_instances(self, infrastructure, security_groups, instance_config):
        """Launch EC2 instances with proper configuration"""
        try:
            instances = []
            
            # Launch web server instances
            for i in range(instance_config.get('web_instance_count', 2)):
                user_data_script = """#!/bin/bash
                yum update -y
                yum install -y httpd
                systemctl start httpd
                systemctl enable httpd
                echo "<h1>Web Server {}</h1>" > /var/www/html/index.html
                """.format(i + 1)
                
                response = self.ec2.run_instances(
                    ImageId=instance_config.get('ami_id', 'ami-0c02fb55956c7d316'),  # Amazon Linux 2
                    MinCount=1,
                    MaxCount=1,
                    InstanceType=instance_config.get('instance_type', 't3.micro'),
                    KeyName=instance_config.get('key_pair_name'),
                    SecurityGroupIds=[security_groups['web_security_group_id']],
                    SubnetId=infrastructure['public_subnet_id'],
                    UserData=user_data_script,
                    TagSpecifications=[{
                        'ResourceType': 'instance',
                        'Tags': [
                            {'Key': 'Name', 'Value': f"{infrastructure['project_name']}-web-{i+1}"},
                            {'Key': 'Project', 'Value': infrastructure['project_name']},
                            {'Key': 'Tier', 'Value': 'Web'}
                        ]
                    }]
                )
                
                instance_id = response['Instances'][0]['InstanceId']
                instances.append({
                    'instance_id': instance_id,
                    'type': 'web',
                    'name': f"{infrastructure['project_name']}-web-{i+1}"
                })
                
                self.logger.info(f"Launched web instance: {instance_id}")
            
            return instances
            
        except ClientError as e:
            self.logger.error(f"Error launching instances: {e}")
            return []
    
    def create_s3_bucket(self, bucket_name, project_name):
        """Create S3 bucket with proper configuration"""
        try:
            # Create bucket
            if self.region == 'us-east-1':
                self.s3.create_bucket(Bucket=bucket_name)
            else:
                self.s3.create_bucket(
                    Bucket=bucket_name,
                    CreateBucketConfiguration={'LocationConstraint': self.region}
                )
            
            # Add tags
            self.s3.put_bucket_tagging(
                Bucket=bucket_name,
                Tagging={
                    'TagSet': [
                        {'Key': 'Project', 'Value': project_name},
                        {'Key': 'Environment', 'Value': 'production'}
                    ]
                }
            )
            
            # Enable versioning
            self.s3.put_bucket_versioning(
                Bucket=bucket_name,
                VersioningConfiguration={'Status': 'Enabled'}
            )
            
            # Set up lifecycle policy
            lifecycle_policy = {
                'Rules': [
                    {
                        'ID': 'DeleteOldVersions',
                        'Status': 'Enabled',
                        'NoncurrentVersionExpiration': {'NoncurrentDays': 30}
                    }
                ]
            }
            
            self.s3.put_bucket_lifecycle_configuration(
                Bucket=bucket_name,
                LifecycleConfiguration=lifecycle_policy
            )
            
            self.logger.info(f"Created S3 bucket: {bucket_name}")
            return bucket_name
            
        except ClientError as e:
            self.logger.error(f"Error creating S3 bucket: {e}")
            return None
    
    def setup_cloudwatch_monitoring(self, instances, project_name):
        """Set up CloudWatch monitoring and alarms"""
        try:
            cloudwatch = boto3.client('cloudwatch', region_name=self.region)
            
            for instance in instances:
                instance_id = instance['instance_id']
                instance_name = instance['name']
                
                # CPU utilization alarm
                cloudwatch.put_metric_alarm(
                    AlarmName=f'{instance_name}-high-cpu',
                    ComparisonOperator='GreaterThanThreshold',
                    EvaluationPeriods=2,
                    MetricName='CPUUtilization',
                    Namespace='AWS/EC2',
                    Period=300,
                    Statistic='Average',
                    Threshold=80.0,
                    ActionsEnabled=True,
                    AlarmDescription=f'High CPU utilization for {instance_name}',
                    Dimensions=[
                        {
                            'Name': 'InstanceId',
                            'Value': instance_id
                        }
                    ],
                    Unit='Percent',
                    Tags=[
                        {'Key': 'Project', 'Value': project_name}
                    ]
                )
                
                # Status check alarm
                cloudwatch.put_metric_alarm(
                    AlarmName=f'{instance_name}-status-check',
                    ComparisonOperator='GreaterThanThreshold',
                    EvaluationPeriods=2,
                    MetricName='StatusCheckFailed',
                    Namespace='AWS/EC2',
                    Period=300,
                    Statistic='Maximum',
                    Threshold=0,
                    ActionsEnabled=True,
                    AlarmDescription=f'Status check failed for {instance_name}',
                    Dimensions=[
                        {
                            'Name': 'InstanceId',
                            'Value': instance_id
                        }
                    ],
                    Tags=[
                        {'Key': 'Project', 'Value': project_name}
                    ]
                )
            
            self.logger.info(f"Set up CloudWatch monitoring for {len(instances)} instances")
            return True
            
        except ClientError as e:
            self.logger.error(f"Error setting up CloudWatch monitoring: {e}")
            return False
    
    def get_infrastructure_status(self, project_name):
        """Get status of all infrastructure components for a project"""
        try:
            status = {
                'project_name': project_name,
                'timestamp': time.strftime('%Y-%m-%d %H:%M:%S'),
                'components': {}
            }
            
            # Get VPC information
            vpcs = self.ec2.describe_vpcs(
                Filters=[
                    {'Name': 'tag:Project', 'Values': [project_name]}
                ]
            )
            status['components']['vpcs'] = len(vpcs['Vpcs'])
            
            # Get EC2 instances
            instances = self.ec2.describe_instances(
                Filters=[
                    {'Name': 'tag:Project', 'Values': [project_name]},
                    {'Name': 'instance-state-name', 'Values': ['running', 'stopped', 'pending']}
                ]
            )
            
            instance_count = 0
            running_instances = 0
            for reservation in instances['Reservations']:
                for instance in reservation['Instances']:
                    instance_count += 1
                    if instance['State']['Name'] == 'running':
                        running_instances += 1
            
            status['components']['instances'] = {
                'total': instance_count,
                'running': running_instances
            }
            
            # Get S3 buckets
            buckets = self.s3.list_buckets()
            project_buckets = []
            for bucket in buckets['Buckets']:
                try:
                    tags = self.s3.get_bucket_tagging(Bucket=bucket['Name'])
                    for tag in tags['TagSet']:
                        if tag['Key'] == 'Project' and tag['Value'] == project_name:
                            project_buckets.append(bucket['Name'])
                            break
                except ClientError:
                    pass  # No tags or access denied
            
            status['components']['s3_buckets'] = len(project_buckets)
            
            return status
            
        except ClientError as e:
            self.logger.error(f"Error getting infrastructure status: {e}")
            return None

# Usage example
def deploy_web_application():
    """Deploy a complete web application infrastructure"""
    project_name = "my-web-app"
    
    # Initialize infrastructure manager
    infra = AWSInfrastructureManager(region='us-west-2')
    
    try:
        # Create VPC infrastructure
        print("Creating VPC infrastructure...")
        vpc_infrastructure = infra.create_vpc_infrastructure(project_name)
        if not vpc_infrastructure:
            print("Failed to create VPC infrastructure")
            return
        
        # Create security groups
        print("Creating security groups...")
        security_groups = infra.create_security_groups(
            vpc_infrastructure['vpc_id'], 
            project_name
        )
        if not security_groups:
            print("Failed to create security groups")
            return
        
        # Launch EC2 instances
        print("Launching EC2 instances...")
        instance_config = {
            'web_instance_count': 2,
            'instance_type': 't3.micro',
            'key_pair_name': 'my-key-pair',  # Make sure this exists
            'ami_id': 'ami-0c02fb55956c7d316'  # Amazon Linux 2
        }
        
        instances = infra.launch_ec2_instances(
            vpc_infrastructure, 
            security_groups, 
            instance_config
        )
        
        # Create S3 bucket for static assets
        print("Creating S3 bucket...")
        bucket_name = f"{project_name}-static-assets-{int(time.time())}"
        infra.create_s3_bucket(bucket_name, project_name)
        
        # Set up monitoring
        print("Setting up CloudWatch monitoring...")
        infra.setup_cloudwatch_monitoring(instances, project_name)
        
        # Get final status
        print("Getting infrastructure status...")
        status = infra.get_infrastructure_status(project_name)
        print(json.dumps(status, indent=2))
        
        print(f"Infrastructure deployment completed for {project_name}")
        
    except Exception as e:
        print(f"Deployment failed: {e}")

if __name__ == "__main__":
    # Uncomment to run deployment
    # deploy_web_application()
    pass
```

### Configuration Management with Ansible

**Real-World Application**: Managing server configurations at scale

```python
import yaml
import subprocess
import os
from pathlib import Path
import tempfile
import json

class AnsibleManager:
    def __init__(self, inventory_file=None):
        self.inventory_file = inventory_file or 'inventory.yml'
        self.playbook_dir = Path('playbooks')
        self.playbook_dir.mkdir(exist_ok=True)
        
        # Ensure Ansible is installed
        try:
            subprocess.run(['ansible', '--version'], check=True, capture_output=True)
        except (subprocess.CalledProcessError, FileNotFoundError):
            raise RuntimeError("Ansible is not installed. Install with: pip install ansible")
    
    def create_inventory(self, servers):
        """Create Ansible inventory file"""
        inventory = {
            'all': {
                'children': {
                    'webservers': {
                        'hosts': {},
                        'vars': {
                            'ansible_user': 'ec2-user',
                            'ansible_ssh_private_key_file': '~/.ssh/my-key.pem'
                        }
                    },
                    'databases': {
                        'hosts': {},
                        'vars': {
                            'ansible_user': 'ec2-user',
                            'ansible_ssh_private_key_file': '~/.ssh/my-key.pem'
                        }
                    }
                }
            }
        }
        
        for server in servers:
            group = server.get('group', 'webservers')
            host_vars = {
                'ansible_host': server['ip_address']
            }
            
            if 'vars' in server:
                host_vars.update(server['vars'])
            
            inventory['all']['children'][group]['hosts'][server['name']] = host_vars
        
        with open(self.inventory_file, 'w') as f:
            yaml.dump(inventory, f, default_flow_style=False)
        
        print(f"Created inventory file: {self.inventory_file}")
    
    def create_web_server_playbook(self):
        """Create playbook for web server configuration"""
        playbook = [
            {
                'name': 'Configure Web Servers',
                'hosts': 'webservers',
                'become': True,
                'vars': {
                    'app_name': 'my-web-app',
                    'app_user': 'webapp',
                    'app_dir': '/opt/webapp'
                },
                'tasks': [
                    {
                        'name': 'Update system packages',
                        'yum': {
                            'name': '*',
                            'state': 'latest'
                        }
                    },
                    {
                        'name': 'Install required packages',
                        'yum': {
                            'name': ['httpd', 'python3', 'python3-pip', 'git'],
                            'state': 'present'
                        }
                    },
                    {
                        'name': 'Create application user',
                        'user': {
                            'name': '{{ app_user }}',
                            'system': True,
                            'shell': '/bin/bash',
                            'home': '{{ app_dir }}'
                        }
                    },
                    {
                        'name': 'Create application directory',
                        'file': {
                            'path': '{{ app_dir }}',
                            'state': 'directory',
                            'owner': '{{ app_user }}',
                            'group': '{{ app_user }}',
                            'mode': '0755'
                        }
                    },
                    {
                        'name': 'Configure Apache virtual host',
                        'template': {
                            'src': 'vhost.conf.j2',
                            'dest': '/etc/httpd/conf.d/{{ app_name }}.conf'
                        },
                        'notify': 'restart apache'
                    },
                    {
                        'name': 'Start and enable Apache',
                        'systemd': {
                            'name': 'httpd',
                            'state': 'started',
                            'enabled': True
                        }
                    },
                    {
                        'name': 'Configure firewall',
                        'firewalld': {
                            'service': 'http',
                            'permanent': True,
                            'state': 'enabled',
                            'immediate': True
                        }
                    }
                ],
                'handlers': [
                    {
                        'name': 'restart apache',
                        'systemd': {
                            'name': 'httpd',
                            'state': 'restarted'
                        }
                    }
                ]
            }
        ]
        
        playbook_file = self.playbook_dir / 'web_servers.yml'
        with open(playbook_file, 'w') as f:
            yaml.dump(playbook, f, default_flow_style=False)
        
        # Create Apache virtual host template
        templates_dir = Path('templates')
        templates_dir.mkdir(exist_ok=True)
        
        vhost_template = """<VirtualHost *:80>
    ServerName {{ ansible_fqdn }}
    DocumentRoot {{ app_dir }}/public
    
    <Directory {{ app_dir }}/public>
        AllowOverride All
        Require all granted
    </Directory>
    
    ErrorLog /var/log/httpd/{{ app_name }}_error.log
    CustomLog /var/log/httpd/{{ app_name }}_access.log combined
</VirtualHost>
"""
        
        with open(templates_dir / 'vhost.conf.j2', 'w') as f:
            f.write(vhost_template)
        
        print(f"Created web server playbook: {playbook_file}")
        return playbook_file
    
    def create_database_playbook(self):
        """Create playbook for database server configuration"""
        playbook = [
            {
                'name': 'Configure Database Servers',
                'hosts': 'databases',
                'become': True,
                'vars': {
                    'mysql_root_password': '{{ vault_mysql_root_password }}',
                    'app_db_name': 'webapp_db',
                    'app_db_user': 'webapp_user',
                    'app_db_password': '{{ vault_app_db_password }}'
                },
                'tasks': [
                    {
                        'name': 'Install MySQL',
                        'yum': {
                            'name': ['mysql-server', 'python3-PyMySQL'],
                            'state': 'present'
                        }
                    },
                    {
                        'name': 'Start and enable MySQL',
                        'systemd': {
                            'name': 'mysqld',
                            'state': 'started',
                            'enabled': True
                        }
                    },
                    {
                        'name': 'Set MySQL root password',
                        'mysql_user': {
                            'name': 'root',
                            'password': '{{ mysql_root_password }}',
                            'login_unix_socket': '/var/lib/mysql/mysql.sock'
                        }
                    },
                    {
                        'name': 'Create application database',
                        'mysql_db': {
                            'name': '{{ app_db_name }}',
                            'state': 'present',
                            'login_user': 'root',
                            'login_password': '{{ mysql_root_password }}'
                        }
                    },
                    {
                        'name': 'Create application database user',
                        'mysql_user': {
                            'name': '{{ app_db_user }}',
                            'password': '{{ app_db_password }}',
                            'priv': '{{ app_db_name }}.*:ALL',
                            'state': 'present',
                            'login_user': 'root',
                            'login_password': '{{ mysql_root_password }}'
                        }
                    },
                    {
                        'name': 'Configure MySQL firewall',
                        'firewalld': {
                            'port': '3306/tcp',
                            'permanent': True,
                            'state': 'enabled',
                            'immediate': True,
                            'source': '10.0.0.0/16'  # Only allow from VPC
                        }
                    }
                ]
            }
        ]
        
        playbook_file = self.playbook_dir / 'databases.yml'
        with open(playbook_file, 'w') as f:
            yaml.dump(playbook, f, default_flow_style=False)
        
        print(f"Created database playbook: {playbook_file}")
        return playbook_file
    
    def create_monitoring_playbook(self):
        """Create playbook for monitoring setup"""
        playbook = [
            {
                'name': 'Configure Monitoring',
                'hosts': 'all',
                'become': True,
                'tasks': [
                    {
                        'name': 'Install monitoring packages',
                        'yum': {
                            'name': ['htop', 'iotop', 'nethogs', 'sysstat'],
                            'state': 'present'
                        }
                    },
                    {
                        'name': 'Install CloudWatch agent',
                        'get_url': {
                            'url': 'https://s3.amazonaws.com/amazoncloudwatch-agent/amazon_linux/amd64/latest/amazon-cloudwatch-agent.rpm',
                            'dest': '/tmp/amazon-cloudwatch-agent.rpm'
                        }
                    },
                    {
                        'name': 'Install CloudWatch agent package',
                        'yum': {
                            'name': '/tmp/amazon-cloudwatch-agent.rpm',
                            'state': 'present'
                        }
                    },
                    {
                        'name': 'Configure CloudWatch agent',
                        'template': {
                            'src': 'cloudwatch-config.json.j2',
                            'dest': '/opt/aws/amazon-cloudwatch-agent/etc/amazon-cloudwatch-agent.json'
                        },
                        'notify': 'restart cloudwatch agent'
                    },
                    {
                        'name': 'Start CloudWatch agent',
                        'shell': '/opt/aws/amazon-cloudwatch-agent/bin/amazon-cloudwatch-agent-ctl -a fetch-config -m ec2 -c file:/opt/aws/amazon-cloudwatch-agent/etc/amazon-cloudwatch-agent.json -s'
                    }
                ],
                'handlers': [
                    {
                        'name': 'restart cloudwatch agent',
                        'shell': '/opt/aws/amazon-cloudwatch-agent/bin/amazon-cloudwatch-agent-ctl -a restart'
                    }
                ]
            }
        ]
        
        playbook_file = self.playbook_dir / 'monitoring.yml'
        with open(playbook_file, 'w') as f:
            yaml.dump(playbook, f, default_flow_style=False)
        
        # Create CloudWatch config template
        templates_dir = Path('templates')
        templates_dir.mkdir(exist_ok=True)
        
        cloudwatch_config = {
            "metrics": {
                "namespace": "CWAgent",
                "metrics_collected": {
                    "cpu": {
                        "measurement": ["cpu_usage_idle", "cpu_usage_iowait", "cpu_usage_user", "cpu_usage_system"],
                        "metrics_collection_interval": 60
                    },
                    "disk": {
                        "measurement": ["used_percent"],
                        "metrics_collection_interval": 60,
                        "resources": ["*"]
                    },
                    "diskio": {
                        "measurement": ["io_time"],
                        "metrics_collection_interval": 60,
                        "resources": ["*"]
                    },
                    "mem": {
                        "measurement": ["mem_used_percent"],
                        "metrics_collection_interval": 60
                    }
                }
            },
            "logs": {
                "logs_collected": {
                    "files": {
                        "collect_list": [
                            {
                                "file_path": "/var/log/httpd/access_log",
                                "log_group_name": "{{ app_name }}-access-logs",
                                "log_stream_name": "{instance_id}"
                            },
                            {
                                "file_path": "/var/log/httpd/error_log",
                                "log_group_name": "{{ app_name }}-error-logs",
                                "log_stream_name": "{instance_id}"
                            }
                        ]
                    }
                }
            }
        }
        
        with open(templates_dir / 'cloudwatch-config.json.j2', 'w') as f:
            json.dump(cloudwatch_config, f, indent=2)
        
        print(f"Created monitoring playbook: {playbook_file}")
        return playbook_file
    
    def run_playbook(self, playbook_file, extra_vars=None, tags=None, limit=None):
        """Run an Ansible playbook"""
        cmd = [
            'ansible-playbook',
            '-i', self.inventory_file,
            str(playbook_file)
        ]
        
        if extra_vars:
            cmd.extend(['--extra-vars', json.dumps(extra_vars)])
        
        if tags:
            cmd.extend(['--tags', tags])
        
        if limit:
            cmd.extend(['--limit', limit])
        
        try:
            print(f"Running playbook: {playbook_file}")
            result = subprocess.run(cmd, check=True, capture_output=True, text=True)
            print("Playbook executed successfully")
            print(result.stdout)
            return True
            
        except subprocess.CalledProcessError as e:
            print(f"Playbook execution failed: {e}")
            print(f"Error output: {e.stderr}")
            return False
    
    def check_connectivity(self):
        """Check connectivity to all hosts"""
        try:
            cmd = ['ansible', 'all', '-i', self.inventory_file, '-m', 'ping']
            result = subprocess.run(cmd, check=True, capture_output=True, text=True)
            print("Connectivity check passed")
            print(result.stdout)
            return True
            
        except subprocess.CalledProcessError as e:
            print(f"Connectivity check failed: {e}")
            print(f"Error output: {e.stderr}")
            return False

# Usage example
def configure_infrastructure():
    """Configure infrastructure using Ansible"""
    # Define servers
    servers = [
        {
            'name': 'web-server-1',
            'ip_address': '10.0.1.10',
            'group': 'webservers'
        },
        {
            'name': 'web-server-2',
            'ip_address': '10.0.1.11',
            'group': 'webservers'
        },
        {
            'name': 'db-server-1',
            'ip_address': '10.0.2.10',
            'group': 'databases'
        }
    ]
    
    # Initialize Ansible manager
    ansible = AnsibleManager()
    
    # Create inventory
    ansible.create_inventory(servers)
    
    # Check connectivity
    if not ansible.check_connectivity():
        print("Cannot connect to servers. Check SSH keys and network connectivity.")
        return
    
    # Create playbooks
    web_playbook = ansible.create_web_server_playbook()
    db_playbook = ansible.create_database_playbook()
    monitoring_playbook = ansible.create_monitoring_playbook()
    
    # Run playbooks
    print("Configuring web servers...")
    ansible.run_playbook(web_playbook)
    
    print("Configuring database servers...")
    # Note: In production, use Ansible Vault for passwords
    extra_vars = {
        'vault_mysql_root_password': 'secure_root_password',
        'vault_app_db_password': 'secure_app_password'
    }
    ansible.run_playbook(db_playbook, extra_vars=extra_vars)
    
    print("Setting up monitoring...")
    ansible.run_playbook(monitoring_playbook)
    
    print("Infrastructure configuration completed!")

if __name__ == "__main__":
    # Uncomment to run configuration
    # configure_infrastructure()
    pass
```

This covers the Infrastructure as Code section of the DevOps specialization. The content focuses on practical, production-ready examples that show how to manage real infrastructure using Python and popular DevOps tools. Each example includes proper error handling, logging, and follows best practices for infrastructure automation.

<function_calls>
<invoke name="taskStatus">
<parameter name="status">completed