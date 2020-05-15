
# openVINO Pose Estimation

### Pose Estimation in ~100ms on a cpu!

**Warning! You must be using a 6, 7, or 8th generation intel cpu with ubuntu see [here](https://software.intel.com/en-us/openvino-toolkit/documentation/system-requirements) for more info**

- This model leverages hardware acceleration for extremely fast inference times. Make sure to read the warning above to ensure you get the most speed out of this!


[Pose demo!](https://youtu.be/IIU4OMnZPSw)


## Speed
| Hardware 	| Inference Time (Milliseconds)
|----------	|-------------------------------
| CPU      	| 100 (intel i-7)

## Accuracy
| Detection       | Accuracy
|---------------	|-------------------------------
| Person          | 42.8% AP

## Detector

| Model     | Description
|----------	|-------------------------------
| Pose      | multi-person 2d pose estimation (based on open pose) with MobileNetv1 as a feature extractor.


## Run
```sh
#cpu
docker run -ti \\
-p 9090:9090 \\
sugarkubes/openvino-pose-estimation:cpu

```
## ENV Variables

| Variable 	          | Default
|-----------------    |-------------------------------
| PORT                | 8080
| HOST                | 0.0.0.0
| GRPC_PORT           | 9001
| GRPC_HOST           | 0.0.0.0
| BASIC_AUTH_USERNAME | ""
| BASIC_AUTH_PASSWORD | ""


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

## Example Output

```json
"connections": [[1,2],[1,5],[2,3],[3,4],[5,6],[6,7],[1,8],[8,9],[9,10],[1,11],[11,12],[12,13],[1,0],[0,14],[14,16],[0,15],[15,17],[2,16],[5,17]],
  "image_size": [ 1200, 800 ],
  "parts_list": {
    "0": "Nose",
    "1": "Neck",
    "2": "RShoulder",
    "3": "RElbow",
    "4": "RWrist",
    "5": "LShoulder",
    "6": "LElbow",
    "7": "LWrist",
    "8": "RHip",
    "9": "RKnee",
    "10": "RAnkle",
    "11": "LHip",
    "12": "LKnee",
    "13": "LAnkle",
    "14": "REye",
    "15": "LEye",
    "16": "REar",
    "17": "LEar",
    "18": "Background"
  },
  "points": [
    {
      "LAnkle": {
        "confidence": "0.74461085", point confidence
        "part_index": 13, you can look up the parts_list for text
        "x": "0.22807017543859648", x as percent. multiply times image_size[0] for pixel value
        "y": "0.8125" y as percent. multiply by image_size[1]for pixel value
      },
      "LEar": {
        "confidence": "0.93777394",
        "part_index": 17,
        "x": "0.2631578947368421",
        "y": "0.25"
      },
      "LElbow": {
        "confidence": "0.7759637",
        "part_index": 6,
        "x": "0.2982456140350877",
        "y": "0.46875"
      },
      "LEye": {
        "confidence": "0.89811933",
        "part_index": 15,
        "x": "0.24561403508771928",
        "y": "0.25"
      },
      "LHip": {
        "confidence": "0.80005884",
        "part_index": 11,
        "x": "0.2631578947368421",
        "y": "0.53125"
      },
      "LKnee": {
        "confidence": "0.82161593",
        "part_index": 12,
        "x": "0.22807017543859648",
        "y": "0.65625"
      },
      "LShoulder": {
        "confidence": "0.92546237",
        "part_index": 5,
        "x": "0.2807017543859649",
        "y": "0.34375"
      },
      "LWrist": {
        "confidence": "0.91839874",
        "part_index": 7,
        "x": "0.2982456140350877",
        "y": "0.5625"
      },
      "Neck": {
        "confidence": "0.8682677",
        "part_index": 1,
        "x": "0.24561403508771928",
        "y": "0.34375"
      },
      "Nose": {
        "confidence": "1.0116411",
        "part_index": 0,
        "x": "0.22807017543859648",
        "y": "0.25"
      },
      "RAnkle": {
        "confidence": "0.7825638",
        "part_index": 10,
        "x": "0.2807017543859649",
        "y": "0.75"
      },
      "RElbow": {
        "confidence": "0.8665149",
        "part_index": 3,
        "x": "0.19298245614035087",
        "y": "0.4375"
      },
      "RHip": {
        "confidence": "0.86423767",
        "part_index": 8,
        "x": "0.21052631578947367",
        "y": "0.53125"
      },
      "RKnee": {
        "confidence": "0.69661057",
        "part_index": 9,
        "x": "0.24561403508771928",
        "y": "0.65625"
      },
      "RShoulder": {
        "confidence": "0.8426995",
        "part_index": 2,
        "x": "0.21052631578947367",
        "y": "0.34375"
      },
      "RWrist": {
        "confidence": "0.9170083",
        "part_index": 4,
        "x": "0.17543859649122806",
        "y": "0.53125"
      }
    },
    ... + 6 more
    ],
  "speed": "123 ms"

```
## Validated Hosts
- intel i-3, i-5, i-7

### Webcam demo!
 - you can set up the base inference container on a compatible device (see warning), and change the config so if you have the container
 running on an intel compatible device, you can change the grpc_address to point to the intel device and run the api on any x86 machine that runs docker!
