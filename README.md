
# 0B55 LIKE API SRC

## Installation & Setup

```
git clone https://github.com/Sumon523022/0B55-LIKE-API-SRC.git
cd 0B55-LIKE-API-SRC

```

A Flask-based Free Fire Like API source project.

## Features

* Flask HTTP API

* `/like` endpoint for sending likes

* `/reset-cache` endpoint for clearing the temporary like cache

* Server-based account file selection

* JWT token generation and temporary token caching

* Protobuf message handling

* AES-CBC UID encryption

* Async HTTP requests with `aiohttp`

* Per-IP daily request limit

* Supports `IND`, `BR`, `US`, `SAC`, `NA`, `BD` and `RU` server names

## Project Structure

```
0B55 LIKE API SRC/  
├── app.py  
├── index.py  
├── wsgi.py  
├── requirements.txt  
├── vercel.json  
├── account_ind.txt  
├── account_br.txt  
├── account_bd.txt  
├── like_pb2.py  
├── like_count_pb2.py  
└── uid_generator_pb2.py

```

## Requirements

Python 3.9+ is recommended.

Install dependencies:

```
pip install -r requirements.txt

```

For Termux:

```
pkg update -y  
pkg install python -y  
pip install -r requirements.txt

```

## Account Files

The application reads account credentials from these files:

| Server | File | 
 | ----- | ----- | 
| IND | account_ind.txt | 
| BR / US / SAC / NA | account_br.txt | 
| BD / RU | account_bd.txt | 

**Format:**

```
UID:PASSWORD

```

**Example:**

```
123456789:YOUR_PASSWORD

```

*Do not publish real account credentials in GitHub or other public repositories.*

## Run Locally

From the project directory:

```
python app.py

```

The server starts on:

* `http://0.0.0.0:5001`

For local testing:

* `http://127.0.0.1:5001`

## Like API

### Endpoint

`GET /like`

### Parameters

| Parameter | Required | Description | 
 | ----- | ----- | ----- | 
| `uid` | Yes | Target Free Fire UID | 
| `server_name` | Yes | Server/region name | 
| `key` | Yes | API key configured by the application | 

**Supported server names:**

* IND

* BR

* US

* SAC

* NA

* BD

* RU

### Example

`http://127.0.0.1:5001/like?uid=123456789&server_name=BD&key=Sumon`

### Successful Response

A successful response contains fields similar to:

```
{  
  "LikesGivenByAPI": 10,  
  "LikesafterCommand": 12345,  
  "LikesbeforeCommand": 12335,  
  "PlayerNickname": "Player",  
  "UID": 123456789,  
  "status": 1,  
  "remains": "(89/90)"  
}

```

### Status Values

* `status: 1` — likes were added

* `status: 2` — no new likes were detected

* `status: 0` — an error occurred

## Daily Request Limit

The application uses:
`KEY_LIMIT = 50`

The request counter is tracked per client IP and resets when a new day begins.

A limit response looks like:

```
{  
  "error": "Daily limit reached",  
  "remains": "(0/50)"  
}

```

## Reset Cache

**Endpoint:**
`GET /reset-cache`

**Example:**
`http://127.0.0.1:5001/reset-cache?key=Sumon`

**Response:**

```
{  
  "message": "Cache cleared"  
}

```

*Use this endpoint carefully.*

## Vercel Deployment

The project includes:

* `index.py`

* `wsgi.py`

* `vercel.json`

`index.py` exposes the Flask application through `wsgi.py`.

Before deploying, make sure the required dependencies are present in `requirements.txt` and that account credentials are stored securely rather than committed to a public repository.

## Important Security Notes

1. Replace the example API key before exposing the service publicly.

2. Never upload real UID/password account credentials to a public repository.

3. Do not expose private JWT tokens.

4. Avoid running Flask debug mode on a public production server.

5. Use HTTPS when the API is exposed over the internet.

6. Protect `/reset-cache` with proper authentication if the service is public.

7. Keep third-party API credentials and account data outside source control.

## Troubleshooting

* `ModuleNotFoundError: No module named 'flask'`

  ```
  pip install Flask
  
  ```

* `ModuleNotFoundError: No module named 'aiohttp'`

  ```
  pip install aiohttp
  
  ```

* `ModuleNotFoundError: No module named 'Crypto'`

  ```
  pip install pycryptodome
  
  ```

* `ModuleNotFoundError: No module named 'jwt'`

  ```
  pip install PyJWT
  
  ```

* **Reinstall all dependencies:**

  ```
  pip install -r requirements.txt
  
  ```

## License

This README documents the supplied source structure. Before redistributing the source, verify that you have permission to use and distribute all included code, credentials, APIs, and generated protocol-buffer files.

## Credits

Original source comments contain multiple author/channel credits. Those credits have not been treated as verified ownership information.