# Stage 5: Cleanup

During this phase, the objective is to meticulously clean up and eliminate all resources used throughout the demonstration series.

## Amazon ECS  
- Delete Service  
- Delete Task Definition  
- Delete Cluster  

## Amazon EC2  
- Delete Load Balancer  
- Delete Target Groups  

## Code
- Delete Pipeline  
- Delete Build Configuration  
- Delete Source Repository  

## Amazon S3  
- Delete Artifact Bucket  

## AWS Identity and Access Management (IAM)
- Revoke the Service Role created during the advanced demo  

## Amazon Elastic Container Registry (ECR)
- Delete the repository  

## Security Group  
- Delete the 'catpipeline-SG' security group.

## CloudFormation  
- Delete the "Docker" Stack