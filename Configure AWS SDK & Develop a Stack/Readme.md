## Install, configure AWS CDK & Develop a stack 


**Code:**

```python
#first install nodejs
sudo apt update
sudo apt upgrade
sudo apt install -y curl

curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash -

sudo apt install -y nodejs


#install and configure cdk

npm install -g aws-cdk


pip install virtualenv
virtualenv venv
source venv/bin/activate
 

cdk init app --language=python


pip install aws-cdk.aws-s3 aws-cdk.aws-lambda

from aws_cdk import core
from aws_cdk import aws_s3 as s3
from aws_cdk import aws_lambda as _lambda

class MyCdkAppStack(core.Stack):

    def __init__(self, scope: core.Construct, id: str, **kwargs) -> None:
        super().__init__(scope, id, **kwargs)

        # Create an S3 bucket
        bucket = s3.Bucket(self,
            "MyFirstBucket",
            versioned=True,
            removal_policy=core.RemovalPolicy.DESTROY
        )

        # Create a Lambda function
        lambda_function = _lambda.Function(self, "MyLambdaFunction",
            runtime=_lambda.Runtime.PYTHON_3_8,
            handler="index.handler",
            code=_lambda.Code.from_asset("lambda")

        )

        # Grant the Lambda function read/write permissions to the bucket
        bucket.grant_read_write(lambda_function)


#index.py

def handler(event, context):
    print("Lambda function triggered!")
    return {
        'statusCode': 200,
        'body': 'Hello from Lambda!'
    }

```
**Result of Code:**

![CPU Utilization](https://i.imgur.com/mjJgcBv.png)
![CPU Utilization](https://i.imgur.com/DwRg7ms.png)
![CPU Utilization](https://i.imgur.com/cdfWJ0s.png)
![CPU Utilization](https://i.imgur.com/AuCDUaP.png)

