````markdown
## Jenkins Pipeline: Provision an EC2 Instance in AWS (us-east-1)

This Jenkins pipeline automates the process of launching a new Amazon EC2 instance in the **us-east-1** region using the AWS CLI.

### Overview
- **Pipeline Goal:** Deploy a single EC2 instance via Jenkins using AWS credentials stored securely in the Jenkins credentials store.
- **Agent:** Can run on any available Jenkins agent node.
- **AWS Authentication:** Uses the `withAWS` step to inject AWS credentials and set the default AWS region for CLI commands.

### Stage: `Create EC2 Instance`
- **Credentials:** Uses the Jenkins-stored credentials ID `my-aws-credentials`.
- **Region:** `us-east-1` (both in `withAWS` context and explicitly in the AWS CLI command).
- **AMI ID:** `ami-0de716d6197524dd9` (must exist in `us-east-1`).
- **Instance Count:** 1
- **Instance Type:** `t2.micro` (Free Tier eligible).
- **Key Pair:** `keypairforluitccp3` (must be pre-created in `us-east-1`).

### Pipeline Code
```groovy
pipeline {
  agent any
  stages {
    stage('Create EC2 Instance') {
      steps {
        withAWS(credentials: 'my-aws-credentials', region: 'us-east-1') {
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

### How It Works

1. **AWS Context Setup** – The `withAWS` step configures AWS CLI to use the provided credentials and region.
2. **EC2 Launch Command** – The `aws ec2 run-instances` CLI command provisions a new EC2 instance with the given AMI, type, and key pair.
3. **Output** – The CLI returns JSON data containing the new instance’s ID, network details, and state.

### Requirements

* **AWS Steps Plugin** installed in Jenkins.
* **AWS CLI** available on the Jenkins agent.
* The specified AMI ID and key pair must exist in `us-east-1`.
* Sufficient IAM permissions to run `ec2:RunInstances`.

```
```

