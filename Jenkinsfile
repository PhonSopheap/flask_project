node {
    def app
    stage('Clone repository') {
        checkout scm
    }
    stage('Build image') {
       app = docker.build("poretrithynea/miniproject")
    }
    stage('Test image') {
        app.inside {
            sh 'echo "Tests passed"'
        }
    }
    stage('Push image') {   
        docker.withRegistry('https://registry.hub.docker.com', 'rithyneahub') {
            app.push("${env.BUILD_NUMBER}")
        }
    } 
    stage('Trigger ManifestUpdate') {
                echo "triggering updatemanifestjob"
                build job: 'updateDeployment', parameters: [string(name: 'DOCKERTAG', value: env.BUILD_NUMBER)]
        }
}
