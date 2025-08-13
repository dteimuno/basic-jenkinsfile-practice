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
