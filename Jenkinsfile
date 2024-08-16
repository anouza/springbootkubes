pipeline {
    agent any

    environment {
        MAVEN_ARGS = "-e clean install"
    }

    stages {
        stage('Build Maven Project'){
            steps {
                withMaven(maven: 'MAVEN_HOME'){
                    sh "mvn ${MAVEN_ARGS}"
                }
            }
        }

        stage('Run Spring Application'){
            steps {
                withEnv(["PATH=/usr/local/bin:$PATH"]){
                    sh "java -jar target/demo-0.0.1-SNAPSHOT.jar"
                }
            }
        }
    }

    post {
        always {
            clearWs()
        }
        success {
            echo 'Success'
        }
        failure {
            echo 'Failed'
        }
    }
}