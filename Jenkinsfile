pipeline {
    agent any

    properties([
        disableConcurrentBuilds(), 
        parameters([
            string(defaultValue: 'vpc-b68224dd', description: 'VPC ID', name: 'VPCID', trim: true), 
            string(defaultValue: 'subnet-1c9b9f66,subnet-4911df22', description: 'Subnets', name: 'Subnet', trim: true), 
            string(defaultValue: 'ami-000e7ce4dd68e7a11', description: 'AMI Instance', name: 'AMIID', trim: true), 
            string(defaultValue: 't2.medium', description: 'Instance Type', name: 'InstanceTypeParam', trim: true)])
    ])

    stages {
        stage('Prepare Agent Environment') {
            steps {
                deleteDir()

                sh '''#!/bin/bash
                pwd 
                echo
                ls -l
                '''

            }
        }
    }
}
