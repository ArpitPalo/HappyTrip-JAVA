pipeline {
    agent any
    stages {
        stage('Build') {
            tools {
                jdk 'jdk8'
            }
            steps {
                echo "Build Project"
                withSonarQubeEnv {
                    powershell label: '', script: 'mvn package -f happytrip-code\\pom.xml sonar:sonar'
                }
                
            }
        }
    }
}
