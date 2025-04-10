---
title: "AWS CloudFormation Extensions"
date: 2025-04-10
description: "AWS CloudFormation extensions are a nice way of adding infrastructure resources from other vendors into your stack"
tags: ["til", "aws", "cloudformation", "datadog"]
---
Recently I learned AWS CloudFormation supports Extensions, 3rd party modules you can enable to add resources from other cloud providers to your CloudFormation stack. This is useful if you're not "all in" on AWS and have resources in other vendors that
are related to resources in your stack, such as Datadog Monitors or PagerDuty services.

https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/registry-public.html

The annoying thing is you can't configure CloudFormation extensions with IAC, only with the AWS CLI, meaning if you have a multi account setup it's more of a pain than it should be to ensure all relevant accounts have properly configured extensions.