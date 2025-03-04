## 1. Generate CloudWatch Metrics:

**Code:**

```python

import boto3
import datetime


# Initialize the CloudWatch client
cloudwatch = boto3.client('cloudwatch')

# Define the metric you want to retrieve
metric_name = 'CPUUtilization'
namespace = 'AWS/EC2'
instance_id = ''

# Calculate the start and end times
end_time = datetime.datetime.utcnow()
start_time = end_time - datetime.timedelta(hours=1)  # Adjust the time window as needed

# Fetch metric data
response = cloudwatch.get_metric_data(
    MetricDataQueries=[
        {
            'Id': 'm1',
            'MetricStat': {
                'Metric': {
                    'Namespace': namespace,
                    'MetricName': metric_name,
                    'Dimensions': [
                        {
                            'Name': 'InstanceId',
                            'Value': instance_id
                        },
                    ]
                },
                'Period': 300,  # Adjust the period as needed
                'Stat': 'Average',  # You can choose the statistic you want
            },
            'ReturnData': True,
        },
    ],
    StartTime=start_time.strftime('%Y-%m-%dT%H:%M:%SZ'),  # Convert to the required format
    EndTime=end_time.strftime('%Y-%m-%dT%H:%M:%SZ'),  # Convert to the required format
)

# Extract the metric values
metric_data = response['MetricDataResults'][0]['Values']

Print the metric data values
print("Metric Data Values:")
for value in metric_data:
    print(value)
```
**Result of Code:**

![Code Execution](https://i.imgur.com/LZWi3pJ.png)

## 2. Creation of CloudWatch Dashboard:

**Code:**

```python

import boto3
import datetime


# Initialize the CloudWatch client
cloudwatch = boto3.client('cloudwatch')

# Define the metric you want to retrieve
metric_name = 'CPUUtilization'
namespace = 'AWS/EC2'
instance_id = ''

# Calculate the start and end times
end_time = datetime.datetime.utcnow()
start_time = end_time - datetime.timedelta(hours=1)  # Adjust the time window as needed

# Fetch metric data
response = cloudwatch.get_metric_data(
    MetricDataQueries=[
        {
            'Id': 'm1',
            'MetricStat': {
                'Metric': {
                    'Namespace': namespace,
                    'MetricName': metric_name,
                    'Dimensions': [
                        {
                            'Name': 'InstanceId',
                            'Value': instance_id
                        },
                    ]
                },
                'Period': 300,  # Adjust the period as needed
                'Stat': 'Average',  # You can choose the statistic you want
            },
            'ReturnData': True,
        },
    ],
    StartTime=start_time.strftime('%Y-%m-%dT%H:%M:%SZ'),  # Convert to the required format
    EndTime=end_time.strftime('%Y-%m-%dT%H:%M:%SZ'),  # Convert to the required format
)

# Extract the metric values
metric_data = response['MetricDataResults'][0]['Values']

#Print the metric data values
#print("Metric Data Values:")
#for value in metric_data:
#    print(value)

import json
# Define the dashboard name and widgets
dashboard_name = 'MyDashboard'
widgets = [
    {
        'type': 'metric',
        'x': 0,
        'y': 0,
        'width': 12,
        'height': 6,
        'properties': {
            'metrics': [
                [namespace, metric_name, 'InstanceId', instance_id],
            ],
            'period': 300,
            'stat': 'Average',
            'region': 'us-east-2',  # Specify your AWS region
        },
    },
]

# Create the dashboard
cloudwatch.put_dashboard(
    DashboardName=dashboard_name,
    DashboardBody=json.dumps({'widgets': widgets}),
)
```
**Result of Code:**

![Code Execution](https://i.imgur.com/BxIZWH5.png)


