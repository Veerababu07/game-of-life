pipeline {
    agent { label 'jdk-11-mvn' }
    environment { 
        JAVA_HOME = "/usr/lib/jvm/java-8-openjdk-amd64"
    }
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
                archiveArtifacts artifacts: '**/target/*.jar'
            }
        }
        stage ('junits') {
            steps {
                junit '**/surefire-reports/*.xml'
            }
        }
        
        stage ('artifactory-configuration') {
            steps {
                rtMavenDeployer (
                    id : 'gadida_guddu',
                    releaseRepo : 'default-libs-release-local',
                    snapshotRepo : 'default-libs-snapshot-local',
                    serverId : 'namaddalodi'

                )
            }
        }
        stage ('Exec Maven') {
            steps {
                rtMavenRun (
                    tool: 'ubuntu', // Tool name from Jenkins configuration
                    pom: 'pom.xml',
                    goals: 'package',
                    deployerId: "gadida_guddu"
                )
            }
        }

        stage ('Publish build info') {
            steps {
                rtPublishBuildInfo (
                    serverId: "namaddalodi"
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
