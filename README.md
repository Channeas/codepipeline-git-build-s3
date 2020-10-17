# Codepipeline build to public bucket

## About

This is a [CloudFormation](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/Welcome.html) template that creates a CodePipeline for building code from Github, and then publishing those builds to a public S3 bucket.

Build instructions should be stored in a [buildspec.yml](./buildspec.yml) file, that is placed in the root of the git repository. See the [buildspec.yml](./buildspec.yml) file for an example of how the file should be structured. You can also check the [official documentation](https://docs.aws.amazon.com/codebuild/latest/userguide/build-spec-ref.html)

If you want a template that does not build code, but just copies it to an S3 bucket, see the repository [codepipeline-git-s3](https://github.com/Channeas/codepipeline-git-s3)

## Resources
The template creates 4 resources in total, as well as 2 IAM policies and 1 S3 bucket policy:
* A CodePipeline pipeline with 3 stages:
  * Get_source - Pulls the source code from the git repository
  * Build_code - Builds the code using the CodeBuild project mentioned below
  * Deploy_build - Deploys the build to the output bucket
* A CodeBuild project - Builds your code according to the buildspec.yml file in the repository
* A public S3 bucket for storing the output of the pipeline
* A non-public S3 bucket for storing build artifacts
* An IAM role for the CodePipeline pipeline
* An IAM role for the CodeBuild project
* An S3 bucket policy for the public bucket

## Parameters
The template requires a total of 8 parameters:
1. **BranchName**

   The branch in the git repository that you want to build

2. **BucketName**

   What you want to call the output bucket. Must be DNS compliant

3. **CodeStarConnection**

   Your [CodeStar Connection](https://docs.aws.amazon.com/dtconsole/latest/userguide/connections.html) to where the git repository is located

4. **ErrorDocument**

   The name of your error file

5. **IndexDocument**

   The name of your index file

6. **PipelineName**

   What you want to call the CodePipeline pipeline

7. **RepositoryOwner**

   The username of the git repository owner

8. **RepositoryName**

   The name of the git repository
