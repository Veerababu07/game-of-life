pipeline {
    agent { node { label 'jdk-11-mvn' } }
    stages {
        stage ('vcs') {
            steps {
                git url : "https://github.com/Veerababu07/game-of-life.git",
                 branch : 'master'
            }
        }
        stage ('build') {
            steps {
                sh 'mvn package'
            }
        }
        stage ('artifacts') {
            steps {
                archiveArtifacts artifacts: 'target/*.jar'
            }
        }
        stage ('Artifactory configuration') {
            steps {
                rtServer (
                    id: "namaddalodi",
                    url: "https://veerababu08.jfrog.io",
                    credentialsId: "veerababu.uppala07@gmail.com"
                )
            }
        }
    }
    post {
        always {
            echo 'Job completed'
            mail subject: 'Build Completed', 
                  body: 'Build Completed', 
                  to: 'veerababu.uppala07@gmail.com'
        }
        failure {
            mail subject: 'Build Failed', 
                  body: 'Build Failed', 
                  to: 'veerababu.uppala07@gmail.com' 
        }
        success {
            junit '**/surefire-reports/*.xml'
        }
    }
}    