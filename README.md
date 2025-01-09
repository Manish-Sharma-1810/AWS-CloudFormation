# AWS-CloudFormation

This repo contains various cloudformation templates

## Steps to deploy IAM role on AWS using CloudFormation

1. **Create a S3 bucket for deployment**:

    ```sh
    aws s3api create-bucket --bucket <BUCKET_NAME> --region us-east-1
    ```

2. **Copy the code to s3 bucket**:

    ```sh
    aws s3 cp ./ s3://<BUCKET_NAME>/ --recursive --region us-east-1
    ```

3. **Create CloudFormation stack for the application**:

    ```sh
    aws cloudformation create-stack \
    --stack-name iam \
    --template-url https://<BUCKET_NAME>.s3.amazonaws.com/iam.yaml \
    --parameters \
        ParameterKey=BucketName,ParameterValue=<BUCKET_NAME> \
        ParameterKey=KeyName,ParameterValue=policy.json \
        ParameterKey=LambdaExecutionPolicyName,ParameterValue=LambdaExecutionPolicy \
        ParameterKey=LambdaExecutionRoleName,ParameterValue=LambdaExecutionRole \
        ParameterKey=ServiceName,ParameterValue=lambda.amazonaws.com \
    --capabilities CAPABILITY_AUTO_EXPAND CAPABILITY_NAMED_IAM \
    --region us-east-1
    ```

4. **Check iam stack**:

    ```sh
    aws cloudformation describe-stacks --stack-name iam --query "Stacks[0].StackStatus" --output text --region us-east-1


    aws cloudformation describe-stacks --stack-name iam --query "Stacks[0].Outputs" --output table --region us-east-1
    ```
