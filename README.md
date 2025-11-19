# Coffee Customer

A Node.js microservice for managing coffee supplier data with MySQL database integration.

## Prerequisites

- Docker installed on your system
- Access to MySQL database (AWS RDS or local MySQL instance)
- Node.js 11+ (for local development)

## Configuration

The application uses environment variables to configure database connection. Default values are defined in `app/config/config.js`:

- `APP_DB_HOST`: Database hostname
- `APP_DB_USER`: Database username
- `APP_DB_PASSWORD`: Database password
- `APP_DB_NAME`: Database name

## Docker Build

### Basic Build
```bash
docker build -t customer .
```

### Build with Database Host Argument
```bash
# Extract DB host from config file
dbEndpoint=$(cat app/config/config.js | grep 'APP_DB_HOST' | cut -d '"' -f2)

# Build with build argument
docker build --build-arg DB_HOST=$dbEndpoint -t customer .
```

## Docker Run

### Run with Environment Variables (Recommended)
```bash
# Run with custom database host
docker run -d \
  -p 8080:8080 \
  -e APP_DB_HOST="your-database-host.rds.amazonaws.com" \
  -e APP_DB_USER="nodeapp" \
  -e APP_DB_PASSWORD="coffee" \
  -e APP_DB_NAME="COFFEE" \
  --name customer \
  customer
```

### Run with Extracted Database Host
```bash
# Extract DB host from config
dbEndpoint=$(cat app/config/config.js | grep 'APP_DB_HOST' | cut -d '"' -f2)

# Run container with DB host argument
docker run -d \
  -p 8080:8080 \
  -e APP_DB_HOST=$dbEndpoint \
  --name supplier-api \
  coffee-supplier-api
```