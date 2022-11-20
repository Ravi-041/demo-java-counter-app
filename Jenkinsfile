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
        }
}