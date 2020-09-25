pipeline {
    agent any

    options {
        disableConcurrentBuilds()
    }
        
//    parameters {
//        string(defaultValue: 'jira', description: 'Stack Name', name: 'STACKNAME', trim: true)
 //       string(defaultValue: 'us-east-2', description: 'AWS Region', name: 'AWS_REGION', trim: true)
  //      string(defaultValue: 'vpc-b68224dd', description: 'VPC ID', name: 'VPCID', trim: true)
  //      string(defaultValue: 'subnet-1c9b9f66,subnet-4911df22', description: 'Subnets', name: 'Subnet', trim: true)
   //     string(defaultValue: 'ami-000e7ce4dd68e7a11', description: 'AMI Instance', name: 'AMIID', trim: true)
    //    string(defaultValue: 't2.medium', description: 'Instance Type', name: 'InstanceTypeParam', trim: true)
    //} 

    stages {
        stage('Prepare Agent Environment') {
            steps {
                deleteDir()

                writeFile file: '/tmp/jira.parms.json',
                  text: /
                  [
                    {
                        "ParameterKey": "VPCID",
                        "ParameterValue": "${env.VPCID}"
                    },
                    {
                        "ParameterKey": "Subnet",
                        "ParameterValue": "${env.Subnet}"
                    },
                    {
                        "ParameterKey": "AMIID",
                        "ParameterValue": "${env.AMIID}"
                    },
                    {
                        "ParameterKey": "InstanceTypeParam",
                        "ParameterValue": "${env.InstanceTypeParam}"
                    }
                  ]
                /
            } // End of Steps
        } // End of stage

        stage('Launch Jira Autoscale Stack') {
            options {
                timeout(time: 1, unit: 'HOURS')
            }
            steps {
                script {
                    sh '''mkdir -p ~/.aws/
                    cat > ~/.aws/config <<EOF
[default]
region = us-east-2
output = json
EOF
'''
                    def ret = sh (script: "aws cloudformation describe-stacks --stack-name ${env.STACKNAME}", returnStatus: true)

                    sh "if [[ $ret -eq 0 ]]; then aws cloudformation update-stack --stack-name ${env.STACKNAME} --template-body file://jira.yaml --region ${env.AWS_REGION} --parameters file:///tmp/jira.parms.json && aws cloudformation wait stack-update-complete --stack-name ${env.STACKNAME} --region ${env.AWS_REGION} ; else  aws cloudformation create-stack --stack-name ${env.STACKNAME} --template-body file://jira.yaml --region ${env.AWS_REGION} --parameters file://tmp/jira.parms.json && aws cloudformation wait stack-create-complete --stack-name ${env.STACKNAME} --region ${env.AWS_REGION}; fi"
                }

            } // End of steps
        } // End of stage
    } // End of stages
} // End of pipeline
