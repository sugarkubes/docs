# Object Detection (Yolov3)

*Yolov3 object detection*

This container comes with 3 models: 
- Fast (80 classes COCO)
- Accurate (80 classes COCO)
- Accurate 600 (600 classes trained on google open images)

## Speed
| Hardware 	| Model        | Inference Time (Milliseconds)
|----------	|------------- | ----------------------
| CPU      	| Accurate     | 500 (intel i-7)
| CPU      	| Accurate-600 | 500 (intel i-7)
| CPU      	| Fast         | 50 (intel i-7)
| GPU      	| Accurate     | 50 (NVIDIA 2080)
| GPU      	| Accurate-600 | 50 (NVIDIA 2080)
| GPU      	| Fast         | 23 (NVIDIA 2080)

This model's inference time increases sublinearly with the number of people.

## Accuracy
| Detection      | Accuracy
|---------------	|-------------------------------
| Accurate       | 57.9 mAP (COCO)
| Fast           | 33.1 mAP (COCO)
| Accurate-600   | untested

## GPU Consumption
| Model          | Consumption (MB)
|---------------	|-------------------------------
| Accurate       | minimum ~500MB
| Fast           | minimum ~100MB
| Accurate-600   | minimum ~500MB

## Detectors

| Model         | Description
|-------------	|-------------------------------
| Fast          | Tiny Yolo v3 - 80 object classes
| Accurate-600  | yolov3 - 600 object classes. 
| Accurate      | yolov3 - 80 object classes. 

### Fast
 - very quick object detection with lower accuracy
 - can be run with or without a gpu
 - average *cpu* inference time is 80 ms per frame, thats roughly 12 FPS!
 - average *gpu* inference time is 23 ms per frame, roughly 45 FPS!
 - 80 object classes

### Accurate-600
- accurate object detection with slower detection time
- can be run with or without a gpu
- average *cpu* inference time is 500 ms, roughly 2 FPS
- average *gpu* inference time is 50ms, roughly 20 FPS
- 600 object classes though with lower accuracy than the "Accurate" model

### Accurate
 - most accurate object detection with slower detection time
 - can be run with or without a gpu
 - average *cpu* inference time is 500 ms, roughly 2 FPS
 - average *gpu* inference time is 50ms, roughly 20 FPS
 - 80 object classes

## Run
```sh

#cpu
docker run --rm -ti \
-p 8080:8080 \
sugarkubes/yolo-object-detection:cpu

#gpu cuda 10.1
docker run --rm -ti \
-p 8080:8080 \
sugarkubes/yolo-object-detection:gpu-cuda-10.1

#gpu cuda 10.2
docker run --rm -ti \
-p 8080:8080 \
sugarkubes/yolo-object-detection:gpu-cuda-10.2
```


## Routes

`GET /`
`GET /health`
`GET /healthz`
- Responds with a 200 for healthcheck

`POST /predict`
- Example:
```sh
curl -X POST \\
http://0.0.0.0:9090/predict \\
-H 'Content-Type: application/json' \\
-d '{ "url": "https://s3.us-west-1.wasabisys.com/public.sugarkubes/repos/sugar-cv/object-detection/friends.jpg" }'
```

- Post parameters
```json
{
  "nms": 0.5", # non-max supression
  "confidence": 0.5, # confidence score for each bounding boxe (must be above this to be returned)
  "draw": true, # draw bounding boxes on returned image
  "return_image": true, # use false for production/faster results
  "url": 'https://your-image.jpg', # use url or b64 image
  "b64": "", # base 64 encoded image
}
```


## ENV Variables

| Variable 	   | Default
|------------  |-------------------------------
| PORT         | 8080
| HOST         | 0.0.0.0
| BASIC_AUTH_USERNAME | ""
| BASIC_AUTH_PASSWORD | ""
| INPUT_WIDTH | 640
| INPUT_HEIGHT | 640
| MODEL | accurate
| LOG | false

## Google Cloud Run Enabled

- This container can immediately be deployed using Google's new Cloud Run serverless service (cpu inference only)
- Its a cheap and quick way to get the model online

## Response

- response is top left and bottom right of bounding box. 
```json
{
  "data": [
      [
      "Person", #class name
      0.7623502612113953, # confidence
      25,   # x1
      206,  # y1
      224,  # x2
      684   # y2
    ],
    ... other objects
  ],
  "image_size": [1920, 1080],
  "inference_time": 431.462,
  "confidence":0.5,
  "nms": o.5,
}
```


## Authentication
- The container comes with the ability to support basic auth.
- basic auth is disabled by default.
- to turn it off, set `BASIC_AUTH_USERNAME=""` and `BASIC_AUTH_PASSWORD=""`
- to turn it on, set `BASIC_AUTH_USERNAME="root"` and `BASIC_AUTH_PASSWORD="your password"`

## About the container
- python 3.5.2 (cpu)
- python 3.5.1 (gpu)
- tensorflow 1.13.1

## Validated Hosts
- intel i series and e-series x86 machines running ubuntu server 18.04 19.04 20.04
- cuda 10.1, 10.2, 9.2
- Tested on NVIDIA GTX 1080Ti, NVIDIA P1000, NVIDIA Quadro, NVIDIA 20180 
- mac (cpu)
