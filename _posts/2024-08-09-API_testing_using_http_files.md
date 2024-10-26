---
title: API testing using .http files  
description: write simple, maintanable and developer friendly automated API tests using .http files  
date: 2024-08-09 10:10:10 +1000
categories: [Testing]
tags: [sample]     # TAG names should always be lowercase
toc: true
---
  
## Why http files

Looking at options to write API tests, there were 2 options considered

  1. Using in memory test server
  2. Using postman

Both have some downsides,  
Test server requires code, more code means more maintanance and complexity increases as project matures.  
Postman produces json files not easy to review through PR process.  

Then came across .http files and their support for writing automated tests.  

Advatntages are no code required, ease of code review, and no dependency on actual platform i.e. if server is moved from dotnet to java or something else tests are still valid  

> one thing to consider is dependency on Intellij based editor,
license is not required for running scripts/tests, but support in editor is a premium feature.  
{: .prompt-info }

## Tools required
  
1. Rider or IntelliJ editor, it has intellisense support
2. Docker for easy automation

## Writing API tests

The format is easy, here is a sample

```http
### GET request to fetch user data
GET http://helloworld.com/users/1
Accept: application/json

### POST request to create a new user
POST https://helloworld.com/users
Content-Type: application/json

{
  "name": "Hello World",
  "username": "helloworld",
  "email": "hello@world.com"
}
```

This format is supported across editors, like vscode and recently visual studio.  

In Intellij based editors, there are few additional features the main one is ability to assert responses using a javascript api.  
