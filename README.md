# CICD

## Build a CodePipeline
1.	Open CodePipeline Service in aws console. <br/>
    (https://<region>.console.aws.amazon.com/codepipeline)

2.	Before creating a new pipeline, select the region of your choice from top left corner region selection option
    <img src="media/1.png" width="300" title="Select Region">

3.	Select Get started or Create pipeline.

4.	Choose Next step

5.	In Step 2: Source, select Source provider as GitHub from the dropdown
    <img src="media/2.png" width="500" title="Select Source Provider">

6.	Choose Connect to GitHub if not already signed in

7.	Select Repository ApplicationServices

8.	Select desired Branch

9.	Choose Next step

10.	In Step 3: Build, select Build provider as AWS CodeBuild
    <img src="media/3.png" width="500" title="Select Source Provider">

11.	Under Configure your project, select Create a new build project
-	For Project name enter <Desired-Project-Name>
-	For Operating system, select Ubuntu
-	For Runtime, select Node.js
-	For Version, select aws/codebuild/nodejs:8.11.0
-   Leave the Role name under AWS CodeBuild service role as default 
-	Open Advanced section

12.	By default, service role will not have required permissions
-	Open IAM console
-	Select the service role
-	Create & Assign below custom policy
    ```
    {
    "Statement": [
        {
            "Action": [
                "s3:GetObject",
                "s3:GetObjectVersion",
                "s3:GetBucketVersioning"
            ],
            "Resource": "*",
            "Effect": "Allow"
        },
        {
            "Action": [
                "s3:PutObject"
            ],
            "Resource": [
                "arn:aws:s3:::codepipeline*",
                "arn:aws:s3:::elasticbeanstalk*"
            ],
            "Effect": "Allow"
        },
        {
            "Action": [
                "elasticbeanstalk:*",
                "ec2:*",
                "elasticloadbalancing:*",
                "autoscaling:*",
                "cloudwatch:*",
                "s3:*",
                "sns:*",
                "cloudformation:*",
                "rds:*",
                "sqs:*",
                "ecs:*",
                "apigateway:*",
                "iam:PassRole",
                "iam:GetRole"
            ],
            "Resource": "*",
            "Effect": "Allow"
        },
        {
            "Action": [
                "lambda:*"
            ],
            "Resource": "*",
            "Effect": "Allow"
        },
        {
            "Action": [
                "cloudformation:CreateStack",
                "cloudformation:DeleteStack",
                "cloudformation:DescribeStacks",
                "cloudformation:UpdateStack",
                "cloudformation:CreateChangeSet",
                "cloudformation:DeleteChangeSet",
                "cloudformation:DescribeChangeSet",
                "cloudformation:ExecuteChangeSet",
                "cloudformation:SetStackPolicy",
                "cloudformation:ValidateTemplate",
                "iam:PassRole",
                "iam:GetRole",
                "iam:PutRolePolicy"
            ],
            "Resource": "*",
            "Effect": "Allow"
        },
        {
            "Action": [
                "codebuild:BatchGetBuilds",
                "codebuild:StartBuild"
            ],
            "Resource": "*",
            "Effect": "Allow"
        },
        {
            "Effect": "Allow",
            "Action": [
                "devicefarm:ListProjects",
                "devicefarm:ListDevicePools",
                "devicefarm:GetRun",
                "devicefarm:GetUpload",
                "devicefarm:CreateUpload",
                "devicefarm:ScheduleRun"
            ],
            "Resource": "*"
        }
        ],
        "Version": "2012-10-17"
        }
    ```

