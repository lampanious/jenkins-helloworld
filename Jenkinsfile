pipeline {
    agent { docker { image 'maven:3.3-jdk-8' }
    stages {
        stage('Stage 1') {
            steps {
                echo 'mvn --version' 
            }
        },
        stage('Stage 2') {
            steps {
                echo 'mvn -B -DskipTests clean package' 
            }
        }
    }
}
