---
layout: post
title: Adding IF condition to your cloudformation stack!
---

In this post I am going to show how to add IF condition in your cloudformation template. The idea is to have unified cloudformation template for ELB and EC2 based healthchecks.

```javascript
   "Conditions": {
      "UseELB": {
        "Fn::Not": [
            {
                "Fn::Equals": [
                    {
                        "Fn::Select": [
                            "0",
                            {
                                "Ref": "LoadBalancerName"
                            }
                        ]
                    },
                    ""
                ]
            }
        ]
      }
    }
  ....
  "HealthCheckType":  { 
            "Fn::If" : [
                  "UseELB",
                  "ELB",
                  "EC2"
            ]
        }
   ....
   "LoadBalancerNames": {
              "Fn::If" : [
                    "UseELB",
                    {"Ref" : "LoadBalancerName"},
                    {"Ref" : "AWS::NoValue"}
              ]
        }
```
Complete cloudformation template can be found <a href="https://github.com/sjey/scripts/blob/master/standardapp-cloudformation-stack.cform">here</a>. The template can also be used to deploy the given application to a bunch of ec2 instances through bamboo.  


