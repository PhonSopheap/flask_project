pipeline {
    agent any
    environment{
        DOCKER_TAG = "${BUILD_NUMBER}"
    }
 stages{
        stage('Build Docker Image'){
            steps{
                sh 'docker build -t poretrithynea/miniproject:${DOCKER_TAG} .'
            }
        }
        stage('DockerHub Push'){
            steps{
                withCredentials([usernamePassword(credentialsId: 'rithyneahub', usernameVariable: 'dockerUser', passwordVariable: 'dockerHubPwd')]) {
                    sh 'docker login -u ${dockerUser} -p ${dockerHubPwd}'
                    sh 'docker push poretrithynea/miniproject:${DOCKER_TAG}'
                }
            }
        }
        stage('Trigger ManifestUpdates') {
                echo "triggering updatemanifestjob"
                build job: 'updatemanifest', parameters: [string(name: 'DOCKERTAG', value: env.BUILD_NUMBER)]
        }
      }
    }
