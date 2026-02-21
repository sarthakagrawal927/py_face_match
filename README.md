# py_face_match

![AI Generated](https://ai-percentage-pin.vercel.app/api/ai-percentage?value=25)
![AI PRs Welcome](https://ai-percentage-pin.vercel.app/api/ai-prs?welcome=yes)

A lightweight Flask API for face matching using `face_recognition`/dlib.

## What It Does

- Exposes an HTTP endpoint to compare one target image against many candidate images
- Supports local file paths and remote URLs
- Returns a boolean-style match result

## API

### `POST /compare`

Request body:

```json
{
  "target": "https://example.com/target.jpg",
  "paths": [
    "https://example.com/candidate-1.jpg",
    "https://example.com/candidate-2.jpg"
  ],
  "local": 0
}
```

Response:

- `{ "result": 1 }` => at least one match found
- `{ "result": 0 }` => no match found
- `{ "result": -1 }` => invalid input

`local=1` means `target`/`paths` are local filesystem paths.

## Setup

```bash
python -m venv venv
source venv/bin/activate
pip install -r requirements.txt

# dev
flask --app main run

# production-style
gunicorn -w 4 main:app
```

## Docker

```bash
docker build -t py-face-match .
docker run --rm -p 8000:8000 py-face-match
```

## Test Assets

Sample images are available under `test/`.

## Known Limitations

- Assumes at least one detectable face in each image
- No explicit exception handling for missing/invalid faces
- Sequential comparison (no parallel worker pool enabled)
