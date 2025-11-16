# Spring Boot S3 Bucket Integration

A Spring Boot application demonstrating file upload and delete operations with Amazon S3 using AWS SDK v2.

## Technologies

- **Spring Boot**: 3.3.4
- **Java**: 21
- **AWS SDK**: 2.28.27 (v2)
- **Build Tool**: Maven

## Features

- Upload files to Amazon S3 bucket
- Delete files from Amazon S3 bucket
- RESTful API endpoints
- AWS SDK v2 integration
- Spring Boot DevTools for development

## Requirements

- Java 21 or higher
- Maven 3.6+
- AWS Account with S3 access
- AWS Access Key and Secret Key

## Getting Started

### 1. Clone the Repository

```bash
git clone https://github.com/hendisantika/SpringBootS3Bucket.git
cd SpringBootS3Bucket
```

### 2. Configure AWS Credentials

Edit `src/main/resources/application.yml` and update the following properties with your AWS credentials:

```yaml
amazonProperties:
  endpointUrl: https://s3.us-east-2.amazonaws.com
  accessKey: YOUR_AWS_ACCESS_KEY
  secretKey: YOUR_AWS_SECRET_KEY
  bucketName: your-bucket-name
  region: your-aws-region
```

**Important Security Note**: Never commit your AWS credentials to version control. Consider using:

- Environment variables
- AWS credentials file (~/.aws/credentials)
- Spring Cloud Config
- AWS IAM roles (for EC2/ECS deployments)

### 3. Configure S3 Bucket Policy

Add the following policy to your S3 bucket to allow public access (adjust based on your security requirements):

```json
{
  "Id": "Policy1611359479753",
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "Stmt1611359470980",
      "Action": [
        "s3:DeleteObject",
        "s3:GetObject",
        "s3:PutObject"
      ],
      "Effect": "Allow",
      "Resource": "arn:aws:s3:::your-bucket-name/*",
      "Principal": "*"
    }
  ]
}
```

### 4. Build and Run

```bash
# Build the project
mvn clean compile

# Run the application
mvn spring-boot:run
```

The application will start on `http://localhost:8080`

## API Endpoints

### Upload File

**Endpoint**: `POST /storage/uploadFile`

**Content-Type**: `multipart/form-data`

**Parameters**:

- `file` - The file to upload (form-data)

**Response**: URL of the uploaded file

**Example using cURL**:

```bash
curl -X POST http://localhost:8080/storage/uploadFile \
  -F "file=@/path/to/your/file.jpg"
```

### Delete File

**Endpoint**: `DELETE /storage/deleteFile`

**Content-Type**: `multipart/form-data`

**Parameters**:

- `url` - The URL of the file to delete (form-data)

**Response**: "Successfully deleted"

**Example using cURL**:

```bash
curl -X DELETE http://localhost:8080/storage/deleteFile \
  -F "url=https://s3.us-east-2.amazonaws.com/your-bucket/filename.jpg"
```

## Project Structure

```
src/main/java/com/hendisantika/springboots3bucket/
├── config/
│   └── S3Config.java              # AWS S3 client configuration
├── controller/
│   └── BucketController.java      # REST API endpoints
├── service/
│   └── AmazonClient.java          # S3 operations service
└── SpringBootS3BucketApplication.java
```

## Migration Notes

This project has been migrated from AWS SDK v1 to AWS SDK v2 (2.28.27). Key changes include:

- Updated from `com.amazonaws:aws-java-sdk` to `software.amazon.awssdk:s3`
- Refactored S3 client initialization to use builder pattern
- Updated API calls to use fluent builders
- Created dedicated configuration class for S3Client bean

## Screenshots

### Upload File

![Upload File](img/upload.png "Upload File")

### Delete File

![Delete File](img/delete.png "Delete File")

### AWS S3 Files

![AWS S3 Files](img/s3.png "AWS S3 Files")

## Contributing

Feel free to submit issues and enhancement requests.

## Author

**Hendi Santika**

- Email: hendisantika@gmail.com
- Telegram: @hendisantika34

## License

This project is open source and available under the [MIT License](LICENSE).

