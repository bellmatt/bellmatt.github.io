---
layout: post
title: Vendor lock-in and OpenTelemetry
date: 2021-11-20 08:00:00 +0000
description: 
img: opentelemetry-horizontal-color.png
fig-caption: 
tags: [Tech, OpenTelemetry, Observability]
---

I've spent some time recently looking at OpenTelemetry and how it can help solve the issue of being locked in to specific Observability tooling and/or vendors. For those unaware, [OpenTelemetry](https://opentelemetry.io/) is an open-source collection of APIs and SDKs that can collect and export telemetry data from applications, with the goal of being able to instrument your code without being tied to the tooling you export that data to. For example, most vendors like Datadog, New Relic etc provide an SDK to instrument code with. The more instrumentation you add, the better value you will get from the tool, but it will become difficult to strip this out later if you decide to switch tooling. A lot of the time the compromise is to settle for light touch instrumentation or auto-instrumentation if its supported.

It is reasonable to assume then, that OpenTelemetry is a good fit to help with this fear of vendor lock-in. Want to switch from New Relic to Datadog, or host your own open-source tooling? In theory, it would just be a configuration change to export the data to the new tool. Let's ignore the fact that switching vendors and tooling is never a quick process in larger companies, but the engineering time saved would be attractive.

At the time of writing, OpenTelemetry is in beta - meaning there is likely to still be some issues preventing its use in production environments. I have been focussing on serverless instrumentation recently, where there are OpenTelemetry AWS Lambda Layers published to make it simple to add instrumentation to Lambda Functions. There have been a few issues which is to be expected - such as NodeJS lambda not starting at all, and some errors exporting Python traces in certain functions.

There's still a lot of work to do - as can be seen from the [OTEL status page](https://opentelemetry.io/status/), but it feels like a big step towards being able to instrument applications early on in development, without being concerned or coupled to any tooling that application telemetry may be exported to in future.
