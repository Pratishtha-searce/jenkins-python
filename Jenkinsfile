pipeline {
    agent any

    stages {

        stage('Build') {
            steps {
                sh 'sudo docker build -t gcr.io/searce-playground-v1/cicd-python:${GIT_COMMIT} .'
            }
        }
        
        stage('Push') {
            steps {
                sh 'sudo docker login -u oauth2accesstoken -p "$(gcloud auth print-access-token)" https://gcr.io'
                sh 'sudo docker push gcr.io/searce-playground-v1/cicd-python:${GIT_COMMIT}'
            }
        }
        stage('Trigger ManifestUpdate') {
            steps{
                sh 'echo "triggering updatemanifestjob"'
                sh 'build job: 'updatemanifest', parameters: [string(name: 'DOCKERTAG', value: env.BUILD_NUMBER)]'
            }
        }
    }
}