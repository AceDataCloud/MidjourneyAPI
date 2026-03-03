# Midjourney Translate API Integration and Usage

The main function of the Midjourney Translate API is to obtain English descriptive terms by inputting Chinese descriptive terms content.

This document will provide detailed instructions on integrating the Midjourney Translate API, helping you easily integrate and fully utilize the powerful features of this API. With the Midjourney Translate API, you can easily convert Chinese descriptive terms content into English descriptive terms content.

## Application Process

To use the Midjourney Translate API, you need to first apply for the corresponding service on the application page [Midjourney Translate API](https://platform.acedata.cloud/documents/e067d19b-7a66-4458-a45f-0fe88c1d5d34). After entering the page, click the "Acquire" button, as shown in the image:

![Application Page](https://cdn.acedata.cloud/rci31i.png)

If you are not logged in or registered, you will be automatically redirected to the [login page](https://platform.acedata.cloud) inviting you to register and log in. After logging in or registering, you will be automatically returned to the current page.

There is a free quota available for first-time applicants, allowing you to use the API for free.

## Request Example

We will use a Chinese descriptive term as an example to demonstrate how to use the API. Suppose the Chinese descriptive term is: exquisite, flawless, pure white angel. Next, we will demonstrate how to upload the Chinese descriptive term and obtain the English descriptive term.

### Setting Request Headers and Request Body

**Request Headers** include:

- `accept`: Specifies that the response result should be in JSON format, set to `application/json`.
- `authorization`: The key to call the API, which can be selected directly after application.

**Request Body** includes:

- `content`: The uploaded Chinese descriptive term.

> Only this one field is needed to complete the translation, please ensure that `content` uses Chinese descriptive terms.

Set it as shown in the image below:

![](https://cdn.acedata.cloud/db8lrn.png)

### Code Example

You can see that various language codes have been automatically generated on the right side of the page, as shown in the image:

<p><img src="https://cdn.acedata.cloud/oj1cck.png" width="500" class="m-auto"></p>

Some code examples are as follows:

#### CURL

```bash
curl -X POST 'https://api.acedata.cloud/midjourney/translate' \
-H 'accept: application/json' \
-H 'authorization: Bearer {token}' \
-H 'content-type: application/json' \
-d '{
  "content": "精致，无暇，洁白的天使"
}'
```

#### Python

```python
import requests

url = "https://api.acedata.cloud/midjourney/translate"

headers = {
    "accept": "application/json",
    "authorization": "Bearer {token}",
    "content-type": "application/json"
}

payload = {
    "content": "精致，无暇，洁白的天使"
}

response = requests.post(url, json=payload, headers=headers)
print(response.text)
```

### Response Example

After a successful request, the API will return one descriptive piece of information translated from the Chinese descriptive term. For example:

```json
{
  "content": "Exquisite, flawless, pure white angel"
}
```

As you can see, the result contains a `content` field, which includes the translated English descriptive term, corresponding to the translation of the Chinese descriptive term.

- `content`: Generates the corresponding English descriptive term, which can be used for image generation tasks.

## Error Handling

When calling the API, if an error occurs, the API will return the corresponding error code and message. For example:

- `400 token_mismatched`: Bad request, possibly due to missing or invalid parameters.
- `400 api_not_implemented`: Bad request, possibly due to missing or invalid parameters.
- `401 invalid_token`: Unauthorized, invalid or missing authorization token.
- `429 too_many_requests`: Too many requests, you have exceeded the rate limit.
- `500 api_error`: Internal server error, something went wrong on the server.

### Error Response Example

```
{
  "success": false,
  "error": {
    "code": "api_error",
    "message": "fetch failed"
  },
  "trace_id": "2cf86e86-22a4-46e1-ac2f-032c0f2a4e89"
}
```

## Conclusion

Through this document, you have learned how to use the Midjourney Translate API to translate uploaded Chinese descriptive terms into English descriptive terms. We hope this document helps you better integrate and use this API. If you have any questions, please feel free to contact our technical support team.