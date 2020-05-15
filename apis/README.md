<h1> Using Machine Learning APIs </h1>

First, visit <a href="http://console.cloud.google.com/apis">API console</a>, choose "Credentials" on the left-hand menu.  Choose "Create Credentials" and generate an API key for your application. You should probably restrict it by IP address to prevent abuse, but for now, just  leave that field blank and delete the API key after trying out this demo.

Copy-paste your API Key here:


```python
APIKEY="AIzaSyD_NZERfGkw0CY82HTeu41TJOia5KHgNrY"  # Replace with your API key
```

<b> Note: Make sure you generate an API Key and replace the value above. The sample key will not work.</b>

From the same API console, choose "Dashboard" on the left-hand menu and "Enable API".

Enable the following APIs for your project (search for them) if they are not already enabled:
<ol>
<li> Google Translate API </li>
<li> Google Cloud Vision API </li>
<li> Google Natural Language API </li>
<li> Google Cloud Speech API </li>
</ol>

Finally, because we are calling the APIs from Python (clients in many other languages are available), let's install the Python package (it's not installed by default on Datalab)


```python
!pip install --upgrade google-api-python-client
```

    Collecting google-api-python-client
      Downloading google_api_python_client-1.8.3-py3-none-any.whl (58 kB)
    [K     |████████████████████████████████| 58 kB 2.7 MB/s eta 0:00:011
    [?25hRequirement already satisfied, skipping upgrade: six<2dev,>=1.6.1 in /opt/conda/lib/python3.7/site-packages (from google-api-python-client) (1.14.0)
    Requirement already satisfied, skipping upgrade: httplib2<1dev,>=0.9.2 in /opt/conda/lib/python3.7/site-packages (from google-api-python-client) (0.17.3)
    Requirement already satisfied, skipping upgrade: uritemplate<4dev,>=3.0.0 in /opt/conda/lib/python3.7/site-packages (from google-api-python-client) (3.0.1)
    Requirement already satisfied, skipping upgrade: google-api-core<2dev,>=1.13.0 in /opt/conda/lib/python3.7/site-packages (from google-api-python-client) (1.17.0)
    Requirement already satisfied, skipping upgrade: google-auth-httplib2>=0.0.3 in /opt/conda/lib/python3.7/site-packages (from google-api-python-client) (0.0.3)
    Requirement already satisfied, skipping upgrade: google-auth>=1.4.1 in /opt/conda/lib/python3.7/site-packages (from google-api-python-client) (1.14.1)
    Requirement already satisfied, skipping upgrade: googleapis-common-protos<2.0dev,>=1.6.0 in /opt/conda/lib/python3.7/site-packages (from google-api-core<2dev,>=1.13.0->google-api-python-client) (1.51.0)
    Requirement already satisfied, skipping upgrade: setuptools>=34.0.0 in /opt/conda/lib/python3.7/site-packages (from google-api-core<2dev,>=1.13.0->google-api-python-client) (46.1.3.post20200325)
    Requirement already satisfied, skipping upgrade: protobuf>=3.4.0 in /opt/conda/lib/python3.7/site-packages (from google-api-core<2dev,>=1.13.0->google-api-python-client) (3.11.4)
    Requirement already satisfied, skipping upgrade: pytz in /opt/conda/lib/python3.7/site-packages (from google-api-core<2dev,>=1.13.0->google-api-python-client) (2019.3)
    Requirement already satisfied, skipping upgrade: requests<3.0.0dev,>=2.18.0 in /opt/conda/lib/python3.7/site-packages (from google-api-core<2dev,>=1.13.0->google-api-python-client) (2.23.0)
    Requirement already satisfied, skipping upgrade: cachetools<5.0,>=2.0.0 in /opt/conda/lib/python3.7/site-packages (from google-auth>=1.4.1->google-api-python-client) (3.1.1)
    Requirement already satisfied, skipping upgrade: pyasn1-modules>=0.2.1 in /opt/conda/lib/python3.7/site-packages (from google-auth>=1.4.1->google-api-python-client) (0.2.7)
    Requirement already satisfied, skipping upgrade: rsa<4.1,>=3.1.4 in /opt/conda/lib/python3.7/site-packages (from google-auth>=1.4.1->google-api-python-client) (4.0)
    Requirement already satisfied, skipping upgrade: certifi>=2017.4.17 in /opt/conda/lib/python3.7/site-packages (from requests<3.0.0dev,>=2.18.0->google-api-core<2dev,>=1.13.0->google-api-python-client) (2020.4.5.1)
    Requirement already satisfied, skipping upgrade: idna<3,>=2.5 in /opt/conda/lib/python3.7/site-packages (from requests<3.0.0dev,>=2.18.0->google-api-core<2dev,>=1.13.0->google-api-python-client) (2.9)
    Requirement already satisfied, skipping upgrade: urllib3!=1.25.0,!=1.25.1,<1.26,>=1.21.1 in /opt/conda/lib/python3.7/site-packages (from requests<3.0.0dev,>=2.18.0->google-api-core<2dev,>=1.13.0->google-api-python-client) (1.25.9)
    Requirement already satisfied, skipping upgrade: chardet<4,>=3.0.2 in /opt/conda/lib/python3.7/site-packages (from requests<3.0.0dev,>=2.18.0->google-api-core<2dev,>=1.13.0->google-api-python-client) (3.0.4)
    Requirement already satisfied, skipping upgrade: pyasn1<0.5.0,>=0.4.6 in /opt/conda/lib/python3.7/site-packages (from pyasn1-modules>=0.2.1->google-auth>=1.4.1->google-api-python-client) (0.4.8)
    [31mERROR: tfx 0.21.2 has requirement pyarrow<0.16,>=0.15, but you'll have pyarrow 0.16.0 which is incompatible.[0m
    [31mERROR: tfx-bsl 0.21.4 has requirement pyarrow<0.16.0,>=0.15.0, but you'll have pyarrow 0.16.0 which is incompatible.[0m
    [31mERROR: tensorflow-data-validation 0.21.5 has requirement pandas<1,>=0.24, but you'll have pandas 1.0.3 which is incompatible.[0m
    [31mERROR: tensorflow-data-validation 0.21.5 has requirement scikit-learn<0.22,>=0.18, but you'll have scikit-learn 0.22.2.post1 which is incompatible.[0m
    Installing collected packages: google-api-python-client
      Attempting uninstall: google-api-python-client
        Found existing installation: google-api-python-client 1.8.2
        Uninstalling google-api-python-client-1.8.2:
          Successfully uninstalled google-api-python-client-1.8.2
    Successfully installed google-api-python-client-1.8.3


<h2> Invoke Translate API </h2>


```python
# running Translate API
from googleapiclient.discovery import build
service = build('translate', 'v2', developerKey=APIKEY)

# use the service
inputs = ['is it really this easy?', 'amazing technology it is', 'wow this is nice as beer']
outputs = service.translations().list(source='en', target='pt', q=inputs).execute()
# print outputs
for input, output in zip(inputs, outputs['translations']):
  print("EN: {0}\t\t PT: {1}".format(input, output['translatedText']))
```

    EN: is it really this easy?		 PT: é realmente assim tão fácil?
    EN: amazing technology it is		 PT: tecnologia incrível é
    EN: wow this is nice as beer		 PT: uau, isso é legal como cerveja


<h2> Invoke Vision API </h2>

The Vision API can work off an image in Cloud Storage or embedded directly into a POST message. I'll use Cloud Storage and do OCR on this image:<br><img src="https://storage.googleapis.com/cloud-training-demos/vision/sign2.jpg" width="200" />.<br>That photograph is from http://www.publicdomainpictures.net/view-image.php?image=15842



```python
# Running Vision API
import base64
import json
IMAGE="gs://cloud-training-demos/vision/sign2.jpg"
vservice = build('vision', 'v1', developerKey=APIKEY)
request = vservice.images().annotate(body={
        'requests': [{
                'image': {
                    'source': {
                        'gcs_image_uri': IMAGE
                    }
                },
                'features': [{
                    'type': 'TEXT_DETECTION',
                    'maxResults': 3,
                }]
            }],
        })
responses = request.execute(num_retries=3)
print(json.dumps(responses, indent=2))
```

    {
      "responses": [
        {
          "textAnnotations": [
            {
              "locale": "zh",
              "description": "\u8bf7\u60a8\u7231\u62a4\u548c\u4fdd\n\u62a4\u536b\u751f\u521b\u5efa\u4f18\n\u7f8e\u6c34\u73af\u5883\n",
              "boundingPoly": {
                "vertices": [
                  {
                    "x": 121,
                    "y": 80
                  },
                  {
                    "x": 1071,
                    "y": 80
                  },
                  {
                    "x": 1071,
                    "y": 655
                  },
                  {
                    "x": 121,
                    "y": 655
                  }
                ]
              }
            },
            {
              "description": "\u8bf7",
              "boundingPoly": {
                "vertices": [
                  {
                    "x": 160,
                    "y": 80
                  },
                  {
                    "x": 359,
                    "y": 80
                  },
                  {
                    "x": 359,
                    "y": 239
                  },
                  {
                    "x": 160,
                    "y": 239
                  }
                ]
              }
            },
            {
              "description": "\u60a8",
              "boundingPoly": {
                "vertices": [
                  {
                    "x": 361,
                    "y": 80
                  },
                  {
                    "x": 487,
                    "y": 80
                  },
                  {
                    "x": 487,
                    "y": 239
                  },
                  {
                    "x": 361,
                    "y": 239
                  }
                ]
              }
            },
            {
              "description": "\u7231\u62a4",
              "boundingPoly": {
                "vertices": [
                  {
                    "x": 480,
                    "y": 112
                  },
                  {
                    "x": 767,
                    "y": 112
                  },
                  {
                    "x": 767,
                    "y": 239
                  },
                  {
                    "x": 480,
                    "y": 239
                  }
                ]
              }
            },
            {
              "description": "\u548c",
              "boundingPoly": {
                "vertices": [
                  {
                    "x": 784,
                    "y": 112
                  },
                  {
                    "x": 919,
                    "y": 112
                  },
                  {
                    "x": 919,
                    "y": 239
                  },
                  {
                    "x": 784,
                    "y": 239
                  }
                ]
              }
            },
            {
              "description": "\u4fdd",
              "boundingPoly": {
                "vertices": [
                  {
                    "x": 936,
                    "y": 104
                  },
                  {
                    "x": 1071,
                    "y": 104
                  },
                  {
                    "x": 1071,
                    "y": 231
                  },
                  {
                    "x": 936,
                    "y": 231
                  }
                ]
              }
            },
            {
              "description": "\u62a4",
              "boundingPoly": {
                "vertices": [
                  {
                    "x": 121,
                    "y": 280
                  },
                  {
                    "x": 327,
                    "y": 280
                  },
                  {
                    "x": 327,
                    "y": 451
                  },
                  {
                    "x": 121,
                    "y": 451
                  }
                ]
              }
            },
            {
              "description": "\u536b\u751f",
              "boundingPoly": {
                "vertices": [
                  {
                    "x": 329,
                    "y": 280
                  },
                  {
                    "x": 671,
                    "y": 280
                  },
                  {
                    "x": 671,
                    "y": 451
                  },
                  {
                    "x": 329,
                    "y": 451
                  }
                ]
              }
            },
            {
              "description": "\u521b\u5efa",
              "boundingPoly": {
                "vertices": [
                  {
                    "x": 673,
                    "y": 280
                  },
                  {
                    "x": 987,
                    "y": 280
                  },
                  {
                    "x": 987,
                    "y": 451
                  },
                  {
                    "x": 673,
                    "y": 451
                  }
                ]
              }
            },
            {
              "description": "\u4f18",
              "boundingPoly": {
                "vertices": [
                  {
                    "x": 989,
                    "y": 280
                  },
                  {
                    "x": 1043,
                    "y": 280
                  },
                  {
                    "x": 1043,
                    "y": 451
                  },
                  {
                    "x": 989,
                    "y": 451
                  }
                ]
              }
            },
            {
              "description": "\u7f8e",
              "boundingPoly": {
                "vertices": [
                  {
                    "x": 152,
                    "y": 504
                  },
                  {
                    "x": 295,
                    "y": 504
                  },
                  {
                    "x": 295,
                    "y": 647
                  },
                  {
                    "x": 152,
                    "y": 647
                  }
                ]
              }
            },
            {
              "description": "\u6c34",
              "boundingPoly": {
                "vertices": [
                  {
                    "x": 312,
                    "y": 504
                  },
                  {
                    "x": 455,
                    "y": 504
                  },
                  {
                    "x": 455,
                    "y": 647
                  },
                  {
                    "x": 312,
                    "y": 647
                  }
                ]
              }
            },
            {
              "description": "\u73af\u5883",
              "boundingPoly": {
                "vertices": [
                  {
                    "x": 464,
                    "y": 504
                  },
                  {
                    "x": 767,
                    "y": 504
                  },
                  {
                    "x": 767,
                    "y": 655
                  },
                  {
                    "x": 464,
                    "y": 655
                  }
                ]
              }
            }
          ],
          "fullTextAnnotation": {
            "pages": [
              {
                "property": {
                  "detectedLanguages": [
                    {
                      "languageCode": "zh",
                      "confidence": 0.75
                    }
                  ]
                },
                "width": 1280,
                "height": 818,
                "blocks": [
                  {
                    "property": {
                      "detectedLanguages": [
                        {
                          "languageCode": "zh",
                          "confidence": 0.75
                        }
                      ]
                    },
                    "boundingBox": {
                      "vertices": [
                        {
                          "x": 121,
                          "y": 80
                        },
                        {
                          "x": 1071,
                          "y": 80
                        },
                        {
                          "x": 1071,
                          "y": 655
                        },
                        {
                          "x": 121,
                          "y": 655
                        }
                      ]
                    },
                    "paragraphs": [
                      {
                        "property": {
                          "detectedLanguages": [
                            {
                              "languageCode": "zh",
                              "confidence": 0.75
                            }
                          ]
                        },
                        "boundingBox": {
                          "vertices": [
                            {
                              "x": 121,
                              "y": 80
                            },
                            {
                              "x": 1071,
                              "y": 80
                            },
                            {
                              "x": 1071,
                              "y": 655
                            },
                            {
                              "x": 121,
                              "y": 655
                            }
                          ]
                        },
                        "words": [
                          {
                            "property": {
                              "detectedLanguages": [
                                {
                                  "languageCode": "zh"
                                }
                              ]
                            },
                            "boundingBox": {
                              "vertices": [
                                {
                                  "x": 160,
                                  "y": 80
                                },
                                {
                                  "x": 359,
                                  "y": 80
                                },
                                {
                                  "x": 359,
                                  "y": 239
                                },
                                {
                                  "x": 160,
                                  "y": 239
                                }
                              ]
                            },
                            "symbols": [
                              {
                                "property": {
                                  "detectedLanguages": [
                                    {
                                      "languageCode": "zh"
                                    }
                                  ]
                                },
                                "boundingBox": {
                                  "vertices": [
                                    {
                                      "x": 160,
                                      "y": 80
                                    },
                                    {
                                      "x": 359,
                                      "y": 80
                                    },
                                    {
                                      "x": 359,
                                      "y": 239
                                    },
                                    {
                                      "x": 160,
                                      "y": 239
                                    }
                                  ]
                                },
                                "text": "\u8bf7"
                              }
                            ]
                          },
                          {
                            "property": {
                              "detectedLanguages": [
                                {
                                  "languageCode": "zh"
                                }
                              ]
                            },
                            "boundingBox": {
                              "vertices": [
                                {
                                  "x": 361,
                                  "y": 80
                                },
                                {
                                  "x": 487,
                                  "y": 80
                                },
                                {
                                  "x": 487,
                                  "y": 239
                                },
                                {
                                  "x": 361,
                                  "y": 239
                                }
                              ]
                            },
                            "symbols": [
                              {
                                "property": {
                                  "detectedLanguages": [
                                    {
                                      "languageCode": "zh"
                                    }
                                  ]
                                },
                                "boundingBox": {
                                  "vertices": [
                                    {
                                      "x": 361,
                                      "y": 80
                                    },
                                    {
                                      "x": 487,
                                      "y": 80
                                    },
                                    {
                                      "x": 487,
                                      "y": 239
                                    },
                                    {
                                      "x": 361,
                                      "y": 239
                                    }
                                  ]
                                },
                                "text": "\u60a8"
                              }
                            ]
                          },
                          {
                            "property": {
                              "detectedLanguages": [
                                {
                                  "languageCode": "zh"
                                }
                              ]
                            },
                            "boundingBox": {
                              "vertices": [
                                {
                                  "x": 480,
                                  "y": 112
                                },
                                {
                                  "x": 767,
                                  "y": 112
                                },
                                {
                                  "x": 767,
                                  "y": 239
                                },
                                {
                                  "x": 480,
                                  "y": 239
                                }
                              ]
                            },
                            "symbols": [
                              {
                                "property": {
                                  "detectedLanguages": [
                                    {
                                      "languageCode": "zh"
                                    }
                                  ]
                                },
                                "boundingBox": {
                                  "vertices": [
                                    {
                                      "x": 480,
                                      "y": 112
                                    },
                                    {
                                      "x": 615,
                                      "y": 112
                                    },
                                    {
                                      "x": 615,
                                      "y": 239
                                    },
                                    {
                                      "x": 480,
                                      "y": 239
                                    }
                                  ]
                                },
                                "text": "\u7231"
                              },
                              {
                                "property": {
                                  "detectedLanguages": [
                                    {
                                      "languageCode": "zh"
                                    }
                                  ]
                                },
                                "boundingBox": {
                                  "vertices": [
                                    {
                                      "x": 632,
                                      "y": 112
                                    },
                                    {
                                      "x": 767,
                                      "y": 112
                                    },
                                    {
                                      "x": 767,
                                      "y": 239
                                    },
                                    {
                                      "x": 632,
                                      "y": 239
                                    }
                                  ]
                                },
                                "text": "\u62a4"
                              }
                            ]
                          },
                          {
                            "property": {
                              "detectedLanguages": [
                                {
                                  "languageCode": "zh"
                                }
                              ]
                            },
                            "boundingBox": {
                              "vertices": [
                                {
                                  "x": 784,
                                  "y": 112
                                },
                                {
                                  "x": 919,
                                  "y": 112
                                },
                                {
                                  "x": 919,
                                  "y": 239
                                },
                                {
                                  "x": 784,
                                  "y": 239
                                }
                              ]
                            },
                            "symbols": [
                              {
                                "property": {
                                  "detectedLanguages": [
                                    {
                                      "languageCode": "zh"
                                    }
                                  ]
                                },
                                "boundingBox": {
                                  "vertices": [
                                    {
                                      "x": 784,
                                      "y": 112
                                    },
                                    {
                                      "x": 919,
                                      "y": 112
                                    },
                                    {
                                      "x": 919,
                                      "y": 239
                                    },
                                    {
                                      "x": 784,
                                      "y": 239
                                    }
                                  ]
                                },
                                "text": "\u548c"
                              }
                            ]
                          },
                          {
                            "property": {
                              "detectedLanguages": [
                                {
                                  "languageCode": "zh"
                                }
                              ]
                            },
                            "boundingBox": {
                              "vertices": [
                                {
                                  "x": 936,
                                  "y": 104
                                },
                                {
                                  "x": 1071,
                                  "y": 104
                                },
                                {
                                  "x": 1071,
                                  "y": 231
                                },
                                {
                                  "x": 936,
                                  "y": 231
                                }
                              ]
                            },
                            "symbols": [
                              {
                                "property": {
                                  "detectedLanguages": [
                                    {
                                      "languageCode": "zh"
                                    }
                                  ],
                                  "detectedBreak": {
                                    "type": "EOL_SURE_SPACE"
                                  }
                                },
                                "boundingBox": {
                                  "vertices": [
                                    {
                                      "x": 936,
                                      "y": 104
                                    },
                                    {
                                      "x": 1071,
                                      "y": 104
                                    },
                                    {
                                      "x": 1071,
                                      "y": 231
                                    },
                                    {
                                      "x": 936,
                                      "y": 231
                                    }
                                  ]
                                },
                                "text": "\u4fdd"
                              }
                            ]
                          },
                          {
                            "property": {
                              "detectedLanguages": [
                                {
                                  "languageCode": "zh"
                                }
                              ]
                            },
                            "boundingBox": {
                              "vertices": [
                                {
                                  "x": 121,
                                  "y": 280
                                },
                                {
                                  "x": 327,
                                  "y": 280
                                },
                                {
                                  "x": 327,
                                  "y": 451
                                },
                                {
                                  "x": 121,
                                  "y": 451
                                }
                              ]
                            },
                            "symbols": [
                              {
                                "property": {
                                  "detectedLanguages": [
                                    {
                                      "languageCode": "zh"
                                    }
                                  ]
                                },
                                "boundingBox": {
                                  "vertices": [
                                    {
                                      "x": 121,
                                      "y": 280
                                    },
                                    {
                                      "x": 327,
                                      "y": 280
                                    },
                                    {
                                      "x": 327,
                                      "y": 451
                                    },
                                    {
                                      "x": 121,
                                      "y": 451
                                    }
                                  ]
                                },
                                "text": "\u62a4"
                              }
                            ]
                          },
                          {
                            "property": {
                              "detectedLanguages": [
                                {
                                  "languageCode": "zh"
                                }
                              ]
                            },
                            "boundingBox": {
                              "vertices": [
                                {
                                  "x": 329,
                                  "y": 280
                                },
                                {
                                  "x": 671,
                                  "y": 280
                                },
                                {
                                  "x": 671,
                                  "y": 451
                                },
                                {
                                  "x": 329,
                                  "y": 451
                                }
                              ]
                            },
                            "symbols": [
                              {
                                "property": {
                                  "detectedLanguages": [
                                    {
                                      "languageCode": "zh"
                                    }
                                  ]
                                },
                                "boundingBox": {
                                  "vertices": [
                                    {
                                      "x": 329,
                                      "y": 280
                                    },
                                    {
                                      "x": 499,
                                      "y": 280
                                    },
                                    {
                                      "x": 499,
                                      "y": 451
                                    },
                                    {
                                      "x": 329,
                                      "y": 451
                                    }
                                  ]
                                },
                                "text": "\u536b"
                              },
                              {
                                "property": {
                                  "detectedLanguages": [
                                    {
                                      "languageCode": "zh"
                                    }
                                  ]
                                },
                                "boundingBox": {
                                  "vertices": [
                                    {
                                      "x": 500,
                                      "y": 280
                                    },
                                    {
                                      "x": 671,
                                      "y": 280
                                    },
                                    {
                                      "x": 671,
                                      "y": 451
                                    },
                                    {
                                      "x": 500,
                                      "y": 451
                                    }
                                  ]
                                },
                                "text": "\u751f"
                              }
                            ]
                          },
                          {
                            "property": {
                              "detectedLanguages": [
                                {
                                  "languageCode": "zh"
                                }
                              ]
                            },
                            "boundingBox": {
                              "vertices": [
                                {
                                  "x": 673,
                                  "y": 280
                                },
                                {
                                  "x": 987,
                                  "y": 280
                                },
                                {
                                  "x": 987,
                                  "y": 451
                                },
                                {
                                  "x": 673,
                                  "y": 451
                                }
                              ]
                            },
                            "symbols": [
                              {
                                "property": {
                                  "detectedLanguages": [
                                    {
                                      "languageCode": "zh"
                                    }
                                  ]
                                },
                                "boundingBox": {
                                  "vertices": [
                                    {
                                      "x": 673,
                                      "y": 280
                                    },
                                    {
                                      "x": 815,
                                      "y": 280
                                    },
                                    {
                                      "x": 815,
                                      "y": 451
                                    },
                                    {
                                      "x": 673,
                                      "y": 451
                                    }
                                  ]
                                },
                                "text": "\u521b"
                              },
                              {
                                "property": {
                                  "detectedLanguages": [
                                    {
                                      "languageCode": "zh"
                                    }
                                  ]
                                },
                                "boundingBox": {
                                  "vertices": [
                                    {
                                      "x": 816,
                                      "y": 280
                                    },
                                    {
                                      "x": 987,
                                      "y": 280
                                    },
                                    {
                                      "x": 987,
                                      "y": 451
                                    },
                                    {
                                      "x": 816,
                                      "y": 451
                                    }
                                  ]
                                },
                                "text": "\u5efa"
                              }
                            ]
                          },
                          {
                            "property": {
                              "detectedLanguages": [
                                {
                                  "languageCode": "zh"
                                }
                              ]
                            },
                            "boundingBox": {
                              "vertices": [
                                {
                                  "x": 989,
                                  "y": 280
                                },
                                {
                                  "x": 1043,
                                  "y": 280
                                },
                                {
                                  "x": 1043,
                                  "y": 451
                                },
                                {
                                  "x": 989,
                                  "y": 451
                                }
                              ]
                            },
                            "symbols": [
                              {
                                "property": {
                                  "detectedLanguages": [
                                    {
                                      "languageCode": "zh"
                                    }
                                  ],
                                  "detectedBreak": {
                                    "type": "EOL_SURE_SPACE"
                                  }
                                },
                                "boundingBox": {
                                  "vertices": [
                                    {
                                      "x": 989,
                                      "y": 280
                                    },
                                    {
                                      "x": 1043,
                                      "y": 280
                                    },
                                    {
                                      "x": 1043,
                                      "y": 451
                                    },
                                    {
                                      "x": 989,
                                      "y": 451
                                    }
                                  ]
                                },
                                "text": "\u4f18"
                              }
                            ]
                          },
                          {
                            "boundingBox": {
                              "vertices": [
                                {
                                  "x": 152,
                                  "y": 504
                                },
                                {
                                  "x": 295,
                                  "y": 504
                                },
                                {
                                  "x": 295,
                                  "y": 647
                                },
                                {
                                  "x": 152,
                                  "y": 647
                                }
                              ]
                            },
                            "symbols": [
                              {
                                "boundingBox": {
                                  "vertices": [
                                    {
                                      "x": 152,
                                      "y": 504
                                    },
                                    {
                                      "x": 295,
                                      "y": 504
                                    },
                                    {
                                      "x": 295,
                                      "y": 647
                                    },
                                    {
                                      "x": 152,
                                      "y": 647
                                    }
                                  ]
                                },
                                "text": "\u7f8e"
                              }
                            ]
                          },
                          {
                            "boundingBox": {
                              "vertices": [
                                {
                                  "x": 312,
                                  "y": 504
                                },
                                {
                                  "x": 455,
                                  "y": 504
                                },
                                {
                                  "x": 455,
                                  "y": 647
                                },
                                {
                                  "x": 312,
                                  "y": 647
                                }
                              ]
                            },
                            "symbols": [
                              {
                                "boundingBox": {
                                  "vertices": [
                                    {
                                      "x": 312,
                                      "y": 504
                                    },
                                    {
                                      "x": 455,
                                      "y": 504
                                    },
                                    {
                                      "x": 455,
                                      "y": 647
                                    },
                                    {
                                      "x": 312,
                                      "y": 647
                                    }
                                  ]
                                },
                                "text": "\u6c34"
                              }
                            ]
                          },
                          {
                            "boundingBox": {
                              "vertices": [
                                {
                                  "x": 464,
                                  "y": 504
                                },
                                {
                                  "x": 767,
                                  "y": 504
                                },
                                {
                                  "x": 767,
                                  "y": 655
                                },
                                {
                                  "x": 464,
                                  "y": 655
                                }
                              ]
                            },
                            "symbols": [
                              {
                                "boundingBox": {
                                  "vertices": [
                                    {
                                      "x": 464,
                                      "y": 512
                                    },
                                    {
                                      "x": 615,
                                      "y": 512
                                    },
                                    {
                                      "x": 615,
                                      "y": 647
                                    },
                                    {
                                      "x": 464,
                                      "y": 647
                                    }
                                  ]
                                },
                                "text": "\u73af"
                              },
                              {
                                "property": {
                                  "detectedBreak": {
                                    "type": "LINE_BREAK"
                                  }
                                },
                                "boundingBox": {
                                  "vertices": [
                                    {
                                      "x": 624,
                                      "y": 504
                                    },
                                    {
                                      "x": 767,
                                      "y": 504
                                    },
                                    {
                                      "x": 767,
                                      "y": 655
                                    },
                                    {
                                      "x": 624,
                                      "y": 655
                                    }
                                  ]
                                },
                                "text": "\u5883"
                              }
                            ]
                          }
                        ]
                      }
                    ],
                    "blockType": "TEXT"
                  }
                ]
              }
            ],
            "text": "\u8bf7\u60a8\u7231\u62a4\u548c\u4fdd\n\u62a4\u536b\u751f\u521b\u5efa\u4f18\n\u7f8e\u6c34\u73af\u5883\n"
          }
        }
      ]
    }



```python
foreigntext = responses['responses'][0]['textAnnotations'][0]['description']
foreignlang = responses['responses'][0]['textAnnotations'][0]['locale']
print("Lang: {0}\nText: {1}".format(foreignlang, foreigntext))
```

    Lang: zh
    Text: 请您爱护和保
    护卫生创建优
    美水环境
    


<h2> Translate sign </h2>


```python
inputs=[foreigntext]
outputs = service.translations().list(source=foreignlang, target='en', q=inputs).execute()
# print(outputs)
for input, output in zip(inputs, outputs['translations']):
  print("ZH:{0}\nEN:{1}".format(input, output['translatedText']))

outputs = service.translations().list(source=foreignlang, target='pt', q=inputs).execute()
# print(outputs)
for input, output in zip(inputs, outputs['translations']):
  print("\nPT:{1}".format(input, output['translatedText']))
```

    ZH:请您爱护和保
    护卫生创建优
    美水环境
    
    EN:Please care for and protect sanitation to create a beautiful water environment
    
    PT:Por favor, cuide e proteja o saneamento para criar um belo ambiente aquático


<h2> Sentiment analysis with Language API </h2>

Let's evaluate the sentiment of some famous quotes using Google Cloud Natural Language API.


```python
lservice = build('language', 'v1beta1', developerKey=APIKEY)
quotes = [
  'To succeed, you must have tremendous perseverance, tremendous will.',
  'It’s not that I’m so smart, it’s just that I stay with problems longer.',
  'Love is quivering happiness.',
  'Love is of all passions the strongest, for it attacks simultaneously the head, the heart, and the senses.',
  'What difference does it make to the dead, the orphans and the homeless, whether the mad destruction is wrought under the name of totalitarianism or in the holy name of liberty or democracy?',
  'When someone you love dies, and you’re not expecting it, you don’t lose her all at once; you lose her in pieces over a long time — the way the mail stops coming, and her scent fades from the pillows and even from the clothes in her closet and drawers. '
]
for quote in quotes:
  response = lservice.documents().analyzeSentiment(
    body={
      'document': {
         'type': 'PLAIN_TEXT',
         'content': quote
      }
    }).execute()
  polarity = response['documentSentiment']['polarity']
  magnitude = response['documentSentiment']['magnitude']
  print('POLARITY=%s MAGNITUDE=%s for %s' % (polarity, magnitude, quote))
```

    POLARITY=1 MAGNITUDE=0.8 for To succeed, you must have tremendous perseverance, tremendous will.
    POLARITY=-1 MAGNITUDE=0.2 for It’s not that I’m so smart, it’s just that I stay with problems longer.
    POLARITY=1 MAGNITUDE=0.9 for Love is quivering happiness.
    POLARITY=1 MAGNITUDE=0.9 for Love is of all passions the strongest, for it attacks simultaneously the head, the heart, and the senses.
    POLARITY=-1 MAGNITUDE=0.2 for What difference does it make to the dead, the orphans and the homeless, whether the mad destruction is wrought under the name of totalitarianism or in the holy name of liberty or democracy?
    POLARITY=-1 MAGNITUDE=0.3 for When someone you love dies, and you’re not expecting it, you don’t lose her all at once; you lose her in pieces over a long time — the way the mail stops coming, and her scent fades from the pillows and even from the clothes in her closet and drawers. 


<h2> Speech API </h2>

The Speech API can work on streaming data, audio content encoded and embedded directly into the POST message, or on a file on Cloud Storage. Here I'll pass in this <a href="https://storage.googleapis.com/cloud-training-demos/vision/audio.raw">audio file</a> in Cloud Storage.


```python
sservice = build('speech', 'v1', developerKey=APIKEY)
response = sservice.speech().recognize(
    body={
        'config': {
            'languageCode' : 'en-US',
            'encoding': 'LINEAR16',
            'sampleRateHertz': 16000
        },
        'audio': {
            'uri': 'gs://cloud-training-demos/vision/audio.raw'
            }
        }).execute()
print(json.dumps(response, indent=2))
```

    {
      "results": [
        {
          "alternatives": [
            {
              "transcript": "how old is the Brooklyn Bridge",
              "confidence": 0.98314303
            }
          ]
        }
      ]
    }



```python
print(response['results'][0]['alternatives'][0]['transcript'])
print('Confidence=%f' % response['results'][0]['alternatives'][0]['confidence'])
```

    how old is the Brooklyn Bridge
    Confidence=0.983143

