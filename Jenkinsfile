pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/siddiquikhalid657-code/hello-openshift-app.git'
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
