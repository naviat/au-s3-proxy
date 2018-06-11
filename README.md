## Usage

### 1. Set environment variables

Environment Variables     | Description                                       | Required | Default 
------------------------- | ------------------------------------------------- | -------- | -----------------
AWS_S3_BUCKET             | The `S3 bucket` to be proxied with this app.      | *        | 
AWS_S3_KEY_PREFIX         | You can configure `S3 object key` prefix.         |          | -
AWS_REGION                | The AWS `region` where the S3 bucket exists.      |          | us-east-1
AWS_ACCESS_KEY_ID         | AWS `access key` for API access.                  |          | EC2 Instance Role
AWS_SECRET_ACCESS_KEY     | AWS `secret key` for API access.                  |          | EC2 Instance Role
AWS_API_ENDPOINT          | The endpoint for AWS API for local development.   |          | -
INDEX_DOCUMENT            | Name of your index document.                      |          | index.html
DIRECTORY_LISTINGS        | List files when a specified URL ends with /.      |          | false
DIRECTORY_LISTINGS_FORMAT | Configures directory listing to be `html` (spider parsable) |       | - 
HTTP_CACHE_CONTROL        | Overrides S3's HTTP `Cache-Control` header.       |          | S3 Object metadata
HTTP_EXPIRES              | Overrides S3's HTTP `Expires` header.             |          | S3 Object metadata
BASIC_AUTH_USER           | User for basic authentication.                    |          | -
BASIC_AUTH_PASS           | Password for basic authentication.                |          | -
SSL_CERT_PATH             | TLS: cert.pem file path.                          |          | -
SSL_KEY_PATH              | TLS: key.pem file path.                           |          | -
CORS_ALLOW_ORIGIN         | CORS: a URI that may access the resource.         |          | -
CORS_ALLOW_METHODS        | CORS: Comma-delimited list of the allowed [HTTP request methods](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html). |          | -
CORS_ALLOW_HEADERS        | CORS: Comma-delimited list of the supported request headers. |          | -
CORS_MAX_AGE              | CORS: Maximum number of seconds the results of a preflight request can be cached. |          | 600
APP_PORT                  | The port number to be assigned for listening.     |          | 80
APP_HOST                  | The host name used to the listener                |          | Listens on all available unicast and anycast IP addresses of the local system.
ACCESS_LOG                | Send access logs to /dev/stdout.                  |          | false
STRIP_PATH                | Strip path prefix.                                |          | -
CONTENT_ENCODING          | Compress response data if the request allows.     |          | false
HEALTHCHECK_PATH          | If it's specified, the path always returns 200 OK |          | -
GET_ALL_PAGES_IN_DIR      | If true will make several calls to get all pages of destination directory |          | false

### 2. Run the application

`docker run -d -p 8080:80 -e AWS_REGION -e AWS_S3_BUCKET pottava/s3-proxy`

* with basic auth:  

`docker run -d -p 8080:80 -e AWS_REGION -e AWS_S3_BUCKET -e BASIC_AUTH_USER -e BASIC_AUTH_PASS pottava/s3-proxy`

* with TLS:  

`docker run -d -p 8080:80 -e AWS_REGION -e AWS_S3_BUCKET -e SSL_CERT_PATH -e SSL_KEY_PATH pottava/s3-proxy`

* with CORS:

`docker run -d -p 8080:80 -e PROXY_URL -e CORS_ALLOW_ORIGIN -e CORS_ALLOW_METHODS -e CORS_ALLOW_HEADERS -e CORS_MAX_AGE pottava/s3-proxy`

* with docker-compose.yml:  

```
proxy:
  image: pottava/s3-proxy
  ports:
    - 8080:80
  environment:
    - AWS_REGION=ap-northeast-1
    - AWS_ACCESS_KEY_ID
    - AWS_SECRET_ACCESS_KEY
    - AWS_S3_BUCKET
    - BASIC_AUTH_USER=admin
    - BASIC_AUTH_PASS=password
    - ACCESS_LOG=true
  container_name: proxy
```

