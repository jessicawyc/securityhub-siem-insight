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

### Step1 Design insights
Take S3 usercases as example. we use aws managed insights.
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

### Step2 Deploy by Cloudformation Template

### Step3 Configuration Parameters


参数设置
```
stackname='securityhub-siem'
templatename='Arch1-template.yaml'
```
运行CLI命令

```
for region in $regions; do
aws cloudformation create-stack --stack-name $stackname --template-body file://$templatename \
--parameters  \
ParameterKey=level0,ParameterValue=public  \
ParameterKey=level1,ParameterValue=internal  \
ParameterKey=level2,ParameterValue=sensitive  \
--capabilities CAPABILITY_IAM \
--region=$region
echo $region
done

```


