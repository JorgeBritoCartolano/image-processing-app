# Image Processing with AWS Lambda

This project demonstrates how to use AWS services such as Lambda, S3, and SNS to create a simple image processing application. The application takes an image uploaded to an S3 bucket, applies a grayscale filter, and stores the processed image in another S3 bucket. Notifications of the image processing are sent via SNS.

## Architecture

![alt text](<S3 Input.png>)

The application consists of the following AWS components:

- **Amazon S3**: Two buckets are used - one for the input images and another for the output images.
- **AWS Lambda**: A function that processes the images by applying a grayscale filter.
- **AWS SNS**: A notification service that sends out messages when an image has been processed.

## Prerequisites

Before deploying the stack, you will need:

- An AWS account
- A pre-existing S3 bucket to store the Lambda function code as a `.zip` file

## Deployment

### 1. Upload Lambda Function Code

Before running the CloudFormation template, upload the following `.zip` files to an S3 bucket:

- `image_processing_lambda.zip`: Contains the code for the Lambda function that processes images.
- `bucket_notification_function.zip`: Contains the code for the Lambda function that sets up the bucket notification. This is a custom resource to overcome the circular dependency issue when setting up S3 bucket notifications.

### 2. Update CloudFormation Template

Replace the placeholders in the `image-processing-cloudformation.yaml` template with your specific values:

- `<your-input-bucket-name>`: The name of the input S3 bucket.
- `<your-output-bucket-name>`: The name of the output S3 bucket.
- `<your-bucket-name-for-lambda-code>`: The name of the S3 bucket where you uploaded the Lambda function `.zip` files.
- `<your-email>`: Your email address to receive SNS notifications.

Note: The Pillow layer ARN used in this project is tied to a specific version of Python (e.g., Python 3.12). If you change the Python version in the Lambda function, make sure to update the Pillow layer ARN to a compatible version. More Pillow layers can be found here: https://github.com/keithrozario/Klayers

### 3. Deploy the CloudFormation Stack

Use the AWS CLI or the AWS Management Console to deploy the stack.

### 4. Confirm SNS Subscription

Once the stack is deployed, you’ll receive an email to confirm your subscription to the SNS topic. Click the confirmation link in the email.

### 5. Upload an Image to the Input Bucket

Upload an image to the input S3 bucket to trigger the Lambda function. The processed grayscale image will be saved to the output S3 bucket, and you will receive an SNS notification upon completion.