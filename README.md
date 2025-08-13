````markdown
## Jenkins Pipeline: Create EC2 Instance on AWS

This Jenkins pipeline automates the provisioning of an Amazon EC2 instance using the AWS CLI.

### Overview
- **Purpose:** Launch an EC2 instance in AWS from a Jenkins job.
- **Jenkins Agent:** Runs on any available agent.
- **AWS Integration:** Uses the `withAWS` step from the AWS Steps Plugin to provide credentials and region context.

### Stage: `Create EC2 Instance`
- **AWS Credentials:** Pulls from Jenkins credentials store (`my-aws-credentials`).
- **Region Context:** The `withAWS` block is set to `us-east-1`, but the AWS CLI command specifies `--region us-east-1`, meaning the instance will be created in **us-east-1**.
- **AMI ID:** `ami-0de716d6197524dd9` (must exist in the chosen region).
- **Instance Type:** `t2.micro` (Free Tier eligible).
- **Key Pair:** `keypairforluitccp3` (must exist in the specified region).
- **Instance Count:** 1 instance is launched.

### Code
```groovy
pipeline {
  agent any
  stages {
    stage('Create EC2 Instance') {
      steps {
        withAWS(credentials: 'my-aws-credentials', region: 'us-west-2') {
          sh '''
            aws ec2 run-instances \
              --image-id ami-0de716d6197524dd9 \
              --count 1 \
              --region us-east-1 \
              --instance-type t2.micro \
              --key-name keypairforluitccp3
          '''
        }
      }
    }
  }
}
````

### Notes

* **Region Mismatch Warning:** The `withAWS` region (`us-west-2`) does not match the `--region` flag (`us-east-1`). The actual EC2 instance will be created in `us-east-1`.
* **Dependencies:**

  * AWS CLI installed and available in the Jenkins agent PATH.
  * AWS Steps Plugin installed in Jenkins.
  * Valid AWS credentials configured in Jenkins.
  * AMI ID and key pair must exist in the target region.
* **Output:** AWS CLI returns JSON with details of the launched instance, including Instance ID, availability zone, and networking information.

```
```
