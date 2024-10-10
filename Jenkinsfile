pipeline {
    agent {
        label 'agent02'
    }
    stages {
        stage('build') {
            steps {
                dir("$WORKSPACE"){
                    sh 'mvn -B -DskipTests clean package'
                }
            }
            post {
                success {
                    dir("$WORKSPACE"){
                        archiveArtifacts artifacts: "target/my-app-1.0-SNAPSHOT.jar"
                    }
                }
            }
        }
        stage('test') {
            steps {
                dir("$WORKSPACE"){
                    sh 'mvn test'
                }
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
