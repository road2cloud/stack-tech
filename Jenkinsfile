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

        stage ('SonarQube Analysis') {
          withSonarQubeEnv('local-server') {
            // requires SonarQube Scanner for Maven 3.2+
            sh 'mvn org.sonarsource.scanner.maven:sonar-maven-plugin:3.2:sonar'
          }
        }

        stage ('Deploy package to Artifactory') {
          def server = Artifactory.newServer url: 'http://localhost:8081/artifactory', credentialsId: 'artifactory-acess'
          def uploadSpec = """{
            "files": [
              {
                "pattern": "target/*.jar",
                "target": "libs-snapshot/stack-tech/"
              }
            ]
          }"""

          server.upload(uploadSpec)
        }

    }
}
