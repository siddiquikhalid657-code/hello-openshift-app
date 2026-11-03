pipeline {
    agent any

    environment {
        PATH = "/opt/homebrew/bin:${env.PATH}"
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/siddiquikhalid657-code/hello-openshift-app.git'
            }
        }

        stage('Check OpenShift Connection') {
            steps {
                sh '''
                    echo "Checking OC CLI"
                    which oc
                    oc version --client

                    echo "Checking OpenShift login"
                    oc whoami
                    oc get nodes
                '''
            }
        }

        stage('Create ConfigMap') {
            steps {
                sh '''
                    oc create configmap hello-html \
                      --from-file=index.html \
                      --dry-run=client -o yaml | oc apply -f -
                '''
            }
        }

        stage('Deploy Application') {
            steps {
                sh '''
                    oc apply -f deployment.yaml
                    oc apply -f service.yaml
                    oc apply -f route.yaml
                '''
            }
        }

        stage('Verify Deployment') {
            steps {
                sh '''
                    oc rollout status deployment/hello-app
                    oc get pods
                    oc get svc
                    oc get route hello-app
                '''
            }
        }
    }
}
```
