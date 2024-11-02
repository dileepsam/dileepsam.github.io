---
title: API testing using .http files  
description: Write simple, maintainable, and developer-friendly automated API tests using .http files  
date: 2024-08-09 10:10:10 +1000
categories: [Testing]
tags: [sample]     # TAG names should always be lowercase
toc: true
---
  
## Why .http Files?

When considering options to write API tests, two options were considered:

1. Using an in-memory test server
2. Using Postman

Both have some downsides.  
A test server requires code, and more code means more maintenance and increased complexity as the project matures.  
Postman produces JSON files that are not easy to review through the PR process.  

Then, we came across .http files and their support for writing automated tests.  

Advantages include no code required, ease of code review, and no dependency on the actual platform. For example, if the server is moved from .NET to Java or something else, the tests are still valid.  

> One thing to consider is the dependency on an IntelliJ-based editor.
A license is not required for running scripts/tests, but support in the editor is a premium feature.  
{: .prompt-info }

## Tools Required
  
1. Rider or IntelliJ editor, which has IntelliSense support
2. Docker for easy automation

## Writing API Tests

The format is easy. Here is a sample:

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

This format is supported across editors like VSCode and recently Visual Studio.  

In IntelliJ-based editors, there are a few additional features. The main one is the ability to assert responses using a JavaScript API.  

### Detailed Examples

Here are some detailed examples of writing API tests using `.http` files:

#### Example 1: GET Request with Assertions

```http
### GET request to fetch user data
GET http://helloworld.com/users/1
Accept: application/json

> {%
client.test("Request executed successfully", function() {
    client.assert(response.status === 200, "Response status is not 200");
    client.assert(response.body.id === 1, "User ID is not 1");
});
%}
```

#### Example 2: POST Request with Assertions

```http
### POST request to create a new user
POST https://helloworld.com/users
Content-Type: application/json

{
  "name": "Hello World",
  "username": "helloworld",
  "email": "hello@world.com"
}

> {%
client.test("User created successfully", function() {
    client.assert(response.status === 201, "Response status is not 201");
    client.assert(response.body.username === "helloworld", "Username is not correct");
});
%}
```

## Advanced Features

### Using Variables

You can use variables in your `.http` files to make your tests more dynamic. For example:

```http
@baseUrl = http://helloworld.com

### GET request using a variable
GET {{baseUrl}}/users/1
Accept: application/json
```

### Chaining Requests

You can chain multiple requests together and use the response of one request in another. For example:

```http
### POST request to create a new user
POST https://helloworld.com/users
Content-Type: application/json

{
  "name": "Hello World",
  "username": "helloworld",
  "email": "hello@world.com"
}

> {%
client.global.set("userId", response.body.id);
%}

### GET request to fetch the newly created user
GET https://helloworld.com/users/{{userId}}
Accept: application/json
```

## Best Practices

### Organize Your Tests

Organize your tests in a way that makes them easy to read and maintain. Use comments and separate different types of requests.

### Use Variables

Use variables to avoid hardcoding values and make your tests more flexible.

### Assert Responses

Always assert the responses to ensure that your API is behaving as expected.

## Troubleshooting Tips

### Common Issues

1. **Invalid URL**: Ensure that the URL is correct and the server is running.
2. **Authentication Errors**: Make sure to include the necessary authentication headers.
3. **Incorrect Assertions**: Double-check your assertions to ensure they are correct.

### Solutions

1. **Check Server Logs**: Check the server logs for any errors or issues.
2. **Debugging**: Use the debugging features in your editor to step through the requests and responses.

