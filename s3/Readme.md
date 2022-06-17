# Deployment
Eventbridge Pattern 替换为:
```
{
  "source": ["aws.securityhub"],
  "detail-type": ["Security Hub Findings - Imported"],
  "detail": {
    "findings": {
      "ProductName": ["Macie"],
      "RecordState": ["ACTIVE"],
      "Workflow": {
        "Status": ["NEW"]
      }
    }
  }
}
```
macie配置时要勾选
![macie](/s3/macie-sh-open.png)
生成SecurityHub Finding如下图

![sample](/s3/SIEM-Alert.png)
