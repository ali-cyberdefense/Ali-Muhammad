## 1. Starting & Stopping the EC2 Instance

This project provides a script to start and stop an EC2 instance using the AWS SDK for Python (Boto3).

**Code:**

```python
import boto3
import time

# Initialize the EC2 and CloudWatch clients
ec2 = boto3.client('ec2')
cw = boto3.client('cloudwatch')

# Define the instance ID of the EC2 instance to manage
instance_id = 'your_instance_id'

# Function to start the EC2 instance
def start_instance():
    ec2.start_instances(InstanceIds=[instance_id])
    print(f"Starting EC2 instance with ID: {instance_id}")

# Function to stop the EC2 instance
def stop_instance():
    ec2.stop_instances(InstanceIds=[instance_id])
    print(f"Stopping EC2 instance with ID: {instance_id}")
```
**Result of Code:**

![CPU Utilization](https://i.imgur.com/A3o6cs2.png)

## 2. Monitor the CPU Utilization

**Code:**

```python
# Function to get CPU utilization for the instance
def get_cpu_utilization():
    # Specify the metric and dimensions for the instance
    metric_name = 'CPUUtilization'
    namespace = 'AWS/EC2'
    period = 300  # 5 minutes
    dimensions = [{'Name': 'InstanceId', 'Value': instance_id}]

    # Get the metric data
    response = cw.get_metric_data(
        MetricDataQueries=[
            {
                'Id': 'm1',
                'MetricStat': {
                    'Metric': {
                        'Namespace': namespace,
                        'MetricName': metric_name,
                        'Dimensions': dimensions
                    },
                    'Period': period,
                    'Stat': 'Average'
                },
                'ReturnData': True,
            },
        ],
        StartTime=time.time() - 3600,  # 1 hour ago
        EndTime=time.time(),
    )

    if 'MetricDataResults' in response:
        datapoints = response['MetricDataResults'][0]['Values']
        if datapoints:
            print(f"Average CPU Utilization: {datapoints[-1]}%")
        else:
            print("No CPU utilization data available.")
    else:
        print("Unable to fetch CPU utilization data.")

# Main function
def main():
    while True:
        action = input("Enter 'start' to start the instance, 'stop' to stop it, 'monitor' to check CPU utilization, or 'exit' to quit: ")
        if action == 'start':
            start_instance()
        elif action == 'stop':
            stop_instance()
        elif action == 'monitor':
            get_cpu_utilization()
        elif action == 'exit':
            break
        else:
            print("Invalid action. Please enter 'start', 'stop', 'monitor', or 'exit'.")

if __name__ == "__main__":
    main()
```
**Result of Code:**

![CPU Utilization](https://i.imgur.com/7nCx6yQ.png)

