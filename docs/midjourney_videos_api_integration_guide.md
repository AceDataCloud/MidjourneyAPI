# Midjourney Videos API Integration Instructions

This article will introduce a Midjourney Videos API integration guide, which allows you to generate official Midjourney videos by inputting custom parameters.

## Application Process

To use the API, you first need to apply for the corresponding service on the [Midjourney Videos API](https://platform.acedata.cloud/documents/midjourney-videos) page. After entering the page, click the "Acquire" button, as shown in the image below:

![](https://cdn.acedata.cloud/q6ytrc.png)

If you are not logged in or registered, you will be automatically redirected to the login page inviting you to register and log in. After logging in or registering, you will be automatically returned to the current page.

Upon your first application, there will be a free quota available for you to use the API for free.

## Basic Usage

First, understand the basic usage method, which involves inputting the prompt `prompt`, the action `action`, and the array of reference images for the first and last frames `image_url` to obtain the processed result. You first need to simply pass a field `action` with the value `generate`, which mainly includes two actions: generate video (`generate`), extend video (`extend`). The specific content is as follows:

<p><img src="https://cdn.acedata.cloud/pi72m9.png" width="500" class="m-auto"></p>

Here, we can see that we have set the Request Headers, including:

- `accept`: the format of the response result you want to receive, here filled in as `application/json`, which means JSON format.
- `authorization`: the key to call the API, which can be directly selected after application.

Additionally, the Request Body is set, including:

- `image_url`: the link to the reference image for the first frame of the generated video.
- `end_image_url`: optional, specifies the reference image for the last frame of the generated video.
- `video_id`: the video ID that needs to be specified when extending the video.
- `video_index`: specifies which specific video of the `video_id` when extending the video, with the index starting from 0, defaulting to 0.
- `action`: the action of this video generation task, mainly including two actions: generate video (`generate`), extend video (`extend`).
- `prompt`: the prompt.
- `mode`: the speed mode for video generation, default is fast.
- `resolution`: the video clarity, default is 720p.
- `loop`: whether to generate a looping video, default is false.
- `callback_url`: the URL to receive the callback result.

After selection, you can see that the corresponding code is also generated on the right side, as shown in the image below:

<p><img src="https://cdn.acedata.cloud/y0cw0p.png" width="500" class="m-auto"></p>

Click the "Try" button to test, as shown in the above image, and we get the following result:

```json
{
  "image_url": "https://storage.fonedis.cc/upload_1751816808164156352.png",
  "image_width": 560,
  "image_height": 688,
  "progress": 100,
  "video_id": "1751816807896311",
  "video_urls": [
    "https://storage.fonedis.cc//video/1c67c36c-8177-4f19-ad72-1dc1567265a6/0_0.mp4",
    "https://storage.fonedis.cc//video/1c67c36c-8177-4f19-ad72-1dc1567265a6/0_1.mp4",
    "https://storage.fonedis.cc//video/1c67c36c-8177-4f19-ad72-1dc1567265a6/0_2.mp4",
    "https://storage.fonedis.cc//video/1c67c36c-8177-4f19-ad72-1dc1567265a6/0_3.mp4"
  ],
  "task_id": "037955e0-deee-4050-baa8-1416300d67e2",
  "success": true
}
```

The returned result contains multiple fields, described as follows:

- `success`: the status of the video generation task at this time.
- `task_id`: the ID of the video generation task at this time.
- `image_url`: the cover image of the video generation task at this time.
- `image_width`: the width of the cover image of the video generation task at this time.
- `image_height`: the height of the cover image of the video generation task at this time.
- `video_id`: the video ID of the video generation task at this time.
- `video_urls`: the array of video links of the video generation task at this time.

We can see that we have obtained satisfactory video information, and we only need to obtain the generated Midjourney video using the video link address from `video_urls`.

Additionally, if you want to generate the corresponding integration code, you can directly copy the generated code, for example, the CURL code is as follows:

```shell
curl -X POST 'https://api.acedata.cloud/midjourney/videos' \
-H 'accept: application/json' \
-H 'authorization: Bearer {token}' \
-H 'content-type: application/json' \
-d '{
  "action": "generate",
  "prompt": "A cat sitting on a table",
  "image_url": "https://cdn.acedata.cloud/jgo1cw.jpg"
}'
```

## Extend Video Functionality

If you want to continue generating an already created Kling video, you can set the parameter `action` to `extend` and input the ID of the video you want to continue generating. The video ID can be obtained based on the basic usage.

At this point, you can see that the ID of the video mentioned above is:

```
"video_id": "1751816807896311"
```

> Note that the `video_id` here is the ID of the generated video. If you do not know how to generate a video, you can refer to the basic usage mentioned above to generate a video.

Next, you must fill in the prompt for the next step to customize the generated video, specifying the following content:

- `video_index`: select the index of the video to extend, which is the index of the `video_urls` generated above, starting from 0, with a default value of 0.
- `video_id`: the specified video ID for extending the video.
- `action`: the action for this video extension, which is `extend`.
- `prompt`: the prompt.

An example of the filled form is as follows:

<p><img src="https://cdn.acedata.cloud/855hnj.png" width="500" class="m-auto"></p>

After filling in, the code is automatically generated as follows:

<p><img src="https://cdn.acedata.cloud/p58w39.png" width="500" class="m-auto"></p>

The corresponding Python code:

```python
import requests

url = "https://api.acedata.cloud/midjourney/videos"

headers = {
    "accept": "application/json",
    "authorization": "Bearer {token}",
    "content-type": "application/json"
}

payload = {
    "action": "extend",
    "prompt": "A cat sitting on a table",
    "video_id": "1751816807896311",
    "video_index": 1
}

response = requests.post(url, json=payload, headers=headers)
print(response.text)
```

Click to run, and you will find that you will get a result, as follows:
```json
{
    "image_url": "https://storage.fonedis.cc/upload_1751817471047011172.png",
    "image_width": 560,
    "image_height": 688,
    "progress": 100,
    "video_id": "1751818094559027",
    "video_urls": [
        "https://storage.fonedis.cc//video/a4bd2f43-b925-462d-9725-8aef98403133/0_0.mp4",
        "https://storage.fonedis.cc//video/a4bd2f43-b925-462d-9725-8aef98403133/0_1.mp4",
        "https://storage.fonedis.cc//video/a4bd2f43-b925-462d-9725-8aef98403133/0_2.mp4",
        "https://storage.fonedis.cc//video/a4bd2f43-b925-462d-9725-8aef98403133/0_3.mp4"
    ],
    "task_id": "da3bdcd0-9c21-4b40-877a-2c36e5f479e5",
    "success": true
}
```

It can be seen that the result content is consistent with the above text, which also realizes the extended video function of the video.

## Asynchronous Callback

Due to the relatively long generation time of the Midjourney Videos API, which takes about 1-2 minutes, if the API does not respond for a long time, the HTTP request will keep the connection open, leading to additional system resource consumption. Therefore, this API also provides support for asynchronous callbacks.

The overall process is: when the client initiates a request, an additional `callback_url` field is specified. After the client initiates the API request, the API will immediately return a result containing a `task_id` field information, representing the current task ID. When the task is completed, the generated video result will be sent to the client-specified `callback_url` in the form of a POST JSON, which also includes the `task_id` field, so that the task result can be associated by ID.

Next, let's understand how to operate specifically through an example.

First, the Webhook callback is a service that can receive HTTP requests, and developers should replace it with the URL of their own HTTP server. For convenience in demonstration, a public Webhook sample site https://webhook.site/ is used. By opening this site, you can get a Webhook URL, as shown in the figure:

<p><img src="https://cdn.acedata.cloud/lali6d.png" width="500" class="m-auto"></p>

Copy this URL, and it can be used as a Webhook. The sample here is `https://webhook.site/556e6971-b41f-4fa8-9151-6e91acd0399f`.

Next, we can set the `callback_url` field to the above Webhook URL, while filling in the corresponding parameters, as shown in the figure:

<p><img src="https://cdn.acedata.cloud/vk0l0a.png" width="500" class="m-auto"></p>

Clicking run, you can find that an immediate result is obtained, as follows:

```
{
  "task_id": "b726a27a-f379-4d91-b569-cfe4b7b299ee"
}
```

After a moment, we can observe the generated video result at `https://webhook.site/556e6971-b41f-4fa8-9151-6e91acd0399f`, as shown in the figure:

<p><img src="https://cdn.acedata.cloud/7hcuw8.png" width="500" class="m-auto"></p>

The content is as follows:

```json
{
  "image_url": "https://storage.fonedis.cc/upload_1751818513244368774.png",
  "image_width": 560,
  "image_height": 688,
  "progress": 100,
  "video_id": "1751818512924054",
  "video_urls": [
    "https://storage.fonedis.cc//video/9ff3783e-bcf6-4f11-b738-09aa52318e6e/0_0.mp4",
    "https://storage.fonedis.cc//video/9ff3783e-bcf6-4f11-b738-09aa52318e6e/0_1.mp4",
    "https://storage.fonedis.cc//video/9ff3783e-bcf6-4f11-b738-09aa52318e6e/0_2.mp4",
    "https://storage.fonedis.cc//video/9ff3783e-bcf6-4f11-b738-09aa52318e6e/0_3.mp4"
  ],
  "task_id": "b726a27a-f379-4d91-b569-cfe4b7b299ee",
  "success": true
}
```

It can be seen that the result contains a `task_id` field, and other fields are similar to the above text, allowing the task to be associated through this field.

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

Through this document, you have learned how to use the Midjourney Videos API to generate videos by inputting prompt words and reference images for the first frame. It is hoped that this document can help you better integrate and use this API. If you have any questions, please feel free to contact our technical support team.