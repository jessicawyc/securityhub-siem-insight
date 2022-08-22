# securityhub-siem-insight
使用securityhub的insight,对威胁特定的场景,自动生成一条Critical告警.
## 架构 Architecture
SecurityHub可以通过两种方式成为SIEM,左侧第一种请详见Global Security Blog
https://aws.amazon.com/cn/blogs/security/correlate-security-findings-with-aws-security-hub-and-amazon-eventbridge/
![arch](/SIEM-2-Architecture.png)
本文提供右侧方式的多个场景
## 场景 User Cases
### IAM Attack
### EC2:Critical vulnerability & External attack
### S3:[Public access & Sensitive data](/s3/Readme.md)
To be continued

## Deployment部署
### Prerequisites Enable SecurityHub with Other ESS service 打开相关服务
Please see详见: https://github.com/jessicawyc/aws-enable-ess

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
```
运行CLI命令

```
aws cloudformation create-stack --stack-name $stackname --template-body file://$templatename \
--parameters  \
ParameterKey=arn1,ParameterValue=$arn1  \
ParameterKey=arn2,ParameterValue=$arn2  \
--capabilities CAPABILITY_IAM \
--region=$region
```
### Step3 Configuration Parameters

