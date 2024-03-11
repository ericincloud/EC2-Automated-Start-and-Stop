# EC2-Automated-Start-and-Stop
#### EC2 Automated Start & Stop automates the starting and stopping of EC2 instances according a schedule. 
#### By automating this process, developers can start up instances according to a schedule without manual intervention. Instances can also be stopped automatically - effectively saving time, money, and resources by preventing an active service like EC2 from running at all times. 

## Architectural Diagram

![EC2StartStopArch](https://github.com/ericincloud/EC2-Automated-Start-and-Stop/assets/144301872/7a0cfda5-d067-4949-a04b-966c4b111a80)

## Step 1: Create an EC2 instance

![creatEC2SS](https://github.com/ericincloud/EC2-Automated-Start-and-Stop/assets/144301872/b094fae4-928a-4ec8-a5b4-2bfa5466bda3)

## Step 2: Create Lambda IAM Role
#### In the IAM role console, create a Lambda IAM role that will allow Lambda to manage EC2 instances and provide proper permission. Select `AmazonEC2FullAccess` and `AWSLambdaBasicExecutionRole`.

![LambdaSS](https://github.com/ericincloud/EC2-Automated-Start-and-Stop/assets/144301872/69fdba26-6796-4e00-ae72-2d6e86738516)

## Step 3: Create "Start" Lambda Function
#### Create a "Start" EC2 Lambda Function. Attach the Lamda IAM role. Use Python code below. Modify `region` and `instance ID` as needed.

```
import boto3 
region = 'us-east-1' # Replace with your desired region
instances = ['i-213sdsfsfsdf'] # Replace with desired EC2 instance ID(s)
ec2 = boto3.client('ec2', region_name=region) 

def lambda_handler(event, context): 
    ec2.start_instances(InstanceIds=instances) 
    print('starting your instances: ' + str(instances)) 
```
![EC2LambdaStart](https://github.com/ericincloud/EC2-Automated-Start-and-Stop/assets/144301872/b38f172f-d7fd-437b-b589-d7d40a7d06d5)

## Step 4: Create "Stop" Lambda Function
##### Create a "Stop" EC2 Lambda Function. Attach the Lambda IAM role. Use Python code below. Modify `region` and `instance ID` as needed.

```
import boto3 
region = 'us-east-1' # Replace with your desired region
instances = ['i-3424432432'] # Replace with desired EC2 instance ID(s)
ec2 = boto3.client('ec2', region_name=region) 

def lambda_handler(event, context): 
    ec2.stop_instances(InstanceIds=instances) 
    print('stopped your instances: ' + str(instances))
```

![EC2LambdaStop](https://github.com/ericincloud/EC2-Automated-Start-and-Stop/assets/144301872/b3cb6553-1b24-4d66-b4af-b487b897b5f7)

## Step 5: Configure CloudWatch Events/EventBridge Schedule

#### EXAMPLE CRON EXPRESSION:

```
37 - Minute (at 37th minute)
15 - Hour (at 3 PM)
11 - Day of the month (11th)
3 - Month (March)
* - Day of the week (any day)

? - any (for day of month)
* - any (other)
```

### Stop Instance Schedule:
#### Head to CloudWatch and start creating a "Schedule" > Select `Recurring Schedule` > Under `Cron-based schedule` create desired schedule to stop instance > Select `Off` under `Flexible time schedule` allowing the process to take place immediately > Next > Select `Invoke Lambda` > Select the "EC2Stop" Lambda function 

![CWStop](https://github.com/ericincloud/EC2-Automated-Start-and-Stop/assets/144301872/a5722481-6e70-494a-98ae-bb31bded5b74)

### Start Instance Schedule:
#### Create another "Schedule" > Select `Recurring Schedule` > Under `Cron-based schedule` create desired schedule to stop instance > Select `Off` under `Flexible time schedule` allowing the process to take place immediately > Next > Select `Invoke Lambda` > Select the "EC2Start" Lambda function 

![CWStart](https://github.com/ericincloud/EC2-Automated-Start-and-Stop/assets/144301872/30761138-8fef-4830-98a7-28eb38906a7f)

### Finish! 

### Congratulations! You've configured EC2 Automated Start and Stop using Lambda Functions and CloudWatch Events/EventBridge. Instance(s) will start up or stop according to schedule! 

## Notes
* Make sure you have the correct Cron expression arrangement/configuration.
* Make sure to disable schedules when no longer in use.

## Reference 
* EXAMPLE CRON EXPRESSION:

```
37 - Minute (at 37th minute)
15 - Hour (at 3 PM)
11 - Day of the month (11th)
3 - Month (March)
* - Day of the week (any day)

? - any (for day of month)
* - any (other)
```
* "Stop" EC2 Lambda Python Function

```
import boto3 
region = 'us-east-1' # Replace with your desired region
instances = ['i-3424432432'] # Replace with desired EC2 instance ID(s)
ec2 = boto3.client('ec2', region_name=region) 

def lambda_handler(event, context): 
    ec2.stop_instances(InstanceIds=instances) 
    print('stopped your instances: ' + str(instances))
```
* "Start" EC2 Lambda Python Function

```
import boto3 
region = 'us-east-1' # Replace with your desired region
instances = ['i-213sdsfsfsdf'] # Replace with desired EC2 instance ID(s)
ec2 = boto3.client('ec2', region_name=region) 

def lambda_handler(event, context): 
    ec2.start_instances(InstanceIds=instances) 
    print('starting your instances: ' + str(instances)) 
```

