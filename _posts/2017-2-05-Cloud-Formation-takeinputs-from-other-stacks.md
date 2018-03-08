---
layout: post
title: Ref resources that are created part of other cloudformation stacks!
---

In this post I am going to show how to refer resources that are created part of other cloudformation stacks. I named the below stack as BuildLambda

In your source cloudformation template export the resource you want to ref in other templates
```javascript
  "Outputs": {
    "LambdaARN": {
      "Description": "Return ARN of a SQS queue that you want to refer",
      "Value": { "Ref": "MessageLambda" },
      "Export": { "Name": { "Fn::Sub": "${AWS::StackName}-Arn" } }
    }
  }
```
Now, you can refer the exported value on other cloudformation templates.

```javascript
  "Fn::Sub": [{"\"arn:aws:lambda:${AWS::Region}:${AWS::AccountId}:function:${ValueFromBuildLambdaStack}\""
        "ValueFromBuildLambdaStack": {
        "Fn::ImportValue": {
            "Fn::Sub": "${BuildLambdaStack}-Arn"
          }
        }
    }]
```

You can refer the following aws documentation for more information.

1. Fn:Sub https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/intrinsic-function-reference-sub.html
2. Fn::ImportValue https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/intrinsic-function-reference-importvalue.html
