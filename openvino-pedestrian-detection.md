# Hardware Accelerated pedestrian detection and attributes

### OpenVino Pedestrian detection with attributes in ~50ms on an intel cpu!

**Warning! You must be using a 6, 7, or 8th generation intel cpu with ubuntu see [here](https://software.intel.com/en-us/openvino-toolkit/documentation/system-requirements) for more info**

- This model leverages Intel's open vino hardware acceleration for extremely fast inference times. Make sure to read the warning above to ensure you get the most speed out of this!


[Attributes demo!](https://youtu.be/57pi1kfKDv8)


## Speed
| Hardware 	| Inference Time (Milliseconds)
|----------	|-------------------------------
| CPU      	| 35 (intel i-7) + 9ms for attributes

## Accuracy
| Detection       | Accuracy
|---------------	|-------------------------------
| Person          | 88% AP
| Attributes      | ~99% (if person detected)

## Detector

| Model     | Description
|----------	|-------------------------------
| Person    | tensorflow based SSD (mobilenet) converted to openvino format

- trained on an internal dataset with 3000 pedestrians to detect (in outside and inside scenes with camera at eye level or higher)

- internally, the container runs a version of tensorflow serving for use with openvino with an http server in front of it

### Pedestrian attributes

- Attributes include
  - 6 point color palette of pedestrian
  - top and bottom color of pedestrian (great for search!)
  - has_backpack
  - has_bag
  - has_coat_jacket
  - has_hat
  - has_longhair
  - has_longpants
  - has_longsleeves
  - is_male

- people slices are size 80w x 160h

![Example Attributes!](https://s3.us-west-1.wasabisys.com/public.sugarkubes/repos/sugar-cv/intel-person-attributes/pedestrian_attributes.png)

![Example Detection!](https://s3.us-west-1.wasabisys.com/public.sugarkubes/repos/sugar-cv/intel-person-attributes/pedestrian_detection.png)


## Run
```sh
#cpu
docker run -ti \\
-p 9090:9090 \\
sugarkubes/openvino-pedestrian-detection:cpu

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
  -d '{ "get_attributes": true, "url": "https://s3.us-west-1.wasabisys.com/public.sugarkubes/repos/sugar-cv/object-detection/friends.jpg" }'
```


## Example Output

```json
{
  "image_size": [
    1200,
    800
  ],
  "pedestrians": [
    {
      "bottom_color": [
        158,
        150,
        131
      ],
      "confidence": "1.0",
      "has_backpack": false,
      "has_bag": false,
      "has_coat_jacket": false,
      "has_hat": false,
      "has_longhair": false,
      "has_longpants": false,
      "has_longsleeves": true,
      "is_male": false,
      "palette": [
        [
          40,
          41,
          40
        ],
        [
          135,
          141,
          143
        ],
        [
          71,
          71,
          69
        ],
        [
          106,
          108,
          108
        ],
        [
          247,
          248,
          249
        ],
        [
          171,
          174,
          174
        ]
      ],
      "top_color": [
        52,
        55,
        53
      ],
      "x1": 1033,
      "x2": 1190,
      "y1": 126,
      "y2": 804
    },
    {
      "bottom_color": [
        106,
        84,
        96
      ],
      "confidence": "1.0",
      "has_backpack": false,
      "has_bag": false,
      "has_coat_jacket": false,
      "has_hat": false,
      "has_longhair": false,
      "has_longpants": false,
      "has_longsleeves": true,
      "is_male": false,
      "palette": [
        [
          87,
          87,
          89
        ],
        [
          113,
          116,
          119
        ],
        [
          37,
          38,
          38
        ],
        [
          62,
          60,
          62
        ],
        [
          142,
          151,
          161
        ],
        [
          190,
          199,
          208
        ]
      ],
      "top_color": [
        58,
        59,
        57
      ],
      "x1": 691,
      "x2": 848,
      "y1": 183,
      "y2": 777
    },
    ... others
  ],
  "inference_time": 45.057
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

## Validated Hosts
- intel i-3, i-5, i-7

## UI

- UI is located at [localhost:9090/tester/index.html](localhost:9090/tester/index.html)
