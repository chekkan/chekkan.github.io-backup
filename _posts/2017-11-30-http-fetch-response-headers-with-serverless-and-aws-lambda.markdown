---
layout: post
title: Access response headers in HTTP Fetch API with Serverless Framework and AWS
  Lambda
date: '2017-11-30 00:23:57'
permalink: /http-fetch-response-headers-with-serverless-and-aws-lambda/
tags:
- aws
- serverless-framework
- lambda
- http
- node-js
---

In order to access response headers such as `Location` in HTTP Fetch api whilst using Serverless Framework and AWS Lambda Functions with CORS enabled, you need to do the following. 

1. Make sure `cors` is set to `true` on `serverless.yml`
1. Make sure in the response header, you are returning the following:
    ```
    callback(null, {
        statusCode: 201,
        headers: {
            'Access-Control-Allow-Origin': '*',
            // Required for cookies, authorization headers with HTTPS
            'access-control-allow-credentials': true,
            'access-control-allow-headers': 'Location',
            'access-control-expose-headers': 'Location',
            Location: id
        }
    })
    ```

Now, you can access the header `location` from `fetch`.
```
this.httpClient.fetch("/users", {
    method: "post",
    body: json({ username: "chekkan" }),
})
.then((res) => {
    return res.headers.get("location")
})
```

**References:**
- [Serverless.yml CORS](https://serverless.com/framework/docs/providers/aws/events/apigateway#enabling-cors)
- [Access-Control-Allow-Headers - Mozilla Developer](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Access-Control-Allow-Headers)

*Photo by [Paul Buffington](https://unsplash.com/photos/Lwe2hbm5XKk?utmsource=unsplash&utmmedium=referral&utmcontent=creditCopyText) on [Unsplash](https://unsplash.com/?utmsource=unsplash&utmmedium=referral&utmcontent=creditCopyText)*