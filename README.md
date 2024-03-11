# EC2-Automated-Start-and-Stop
#### Desc..........

## Architectural Diagram


## Step 1: Create EC2 instance

## Step 2: Create Lambda IAM Role
#### In the IAM role console, create a Lambda IAM role that will allow Lambda to manage EC2 instances nad provide proper permission. Select `AmazonEC2FullAccess` and `AWSLambdaBasicExecutionRole`.



## Step 3: Create "Start" Lambda Function
#### Create a "Start" EC2 Lambda Function. Attach the Lamda IAM role. Use Python code below. Modify `region` and `instance ID` as needed.

```
import boto3 
region = 'us-east-1' # Replace with your desired region
instances = ['213sdsfsfsdf'] # Replace with desired EC2 instance ID(s)
ec2 = boto3.client('ec2', region_name=region) 

def lambda_handler(event, context): 
    ec2.start_instances(InstanceIds=instances) 
    print('starting your instances: ' + str(instances)) 
```

## Step 4: Create "Stop" Lambda Function
##### Create a "Stop" EC2 Lambda Function. Attach the Lambda IAM role. Use Python code below. Modify `region` and `instance ID` as needed.

```
import boto3 
region = 'us-east-1' # Replace with your desired region
instances = ['3424432432'] # Replace with desired EC2 instance ID(s)
ec2 = boto3.client('ec2', region_name=region) 

def lambda_handler(event, context): 
    ec2.stop_instances(InstanceIds=instances) 
    print('stopped your instances: ' + str(instances))
```

## Step 5: Configure CloudWatch Events/EventBridge 
#####

## Step 6: 
#####


### Finish! Congratulations, you've setup and configured EC2 Automated Start and Stop using Lambda Functions and CloudWatch Events/EventBridge. 
## Notes
*

## Reference 
*
