# Object Detection (Yolov3)

*Yolov3 object detection*


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
| Detection       | Accuracy
|---------------	|-------------------------------
| Face - Accurate | ~95% (frontal faces) (accurate model)
| Face - Fast     | ~90% (frontal faces) (accurate model)
| Age      	      | +- 5 years

There are two detectors built into this container. You can toggle between them in the post parameters

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
docker run -ti \\
-p 9090:9090 \\
sugarkubes/tensorflow-age-gender:cpu

#gpu
nvidia-docker run -ti \\
-p 9090:9090 \\
sugarkubes/tensorflow-age-gender:gpu
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
  "face_detector": "fast", # One of ["accurate", "fast"]
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
| GPU          | "" (true for GPU version)
| GPU_FRACTION | 0.25 (25% of the gpu will be allocated to this model)
| BASIC_AUTH_USERNAME | ""
| BASIC_AUTH_PASSWORD | ""

## Google Cloud Run Enabled

- This container can immediately be deployed using Google's new Cloud Run serverless service (cpu inference only)
- Its a cheap and quick way to get the model online

## Response

```json
{
  //         x1   y1   x2   y2   w    h  conf age gender
  "faces": [[451, 0, 914, 452, 463, 514, -1, 37, "M"]],
  "image_size": [1920, 1080],
  "inference_time": 431.462,
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
