pipeline {
    agent any

    tools {
        maven 'Maven 3.3.9'
        jdk 'jdk8'
    }

    stages {
        stage ('Initialize') {
            steps {
                sh '''
                    echo "PATH = ${PATH}"
                    echo "M2_HOME = ${M2_HOME}"
                '''
            }
        }

        /*stage ('Build & Deploy package') {
          steps{
            script {
              def server = Artifactory.server('internet-server')
              def rtMaven = Artifactory.newMavenBuild()
              rtMaven.deployer server: server, releaseRepo: 'cagip-whiteapp-maven-staging-local', snapshotRepo: 'cagip-whiteapp-maven-staging-local'
              rtMaven.tool = 'Maven 3.3.9'
              def buildInfo = rtMaven.run pom: 'pom.xml', goals: 'install'
              server.publishBuildInfo buildInfo
            }
          }
        }*/
        
        stage ('Maven Build') {
            steps {
                sh 'mvn -Dmaven.test.failure.ignore=true install'
            }
            post {
                success {
                    archiveArtifacts 'target/*.jar'
                }
            }
        }

        

    }
}
