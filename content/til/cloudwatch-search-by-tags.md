---
title: "AWS CloudWatch search logs by tags"
date: 2026-05-10
description: "AWS CloudWatch Logs can now be search by tags"
tags: ["til", "aws", "cloudwatch", "logs", "observability"]
---
TIL that CloudWatch Logs now supports querying by tags:
https://aws.amazon.com/about-aws/whats-new/2026/05/amazon-cloudwatch-logs-query-by-tags/

This is useful for humans in accounts with a lot of log groups, but for AI-driven SRE workflows
this should reduce the need for agents to have “inventory awareness” of logs (which is often messy/brittle)
and make it possible to describe the slice of the system you care about - using
intent-like attributes such as `env=prod`, `service=payments`, or `tier=backend`.
