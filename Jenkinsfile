pipeline {
    agent any

    stages {
        stage ('Build & Deploy package') {
          steps{
            script {
              def server = Artifactory.server('local-server')
              def rtMaven = Artifactory.newMavenBuild()
              rtMaven.resolver server: server, releaseRepo: 'libs-release', snapshotRepo: 'libs-snapshot'
              rtMaven.deployer server: server, releaseRepo: 'libs-release-local', snapshotRepo: 'libs-snapshot-local'
              rtMaven.tool = 'Maven 3.3.9'
              def buildInfo = rtMaven.run pom: 'pom.xml', goals: 'install'
              server.publishBuildInfo buildInfo
            }
          }
        }

        stage ('SonarQube Analysis') {
          steps{
            withSonarQubeEnv('local-server') {
                sh 'mvn org.sonarsource.scanner.maven:sonar-maven-plugin:3.2:sonar'
            }
          }
        }

        stage ('Maven Build') {
            post {
                success {
                    archiveArtifacts 'target/*.jar'
                }
            }
        }

    }
}
