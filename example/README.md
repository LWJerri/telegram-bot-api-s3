# Example

By default, [tdlib/telegram-bot-api](https://github.com/tdlib/telegram-bot-api)
doesn't provide possibility to download files from API (without local-mode)
so that's meant you will need to expose files somehow differently, for example by nginx.

## Basic Example (Local Storage)

**File:** `docker-compose.yaml`

Uses docker-compose configuration with running 2 containers:

- [lwjerri/telegram-bot-api-s3](https://hub.docker.com/r/lwjerri/telegram-bot-api-s3)
- [nginx](https://hub.docker.com/_/nginx)

This example uses local Docker volume for persistent storage.

```bash
# Set your credentials
export TELEGRAM_API_ID=your_api_id
export TELEGRAM_API_HASH=your_api_hash

# Start services
docker-compose up -d
```

### S3 Storage Example (AWS/S3-compatible)

**File:** `docker-compose.s3.yaml`

Uses S3-compatible storage (AWS S3, DigitalOcean Spaces, Backblaze B2, etc.) for file storage.
Files are uploaded to S3 after being downloaded from Telegram, and presigned URLs are returned.

```bash
# Set your credentials
export TELEGRAM_API_ID=your_api_id
export TELEGRAM_API_HASH=your_api_hash
export S3_BUCKET=your-bucket-name
export S3_ACCESS_KEY_ID=your_access_key
export S3_SECRET_ACCESS_KEY=your_secret_key
export S3_REGION=us-east-1

# Start services
docker-compose -f docker-compose.s3.yaml up -d
```

## S3 Storage Advanced

When S3 storage is enabled:

- Files are uploaded to S3 immediately after being downloaded from Telegram.
- The `file_path` in `getFile` response contains a presigned S3 URL.
- Presigned URLs expire after 1 hour by default (configurable via `S3_PRESIGNED_URL_EXPIRY`).

## Notes

This example is recommended only for local development but in production you can use it only by your own risk.
