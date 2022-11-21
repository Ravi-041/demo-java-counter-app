pipeline{
    agent any
        tools {
            maven 'maven'
            jdk 'JDK11'
        }
        stages{
            stage("git checkout"){
                steps{
                    git branch: 'main', url: 'https://github.com/Ravi-041/demo-java-counter-app.git'
                }
            }
            stage("unit testing"){
                steps{
                    sh 'mvn test'
                }   
            }
            stage("Integration tesing"){
                steps{
                    sh 'mvn verify -DskipUnitTests'
                }
            }
            stage("Build stage"){
                steps{
                    sh 'mvn clean install'
                }
            }
            stage("static code analysis"){
                steps{
                    script{
                        withSonarQubeEnv(credentialsId: 'sonar-api') {
                        sh 'mvn clean package sonar:sonar'
                        }
                    }
                }
            }
            stage('Quality Gate Status'){
                steps{
                    script{
                        waitForQualityGate abortPipeline: false, credentialsId: 'sonar-api'
                    }
                }
            }
            stage('Upload War file to nexus'){
                steps{
                    script{
                        def readPomVersion = readMavenPom file: 'pom.xml'
                        nexusArtifactUploader artifacts: 
                        [
                            [artifactId: 'springboot', 
                            classifier: '', 
                            file: 'target/Uber.jar', 
                            type: 'jar'
                            ]
                        ], 
                        credentialsId: 'nexus-auth', 
                        groupId: 'com.example', 
                        nexusUrl: '192.168.0.108:8081', 
                        nexusVersion: 'nexus3', 
                        protocol: 'http', 
                        repository: 'demoapp-release', 
                        version: '${readPomVersion.version}'
                    }
                }
            }
        }
}