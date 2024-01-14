# S3Service
Service build in java and springboot to demonstrate the upload and retrieval to your AWS s3 buckets. 

## AWS s3 Setup 
- Create AWS instance 
- Create Bucket to store files
- Configure bucket access and name 
- Gather necessary data (tokens, id's regions etc)

## Config 
To connect, config requires
- access key 
- secret key 
- bucket name 

Main engine will be the s3Client 
```
@Bean
    public AmazonS3 s3Client() {
        AWSCredentials credentials = new BasicAWSCredentials(accessKey, accessSecret);
        return AmazonS3ClientBuilder.standard()
                .withCredentials(new AWSStaticCredentialsProvider(credentials))
                .withRegion(region).build();
    }
```

## Controller 
With our controller we are able to upload, download, and delete our files from s3 

### Upload 
The uploadFile method handles POST requests to /file/upload and expects a file in the form data.
```
curl -X POST http://localhost:8080/file/upload \
     -F "file=@/path/to/your/file" \
     -H "Content-Type: multipart/form-data"

```

### Download File 
The downloadFile method handles GET requests to /file/download/{fileName} and allows you to download the file with the specified name.
```
curl -X GET http://localhost:8080/file/download/{fileName} \
     -o {localFileName} \
     -H "Content-type: application/octet-stream"

```

### Deleting File 
The deleteFile method handles DELETE requests to /file/delete/{fileName} and allows you to delete the file with the specified name.
```
curl -X DELETE http://localhost:8080/file/delete/{fileName}

```
## Service 
This service will mainly interact with the AmazonS3 api to interface with s3 operations 

```
s3Client.putObject(new PutObjectRequest(bucketName, fileName, fileObj));

s3Client.getObject(bucketName, fileName);

s3Client.deleteObject(bucketName, fileName);
```