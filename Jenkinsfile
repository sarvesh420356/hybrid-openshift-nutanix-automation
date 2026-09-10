pipeline {
    agent any

    stages {

        stage('Verify Tools') {
            steps {
                sh 'java -version'
                sh 'ansible --version'
                sh 'oc version --client'
            }
        }

        stage('Check OpenShift Login') {
            steps {
                sh 'oc whoami'
            }
        }

        stage('Pipeline Ready') {
            steps {
                echo 'Hybrid Automation Environment is Ready.'
            }
        }

    }
}
