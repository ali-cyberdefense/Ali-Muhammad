## 1. Create RDS Instance

This project provides a script to create RDS Instance using the AWS SDK for Python (Boto3).

**Code:**

```python

import boto3

# Initialize the RDS client
rds_client = boto3.client('rds')

# Define the parameters for your RDS instance
db_instance_name = 'rds'
db_instance_class = 'db.t2.micro'
engine = 'mysql'
master_username = 'Huzaifa_Tanveer'
master_password = 'iamtheone1122!'

# Create the RDS instance
response = rds_client.create_db_instance(
DBInstanceIdentifier=db_instance_name,
DBInstanceClass=db_instance_class,
Engine=engine,
MasterUsername=master_username,
MasterUserPassword=master_password,
AllocatedStorage=20,  # Specify the storage size in GB
MultiAZ=False,  # Set to True for Multi-AZ deployment
# Other parameters like VPC security groups, subnet groups, etc.
)

# Check the response for errors
if 'DBInstance' in response:
print(f"RDS instance '{db_instance_name}' created successfully.")
else:
print("Error creating RDS instance:", response)

```
**Result of Code:**

![CPU Utilization](https://i.imgur.com/KHvqalU.png)

## 2. Manual Backup of RDS Instance

**Code:**

```python
# Create a manual backup of the RDS instance
import boto3

# Initialize the RDS client
rds_client = boto3.client('rds')

backup_name = 'dbBackup'
db_instance_name = 'database'

response = rds_client.create_db_snapshot(
DBSnapshotIdentifier=backup_name,
DBInstanceIdentifier=db_instance_name
)

# Check the response for errors
if 'DBSnapshot' in response:
print(f"Manual backup '{backup_name}' created successfully.")
else:
print("Error creating manual backup:", response)

```
**Result of Code:**

![CPU Utilization](https://i.imgur.com/N5HOlSs.png)


