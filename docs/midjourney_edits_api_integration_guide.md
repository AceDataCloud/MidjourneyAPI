# Midjourney Edits API Integration Instructions

This article will introduce a Midjourney Edits API integration guide, which allows for editing incoming images through input prompts.

## Application Process

To use the API, you need to first apply for the corresponding service on the [Midjourney Edits API](https://platform.acedata.cloud/documents/midjourney-edits) page. After entering the page, click the "Acquire" button, as shown in the image below:

![](https://cdn.acedata.cloud/q6ytrc.png)

If you are not logged in or registered, you will be automatically redirected to the login page inviting you to register and log in. After logging in or registering, you will be automatically returned to the current page.

Upon your first application, there will be a free quota available for you to use the API for free.

## Basic Usage

First, understand the basic usage method, which involves inputting the prompt `prompt`, the action `action`, and the reference image `image_url` to obtain the processed result. You first need to simply pass a field `action` with the value `generate`, as detailed below:

<p><img src="https://cdn.acedata.cloud/m44i6n.png" width="500" class="m-auto"></p>

Here, we can see that we have set the Request Headers, including:

- `accept`: the format of the response result you want to receive, filled in as `application/json`, which means JSON format.
- `authorization`: the key to call the API, which can be selected directly after application.

Additionally, the Request Body is set, including:

- `mask`: can specify the mask position of the image area for editing and regeneration.
- `split_images`: splits the generated image into multiple images, returned through the sub_image_urls field. By default, it is false.
- `action`: the action for this image editing generation task, defaulting to `generate`.
- `image_url`: the link to the image that needs to be edited.
- `prompt`: the prompt.
- `mode`: generation mode, optional `fast`/`relax`/`turbo`.
- `callback_url`: the URL to receive the callback result.

After selection, you can see that the corresponding code is generated on the right side, as shown in the image below:

<p><img src="https://cdn.acedata.cloud/9wed3b.png" width="500" class="m-auto"></p>

Click the "Try" button to test, as shown in the above image, and we get the following result:

```json
{
  "sub_image_urls": [
    "https://cdn.midjourney.com/88e16dab-ef48-43a5-af73-bf24065287bc/0_0.png",
    "https://cdn.midjourney.com/88e16dab-ef48-43a5-af73-bf24065287bc/0_1.png",
    "https://cdn.midjourney.com/88e16dab-ef48-43a5-af73-bf24065287bc/0_2.png",
    "https://cdn.midjourney.com/88e16dab-ef48-43a5-af73-bf24065287bc/0_3.png"
  ],
  "image_url": "https://storage.fonedis.cc/attachments/1372468820912115716/1391371957878132849/cat_sitting_table_88e16dab-ef48-43a5-af73-bf24065287bc.png?ex=686ba79d&is=686a561d&hm=ad005d06f6673d6152456e04c3cbec39d062bd9df10448623fae27ddaf8b8a80&",
  "image_width": 960,
  "image_height": 1200,
  "raw_image_url": "https://storage.fonedis.cc/attachments/1372468820912115716/1391371957878132849/cat_sitting_table_88e16dab-ef48-43a5-af73-bf24065287bc.png?ex=686ba79d&is=686a561d&hm=ad005d06f6673d6152456e04c3cbec39d062bd9df10448623fae27ddaf8b8a80&",
  "raw_image_width": 960,
  "raw_image_height": 1200,
  "progress": 100,
  "image_id": "1391372193836826624",
  "task_id": "26c39859-f54a-4998-9e42-3da96eceee8c",
  "success": true
}
```

The returned result contains multiple fields, described as follows:

- `success`: the status of the image editing generation task.
- `task_id`: the ID of the image editing generation task.
- `image_id`: the image ID of this image editing task.
- `sub_image_urls`: multiple image results of the image generation task.
- `image_url`: the link to the generated image result.
- `image_width`: the width of the generated image result.
- `image_height`: the height of the generated image result.
- `progress`: the progress field of the image editing generation task.

We can see that we have obtained satisfactory image information, and we only need to retrieve the generated image using the image link address from `image_url`.

Additionally, if you want to generate the corresponding integration code, you can directly copy the generated code, for example, the CURL code is as follows:

```shell
curl -X POST 'https://api.acedata.cloud/midjourney/edits' \
-H 'accept: application/json' \
-H 'authorization: Bearer {token}' \
-H 'content-type: application/json' \
-d '{
  "prompt": "A cat sitting on a table",
  "split_images": true,
  "image_url": "https://cdn.acedata.cloud/jgo1cw.jpg",
  "action": "generate"
}'
```

## Asynchronous Callback

Since the time taken by the Midjourney Edits API to generate is relatively long, approximately 1-2 minutes, if the API does not respond for a long time, the HTTP request will keep the connection open, leading to additional system resource consumption. Therefore, this API also provides support for asynchronous callbacks.

The overall process is: when the client initiates a request, an additional `callback_url` field is specified. After the client initiates the API request, the API will immediately return a result containing a `task_id` field, representing the current task ID. When the task is completed, the generated video result will be sent to the client-specified `callback_url` in the form of a POST JSON, which also includes the `task_id` field, allowing the task result to be associated by ID.

Let’s understand how to operate specifically through an example.

First, the Webhook callback is a service that can receive HTTP requests, and developers should replace it with the URL of their own HTTP server. For demonstration purposes, we will use a public Webhook sample site https://webhook.site/. Open this site to obtain a Webhook URL, as shown in the image below:

![](https://cdn.acedata.cloud/hfrbzw.png)

Copy this URL, and it can be used as a Webhook. The sample here is `https://webhook.site/556e6971-b41f-4fa8-9151-6e91acd0399f`.

Next, we can set the field `callback_url` to the above Webhook URL while filling in the corresponding parameters, as shown in the image below:

<p><img src="https://cdn.acedata.cloud/q3fnhv.png" width="500" class="m-auto"></p>

Clicking run, we can see that an immediate result is obtained, as follows:

```
{
  "task_id": "b8b7fdc2-628e-40dd-bc0c-671c3ddac9e9"
}
```

After a moment, we can observe the generated video result at `https://webhook.site/556e6971-b41f-4fa8-9151-6e91acd0399f`, as shown in the image below:

<p><img src="https://cdn.acedata.cloud/t8cupr.png" width="500" class="m-auto"></p>

The content is as follows:
```json
{
    "sub_image_urls": [
        "https://cdn.midjourney.com/f3638ed2-60f6-49dd-897a-6d68c15afb17/0_0.png",
        "https://cdn.midjourney.com/f3638ed2-60f6-49dd-897a-6d68c15afb17/0_1.png",
        "https://cdn.midjourney.com/f3638ed2-60f6-49dd-897a-6d68c15afb17/0_2.png",
        "https://cdn.midjourney.com/f3638ed2-60f6-49dd-897a-6d68c15afb17/0_3.png"
    ],
    "image_url": "https://storage.fonedis.cc/attachments/1372468820912115716/1391374307719905340/cat_sitting_table_f3638ed2-60f6-49dd-897a-6d68c15afb17.png?ex=686ba9cd&is=686a584d&hm=71543c21c38db8a50c7ebcf54bc5208ec349e8592ec9e332f778f74167000ced&",
    "image_width": 960,
    "image_height": 1200,
    "raw_image_url": "https://storage.fonedis.cc/attachments/1372468820912115716/1391374307719905340/cat_sitting_table_f3638ed2-60f6-49dd-897a-6d68c15afb17.png?ex=686ba9cd&is=686a584d&hm=71543c21c38db8a50c7ebcf54bc5208ec349e8592ec9e332f778f74167000ced&",
    "raw_image_width": 960,
    "raw_image_height": 1200,
    "progress": 100,
    "image_id": "1391374390892953600",
    "task_id": "b8b7fdc2-628e-40dd-bc0c-671c3ddac9e9",
    "success": true
}
```

You can see that the result contains a `task_id` field, and the other fields are similar to the above text. This field can be used to associate tasks.

## Error Handling

When calling the API, if an error occurs, the API will return the corresponding error code and message. For example:

- `400 token_mismatched`: Bad request, possibly due to missing or invalid parameters.
- `400 api_not_implemented`: Bad request, possibly due to missing or invalid parameters.
- `401 invalid_token`: Unauthorized, invalid or missing authorization token.
- `429 too_many_requests`: Too many requests, you have exceeded the rate limit.
- `500 api_error`: Internal server error, something went wrong on the server.

### Error Response Example

```json
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

Through this document, you have learned how to use the Midjourney Edits API to edit images by inputting prompts. We hope this document helps you better integrate and use the API. If you have any questions, please feel free to contact our technical support team.