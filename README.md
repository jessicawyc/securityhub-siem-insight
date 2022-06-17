# securityhub-siem-insight
使用securityhub的insight,对威胁特定的场景,自动生成一条Critical告警.
## 架构 Architecture
SecurityHub可以通过两种方式成为SIEM,左侧第一种请详见Global Security Blog
https://aws.amazon.com/cn/blogs/security/correlate-security-findings-with-aws-security-hub-and-amazon-eventbridge/
![arch](/SIEM-2-Architecture.png)
本文提供右侧方式的多个场景
## 场景
### IAM攻击
### EC2:Critical vulnerability & External attack
### S3:[Public access & Sensitive data](/s3/Readme.md)

## Deployment部署
### Step1 Enable SecurityHub with Other ESS service 打开相关服务
Please see详见: https://github.com/jessicawyc/aws-enable-ess

### Step2 配置EventBridge
请参见,根据不同场景,再修改Event Pattern
https://github.com/jessicawyc/securityhub-alert#2%E8%87%AA%E5%8A%A8%E5%8F%91%E9%80%81%E5%91%8A%E8%AD%A6%E6%A8%A1%E5%BC%8F

### Step3 部署Lambda
#### 配置Lambda要使用的IAM Role
需要下载两个policy文件到本地
lambdapolicy.json和trust-lambda.json
因不同SIEM场景各异,可到相应文件夹中下载
参数设置
```
lambdapolicy='lambda-name'
rolename='lambda-siem-name'
```
运行CLI命令

```
rolearn=$(aws iam create-role --role-name $rolename --assume-role-policy-document file://trust-lambda.json --query 'Role.Arn' --output text)
aws iam put-role-policy --role-name=$rolename --policy-name $lambdapolicy --policy-document file://lambdapolicy.json
```

#### Create Lambda function
参数设置
```
function='lambda-name'
```
运行CLI命令
```
lambdaarn=$(aws lambda create-function \
    --function-name $function \
    --runtime python3.9 \
    --zip-file fileb://index.zip \
    --handler index.lambda_handler \
    --role $rolearn --region=$region --no-cli-pager --query 'FunctionArn' --output text)
```
