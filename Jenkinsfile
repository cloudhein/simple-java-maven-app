pipeline {
    agent {
        label 'agent01'
    }

    tools {
        maven 'mymaven'
    }

    environment {
        build = "maven"
        team = "devops"
    }

    parameters {
        string defaultValue: 'thank your for choosing us', name: 'message', trim: true
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
                        archiveArtifacts artifacts: "target/*.jar"
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
                echo "###########################"
                echo "$build is sucessfully deployed by $team"
                echo "###########################"
                echo "${params.message}"
            }
        }

        stage('Parallel Stage') {
            parallel {
                stage('stage A') {
                    steps {
                        echo "Parallel run on stage A"
                    }
                }
                stage('stage B') {
                    steps {
                        echo "Parallel run on stage B"
                    }
                }
            }
        }
    }

}
