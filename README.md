# Securityhub-siem-insight
使用securityhub的insight,对威胁特定的场景,自动生成一条Critical告警.
## 架构 Architecture
SecurityHub可以通过两种方式成为SIEM,左侧第一种请详见Global Security Blog本文提供右侧方式的多个场景.

There are two ways to correlate finding in securityhub as below, the left side using dynamoDB, you may refer below blog. In this blog, I will show you the architecure in the right side,by using insights.

https://aws.amazon.com/cn/blogs/security/correlate-security-findings-with-aws-security-hub-and-amazon-eventbridge/
![arch](/SIEM-2-Architecture.png)

## 场景 User Cases
### IAM Attack
### EC2:Critical vulnerability & External attack
### Prerequisites Enable SecurityHub with Other ESS service 打开相关服务
Please see详见: https://github.com/jessicawyc/aws-enable-ess

## Deployment for User case :S3:[Public access & Sensitive data](/s3/Readme.md) 部署第一个user case


### Step 1 Design insights
#### S3:Public access & Sensitive data
For this user case, we  will use aws managed insights:

  "2. S3 buckets with public write or read permissions"

  "10. S3 buckets with sensitive data"

You may find the arn in the offical document and give the arn to parameter in step 2
https://docs.aws.amazon.com/securityhub/latest/userguide/securityhub-managed-insights.html



### Step 2 Deploy by Cloudformation Template
Use CLI to create a cloudformation stack or you may use console to do this see detail steps in https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/cfn-console-login.html.

参数设置
```
stackname='securityhub-siem-2'
templatename='Sechub-2insight-template.yaml'
region='us-east-1'
arn1='arn:aws:securityhub:::insight/securityhub/default/10'
arn2='arn:aws:securityhub:::insight/securityhub/default/12'
findingtype='Software and Configuration Checks/Amazon Security Best Practices'
title='SIEM Alert-Publicly shared S3 with sensitive data'
resourcetype='AwsS3Bucket'
```
运行CLI命令

```
aws cloudformation create-stack --stack-name $stackname --template-body file://$templatename \
--parameters  \
ParameterKey=arn1,ParameterValue=$arn1  \
ParameterKey=arn2,ParameterValue=$arn2  \
ParameterKey=findingtype,ParameterValue=$findingtype  \
ParameterKey=title,ParameterValue=$title  \
ParameterKey=resourcetype,ParameterValue=$resourcetype  \
--capabilities CAPABILITY_IAM \
--region=$region
```
Test result in securityhub
![snapshot](s3/SIEM-Alert.png)

## Deployment for User case 2 :IAM attack 部署第二个user case

### Step 1 Design insights

For this user case, we  will use 3 cutome insights, you may follow the guide https://docs.aws.amazon.com/securityhub/latest/userguide/securityhub-custom-insights.html#:~:text=include%20both%20resources.-,Creating%20a%20custom%20insight%20(console),-From%20the%20console to create in console,
or use CLI

use below CLI to create and get the arn of three custom insights:
参数设置 Set Paramter
```
region='eu-west-2'
insight1='usercase2-1-InitialAccess'
insight2='usercase2-2-Persistence'
insight3='usecase2-3-PrivilegeEscalation'
```
参数设置 Set Paramter
```
stackname='securityhub-siem-2'
templatename='Sechub-2insight-template.yaml'
region='us-east-1'

```
运行CLI命令 Run CLI Command

```
aws cloudformation create-stack --stack-name $stackname --template-body file://$templatename \
--parameters  \
ParameterKey=arn1,ParameterValue=$arn1  \
ParameterKey=arn2,ParameterValue=$arn2  \
--capabilities CAPABILITY_IAM \
--region=$region
```





### Step 2 Deploy by Cloudformation Template
Use CLI to create a cloudformation stack or you may use console to do this see detail steps in https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/cfn-console-login.html.

参数设置 Set Paramter
```
stackname='securityhub-siem-2'
templatename='Sechub-2insight-template.yaml'
region='us-east-1'
arn1='arn:aws:securityhub:::insight/securityhub/default/10'
arn2='arn:aws:securityhub:::insight/securityhub/default/12'
```
运行CLI命令 Run CLI Command

```
aws cloudformation create-stack --stack-name $stackname --template-body file://$templatename \
--parameters  \
ParameterKey=arn1,ParameterValue=$arn1  \
ParameterKey=arn2,ParameterValue=$arn2  \
--capabilities CAPABILITY_IAM \
--region=$region
```
Test result in securityhub
![snapshot](s3/SIEM-Alert.png)
