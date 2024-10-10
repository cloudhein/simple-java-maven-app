pipeline {
    agent {
        label 'agent02'
    }
    stages {
        stage('build') {
            steps {
                sh "cd '$WORKSPACE'"
                sh 'mvn -B -DskipTests clean package'
            }
        }
        stage('test') {
            steps {
                sh "cd '$WORKSPACE'"
                sh 'mvn test'
            }
            post {
                success {
                    echo "Test stage is sucessfully run"
                }
            }
        }
        stage('Deploy') {
            steps {
                echo "Deploying Jar package"
                sh 'java -jar $WORKSPACE/target/my-app-1.0-SNAPSHOT.jar'
            }
        }
    }

}
