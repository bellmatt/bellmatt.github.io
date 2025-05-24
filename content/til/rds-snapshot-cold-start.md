---
title: "AWS RDS cold starts"
date: 2025-05-24
description: "AWS RDS can be very slow after restoring from a snaphot"
tags: ["til", "rds", "aws", "s3"]
---
After restoring an AWS RDS database from a snapshot, RDS only lazy loads data from S3 as and when it's required,
which for large tables can cause some initial slowness and quite a lot of I/O.

https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_RestoreFromSnapshot.html

> To help mitigate the effects of lazy loading on tables to which you require quick access, you can perform operations that involve full-table scans, such as SELECT *. This allows Amazon RDS to download all of the backed-up table data from S3.