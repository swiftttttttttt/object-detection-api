# Object Detection API

REST API to detect objects in images. Get labels, confidence scores, and bounding box coordinates. Powered by neural networks.

## Features

- Detect multiple objects in a single image
- Returns object names, confidence scores (0.0-1.0), and bounding box coordinates
- Supports JPEG and PNG formats (up to 10MB)
- 5,000 requests/month on free tier
- Example Response:
```json
[
  {
    "object_name": "mango",
    "confidence_score": 0.61,
    "region": {
      "top_left_x": 7,
      "top_left_y": 177,
      "bottom_right_x": 718,
      "bottom_right_y": 1262
    }
  }
]
```

## Authentication

1. Create account at [omkar.cloud](https://www.omkar.cloud/auth/sign-up)

![Sign Up](https://raw.githubusercontent.com/omkarcloud/assets/master/images/signup.png)

2. Get API key from [omkar.cloud/api-key](https://www.omkar.cloud/api-key)

![Copy API Key](https://raw.githubusercontent.com/omkarcloud/assets/master/images/enrichment-key-omkar.png)

3. Include `API-Key` header in requests

## Quick Start

```bash
curl -X POST "https://object-detection-api.omkar.cloud/detect" \
  -H "API-Key: YOUR_API_KEY" \
  -F "image=@photo.jpg"
```

```json
[
  {
    "object_name": "mango",
    "confidence_score": 0.61,
    "region": {
      "top_left_x": 7,
      "top_left_y": 177,
      "bottom_right_x": 718,
      "bottom_right_y": 1262
    }
  }
]
```

## Installation

### Python

```bash
pip install requests
```

```python
import requests

with open("photo.jpg", "rb") as image_file:
    response = requests.post(
        "https://object-detection-api.omkar.cloud/detect",
        headers={"API-Key": "YOUR_API_KEY"},
        files={"image": image_file}
    )

data = response.json()
for obj in data:
    print(f"Detected: {obj['object_name']} (confidence: {obj['confidence_score']:.2f})")
```

### Node.js

```bash
npm install axios form-data
```

```javascript
import axios from "axios";
import FormData from "form-data";
import fs from "fs";

const form = new FormData();
form.append("image", fs.createReadStream("photo.jpg"));

const response = await axios.post(
    "https://object-detection-api.omkar.cloud/detect",
    form,
    {
        headers: {
            "API-Key": "YOUR_API_KEY",
            ...form.getHeaders()
        }
    }
);

response.data.forEach(obj => {
    console.log(`Detected: ${obj.object_name} (confidence: ${obj.confidence_score})`);
});
```

## API Reference

### Endpoint

```
POST https://object-detection-api.omkar.cloud/detect
```

### Headers

| Header | Required | Description |
|--------|----------|-------------|
| `API-Key` | Yes | API key from [omkar.cloud/api-key](https://www.omkar.cloud/api-key) |
| `Content-Type` | Yes | `multipart/form-data` |

### Request Body

| Field | Required | Description |
|-------|----------|-------------|
| `image` | Yes | Image file (JPEG or PNG, max 10MB) |

### Response Fields

| Field | Type | Description |
|-------|------|-------------|
| `object_name` | string | Detected object label (e.g., "car", "person", "dog") |
| `confidence_score` | float | Model confidence (0.0 to 1.0). Higher = more confident |
| `region` | object | Bounding box coordinates |

Region object:

| Field | Type | Description |
|-------|------|-------------|
| `top_left_x` | int | X coordinate of top-left corner |
| `top_left_y` | int | Y coordinate of top-left corner |
| `bottom_right_x` | int | X coordinate of bottom-right corner |
| `bottom_right_y` | int | Y coordinate of bottom-right corner |

## Examples

### Detect objects and filter by confidence

```python
import requests

with open("photo.jpg", "rb") as image_file:
    response = requests.post(
        "https://object-detection-api.omkar.cloud/detect",
        headers={"API-Key": "YOUR_API_KEY"},
        files={"image": image_file}
    )

# Filter detections with confidence > 0.5
high_confidence = [obj for obj in response.json() if obj['confidence_score'] > 0.5]
for obj in high_confidence:
    print(f"{obj['object_name']}: {obj['confidence_score']:.2%}")
```

### Get bounding box for cropping

```python
import requests

with open("photo.jpg", "rb") as image_file:
    response = requests.post(
        "https://object-detection-api.omkar.cloud/detect",
        headers={"API-Key": "YOUR_API_KEY"},
        files={"image": image_file}
    )

for obj in response.json():
    region = obj['region']
    width = region['bottom_right_x'] - region['top_left_x']
    height = region['bottom_right_y'] - region['top_left_y']
    print(f"{obj['object_name']}: {width}x{height}px at ({region['top_left_x']}, {region['top_left_y']})")
```

### Count specific objects

```python
import requests
from collections import Counter

with open("photo.jpg", "rb") as image_file:
    response = requests.post(
        "https://object-detection-api.omkar.cloud/detect",
        headers={"API-Key": "YOUR_API_KEY"},
        files={"image": image_file}
    )

counts = Counter(obj['object_name'] for obj in response.json())
print(f"Objects found: {dict(counts)}")
```

## Error Handling

```python
import requests

with open("photo.jpg", "rb") as image_file:
    response = requests.post(
        "https://object-detection-api.omkar.cloud/detect",
        headers={"API-Key": "YOUR_API_KEY"},
        files={"image": image_file}
    )

if response.status_code == 200:
    data = response.json()
elif response.status_code == 401:
    # Invalid API key
    pass
elif response.status_code == 413:
    # Image too large (>10MB)
    pass
elif response.status_code == 429:
    # Rate limit exceeded
    pass
```

## Rate Limits

| Plan | Price | Requests/Month |
|------|-------|----------------|
| Free | $0 | 5,000 |
| Starter | $25 | 100,000 |
| Grow | $75 | 1,000,000 |
| Scale | $150 | 10,000,000 |

## Questions? We have answers.

Reach out anytime. We will solve your query within 1 working day.

[![Contact Us on WhatsApp about Object Detection API](https://raw.githubusercontent.com/omkarcloud/assets/master/images/whatsapp-us.png)](https://api.whatsapp.com/send?phone=918178804274&text=I%20have%20a%20question%20about%20the%20Object%20Detection%20API.)

[![Contact Us on Email about Object Detection API](https://raw.githubusercontent.com/omkarcloud/assets/master/images/ask-on-email.png)](mailto:happy.to.help@omkar.cloud?subject=Object%20Detection%20API%20Question)
