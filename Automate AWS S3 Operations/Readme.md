## Create S3 Bucket & Upload/Download Files

This project provides a script to create S3 Bucket using the AWS SDK for Python (Boto3).

**Code:**

```python

import boto3

#s3 client
s3 = boto3.client('s3')


bucket_name = 'my_bucket'

# Create the S3 bucket
s3.create_bucket(Bucket=bucket_name)


# Replace with your specific local file, bucket name, and object key
local_file_path = my_file.txt
bucket_name = bucket_name
object_key = file2.txt

# Upload the local file to the S3 bucket with the specified object key
s3.upload_file(local_file_path, bucket_name, object_key)

#downloading files
s3.download_file('your_bucket_name', 'remote_file.txt', 'local_file.txt')


```
**Result of Code:**

![Code Execution](https://i.imgur.com/PyO07FA.png)
![Code Execution](https://i.imgur.com/PyO07FA.png)

