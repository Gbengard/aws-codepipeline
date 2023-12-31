# STAGE 3 - Continuous Integration Pipeline

In this phase of the demonstration, you will establish a robust code pipeline utilizing AWS CodeCommit and AWS CodeBuild to facilitate a seamless and continuous build process. The primary objective is to trigger the creation of a new Docker image in Amazon Elastic Container Registry (ECR) every time a new commit is made to the CodeCommit repository. It is important to note that the CodeDeploy step will be temporarily excluded during this setup and will be incorporated in subsequent stages.

## PIPELINE SETUP

1. Navigate to the AWS CodePipeline console at [CodePipeline Console](https://us-east-1.console.aws.amazon.com/codesuite/codepipeline/pipelines).
2. Create a new pipeline.
3. Set the `Pipeline name` as `catpipeline`.
4. Opt for creating a `new service role` with the default name: `AWSCodePipelineServiceRole-us-east-1-catpipeline`.
5. Expand `Advanced Settings` and ensure that the `default location` is configured for the artifact store, and the `default AWS managed key` is selected for the `Encryption key`.
6. Proceed to the next step.

### Source Stage

1. Choose `AWS CodeCommit` as the `Source Provider`.
2. Select `catpipeline-codecommit` for the repository name.
3. Pick the appropriate branch from the `Branch name` dropdown.
4. Under `Detection options`, select `Amazon CloudWatch Events`, and for `Output artifact format`, choose `CodePipeline default`.
5. Proceed to the next step.

### Build Stage

1. Choose `AWS CodeBuild` for the `Build provider`.
2. Set the `Region` to `US East (N.Virginia)`.
3. Locate and choose the previously created project from the `Project name` search box.
4. For `Build type`, opt for `Single Build`.
5. Proceed to the next step.

### Deploy Stage

1. Skip the deploy stage for now, and confirm the configuration.
2. Create the pipeline.

## PIPELINE TESTING

The pipeline will undergo an initial execution, which should complete without any issues. Monitor the progress by clicking on details in the build stage or wait for the pipeline to complete.

Open the Amazon S3 console in a new tab ([S3 Console](https://s3.console.aws.amazon.com/s3/)) and access the `codepipeline-us-east-1-XXXX` bucket. Navigate to `catpipeline/`, which serves as the artifacts area for this pipeline. Keep this tab open, as it will be utilized later in the demonstration.

## UPDATE THE BUILD STAGE

Locate your local copy of the catpipeline-codecommit repository and update the `buildspec.yml` file as follows. Ensure to use a code editor, convert tabs to spaces, and verify correct indentation.

```yaml
version: 0.2

phases:
  pre_build:
    commands:
      - echo Logging in to Amazon ECR...
      - aws ecr get-login-password --region $AWS_DEFAULT_REGION | docker login --username AWS --password-stdin $AWS_ACCOUNT_ID.dkr.ecr.$AWS_DEFAULT_REGION.amazonaws.com
      - REPOSITORY_URI=$AWS_ACCOUNT_ID.dkr.ecr.$AWS_DEFAULT_REGION.amazonaws.com/$IMAGE_REPO_NAME
      - COMMIT_HASH=$(echo $CODEBUILD_RESOLVED_SOURCE_VERSION | cut -c 1-7)
      - IMAGE_TAG=${COMMIT_HASH:=latest}
  build:
    commands:
      - echo Build started on `date`
      - echo Building the Docker image...          
      - docker build -t $REPOSITORY_URI:latest ./repo/container/.
      - docker tag $REPOSITORY_URI:latest $REPOSITORY_URI:$IMAGE_TAG    
  post_build:
    commands:
      - echo Build completed on `date`
      - echo Pushing the Docker image...
      - docker push $REPOSITORY_URI:latest
      - docker push $REPOSITORY_URI:$IMAGE_TAG
      - echo Writing image definitions file...
      - printf '[{"name":"%s","imageUri":"%s"}]' "$IMAGE_REPO_NAME" "$REPOSITORY_URI:$IMAGE_TAG" > imagedefinitions.json
artifacts:
  files: imagedefinitions.json
```

## TEST A COMMIT

Run the following commands:

```bash
git add -A .
git commit -m "updated buildspec.yml"
git push
```

Observe the pipeline execution and witness the automated generation of a new version of the image in ECR when the commit occurs.