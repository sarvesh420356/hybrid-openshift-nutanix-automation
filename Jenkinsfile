pipeline {
    agent any

    environment {
        OCP_API = 'https://api.ocp-nutanix.ntillab.com:6443'
    }

    stages {

        stage('Verify Tools') {
            steps {
                sh 'java -version'
                sh 'ansible --version'
                sh 'oc version --client'
            }
        }

        stage('Login to OpenShift') {
            steps {
                withCredentials([string(credentialsId: 'openshift-token', variable: 'OCP_TOKEN')]) {
                    sh '''
                        oc login ${OCP_API} \
                          --token=${OCP_TOKEN} \
                          --insecure-skip-tls-verify=true
                    '''
                }
            }
        }

        stage('Check OpenShift Login') {
            steps {
                sh 'oc whoami'
                sh 'oc get nodes'
            }
        }

        stage('Pipeline Ready') {
            steps {
                echo 'Hybrid Automation Environment is Ready.'
            }
        }
    }
}
