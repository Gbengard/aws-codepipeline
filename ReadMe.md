# Advanced CodePipeline Demonstration

This comprehensive demo series guides you through the implementation of a sophisticated code pipeline, encompassing commit, build, and deploy stages.

The advanced demo comprises five key stages:

- **Stage 1:** Configure Security & Create a CodeCommit Repository
- **Stage 2:** Configure CodeBuild to clone the repository, create a container image, and store it on Amazon ECR (Elastic Container Registry)
- **Stage 3:** Configure a CodePipeline with commit and build steps to automate the build on each commit
- **Stage 4:** Create an ECS (Elastic Container Service) Cluster, Target Groups, Application Load Balancer, and configure the code pipeline for deployment to ECS Fargate
- **Stage 5:** CLEANUP

## Instructions

- [Stage 1](https://github.com/gbengard/aws-codepipeline/blob/master/Stage1-CodeCommit.md)
- [Stage 2](https://github.com/gbengard/aws-codepipeline/blob/master/Stage2-CodeBuild.md)
- [Stage 3](https://github.com/gbengard/aws-codepipeline/blob/master/Stage3-CodePipeline.md)
- [Stage 4](https://github.com/gbengard/aws-codepipeline/blob/master/Stage4-CodeDeploy.md)
- [Stage 5](https://github.com/gbengard/aws-codepipeline/blob/master/Stage5-CleanUp.md)
