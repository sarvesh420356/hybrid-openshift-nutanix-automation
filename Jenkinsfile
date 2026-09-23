pipeline {
    agent any

    environment {
        OCP_API = 'https://api.lab.ocp.lan:6443'
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
	
	stage('Nutanix Health Check') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'nutanix-creds',
                    usernameVariable: 'NUTANIX_USERNAME',
                    passwordVariable: 'NUTANIX_PASSWORD'
                )]) {
                    sh '''
                        ansible-playbook \
                          -i ansible/inventory.ini \
                          ansible/nutanix-health.yml
                    '''
                }
            }
        }
	stage('Nutanix Host Health Check') {
	    steps {
		withCredentials([usernamePassword(
		    credentialsId: 'nutanix-creds',
		    usernameVariable: 'NUTANIX_USERNAME',
		    passwordVariable: 'NUTANIX_PASSWORD'
		)]) {
		    sh '''
			ansible-playbook ansible/nutanix-host-health.yml
		    '''
		}
	    }
	}
	
	stage('Nutanix VM inventory') {
	    steps {
		withCredentials([usernamePassword(
		    credentialsId: 'nutanix-creds',
		    usernameVariable: 'NUTANIX_USERNAME',
		    passwordVariable: 'NUTANIX_PASSWORD'
		)]) {
		    sh '''
			ansible-playbook ansible/nutanix-vm-inventory.yml
		    '''
		}
	    }
	}
			
    }

    post {
        success {
            echo 'Pipeline completed successfully.'
        }
        failure {
            echo 'Pipeline failed. Check Console Output.'
        }
    }
}
