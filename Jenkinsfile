pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                echo 'Building application...'
            }
        }

        stage('Test') {
            steps {
                echo 'Testing application...'
            }
        }

        stage('Deploy') {
            steps {
                withCredentials([string(credentialsId: 'openshift-token', variable: 'OC_TOKEN')]) {
                    sh '''
                        /usr/local/bin/oc login https://api.crc.testing:6443 --token="$OC_TOKEN" --insecure-skip-tls-verify=true
                        /usr/local/bin/oc project jenkins-cicd-app
                        /usr/local/bin/oc apply -f openshift/
                        /usr/local/bin/oc start-build jenkins-cicd-app --from-dir=. --follow
                        /usr/local/bin/oc rollout restart deployment/jenkins-cicd-app
                        /usr/local/bin/oc rollout status deployment/jenkins-cicd-app --timeout=180s
                    '''
                }
            }
        }
    }
}
