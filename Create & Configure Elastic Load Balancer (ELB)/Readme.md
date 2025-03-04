## 1. Create & Configure a New ELB

**Code:**

```python
import boto3

# Initialize a Boto3 client for ELBv2
client = boto3.client('elbv2')

def create_application_load_balancer(name, subnets):
    response = client.create_load_balancer(
        Name=name,
        Subnets=subnets,
        Type='application'
    )
    return response

# Example usage
# Replace 'YOUR_SUBNET_1', 'YOUR_SUBNET_2' with your actual subnet IDs
lb_name = 'huzElb'
subnets_list = ['subnet-08a6fdf001add36a1','subnet-0d4867695ecccd146']
response = create_application_load_balancer(lb_name, subnets_list)
print(response['LoadBalancers'][0]['LoadBalancerArn'])

```
**Result of Code:**

![Code Execution](https://i.imgur.com/3fNrPUj.png)

## 2. Monitor and Analyze traffic on ELB

**Code:**

```python
mport boto3
from datetime import datetime, timedelta

# Initialize the CloudWatch client
cloudwatch_client = boto3.client('cloudwatch')

# Define the ELB name
elb_name = 'flaskElb'  # Replace with your ELB name


# Time range for the metrics
end_time = datetime.now()
start_time = end_time - timedelta(days=1)  # Metrics for the last 24 hours
# Define the metric name and dimensions
metric_name = 'RequestCount'  # Change to the desired metric
dimensions = [
    {
        'Name': 'LoadBalancerName',
        'Value': elb_name
    },
]

# Retrieve ELB metrics
response = cloudwatch_client.get_metric_data(
    MetricDataQueries=[
        {
            'Id': 'm1',
            'MetricStat': {
                'Metric': {
                    'Namespace': 'AWS/ELB',
                    'MetricName': metric_name,
                    'Dimensions': dimensions
                },
                'Period': 300,  # Adjust the period as needed (in seconds)
                'Stat': 'Sum',  # Change to other stats like 'Average', 'Maximum', etc.
            },
            'ReturnData': True,
        },
    ],


     StartTime=start_time,
     EndTime=end_time,    
)

# Print the metric data
print("Metric Data:", response['MetricDataResults'])
for datapoint in response.get('Datapoints', []):
    print(f"Time: {datapoint['Timestamp']}, Request Count: {datapoint[statistic]}")
```
**Result of Code:**

![Code Execution](https://i.imgur.com/BkOk5CU.png)
