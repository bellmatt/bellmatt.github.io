---
title: "Charset normaliser"
date: 2025-07-17
description: "Read text when you don't know the charset encoding"
tags: ["til", "python", "midata"]
---
[charset_normaliser](https://github.com/jawah/charset_normalizer) is a python package that
allows you to read from a file when you don't know what the encoding is going to be.

Not unrelated: The [midata](https://www.gov.uk/government/news/the-midata-vision-of-consumer-empowerment)
initiative allows consumers to download transactional data in a consistent CSV format. 
Santander chose to make their implementation a semicolon-separated csv file using the `cp1250` ([Windows-1250](https://en.wikipedia.org/wiki/Windows-1250)) encoding. 
Imagine the state of the systems involved in generating that file...
