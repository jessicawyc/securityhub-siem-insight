# Deployment
## Eventbridge Pattern 替换为:
```
{
  "source": ["aws.securityhub"],
  "detail-type": ["Security Hub Findings - Imported"],
  "detail": {
    "findings": {
      "ProductName": ["Macie"],
      "Severity": {
        "Label": ["HIGH","LOW","MEDIUM"]
      },
      "RecordState": ["ACTIVE"],
      "Workflow": {
        "Status": ["NEW"]
      }
    }
  }
}
```
## 配置两个Lambda环境变量
![如图](/s3/lambda环境变量.png)
```
arn1
```
```
arn:aws:securityhub:::insight/securityhub/default/10
```
```
arn2
```
```
arn:aws:securityhub:::insight/securityhub/default/12
```
## macie配置时要勾选
![macie](/s3/macie-sh-open.png)

## 生成SecurityHub Finding如下图

![sample](/s3/SIEM-Alert.png)
