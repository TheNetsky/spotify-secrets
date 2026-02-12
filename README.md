# spotify-secrets

[![Update Spotify Secrets](https://github.com/TheNetsky/spotify-secrets/actions/workflows/secrets.yml/badge.svg)](https://github.com/TheNetsky/spotify-secrets/actions/workflows/secrets.yml)

## Endpoints
https://raw.githubusercontent.com/TheNetsky/spotify-secrets/refs/heads/main/secrets/secrets.json

## Credits

A Spotify secrets scraper that updates hourly to monitor and extract secrets from Spotify's infrastructure.

Original method by [misiektoja/spotify_monitor](https://github.com/misiektoja/spotify_monitor/blob/dev/debug/spotify_monitor_secret_grabber.py), converted to TypeScript.

## Installation

To install dependencies:

```bash
bun install
```

## Usage

To run:

```bash
bun start
```

## Using the JSON Data

The scraper generates three JSON files that are updated hourly and exposed via raw JSON URLs:

### Plain Secrets (array)

**URL**: `https://github.com/Thereallo1026/spotify-secrets/blob/main/secrets/secrets.json?raw=true`

Returns a JSON array of following objects: `{ "version": number, "secret": string }`

```json
[ { "version": 12, "secret": "secret12" }, { "version": 13, "secret": "secret13" } ]
```

### Secret Bytes (array)

**URL**: `https://github.com/Thereallo1026/spotify-secrets/blob/main/secrets/secretBytes.json?raw=true`

Returns a JSON array of following objects: `{ "version": number, "secret": number[] }`

```json
[ { "version": 12, "secret": [115, 101, 99, 114, 101, 116, 49, 50] }, { "version": 13, "secret": [115, 101, 99, 114, 101, 116, 49, 51] } ]
```

### Secret Bytes (object/dict)

**URL**: `https://github.com/Thereallo1026/spotify-secrets/blob/main/secrets/secretDict.json?raw=true`

Returns a JSON object mapping each version to its array of byte values: `{ [version: string]: number[] }`

```json
{ "12": [115, 101, 99, 114, 101, 116, 49, 50], "13": [115, 101, 99, 114, 101, 116, 49, 51] }
```

### TypeScript Interface

The scraper outputs secrets in one of two unified formats:

**Array format** (`secrets.json`, `secretBytes.json`):

```typescript
interface SpotifySecrets {
	secret: string | number[];
	version: number;
}[];
```

**Object/Dict format** (`secretDict.json`):

```typescript
interface SpotifySecretsDict {
  [version: string]: number[]
}
```

### Usage Example (TypeScript)

```typescript
// Fetch secrets
const response = await fetch('https://github.com/Thereallo1026/spotify-secrets/blob/main/secrets/secrets.json?raw=true');
const secrets: SpotifySecrets = await response.json();

// Get latest version
const latestSecret = secrets[secrets.length - 1];
console.log(`Version ${latestSecret.version}: ${latestSecret.secret}`);
```

### Usage Example (Python)

```python
import requests

# Fetch secrets
response = requests.get("https://github.com/Thereallo1026/spotify-secrets/blob/main/secrets/secretDict.json?raw=true")
secrets = response.json()

# Get latest version
latest_secret = secrets[(v := max(secrets, key=int))]
print(f"Version {v}: {latest_secret}")
```

This project was created using `bun init` in bun v1.2.18. [Bun](https://bun.sh) is a fast all-in-one JavaScript runtime.
