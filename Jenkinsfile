pipeline {
    agent {
        label 'built-in'
    }

    tools {
        maven 'mymaven'
    }

    environment {
        build = "maven"
        server1 = "production environment"
        server2 = "development environment"
    }

    parameters {
        string defaultValue: 'thank your for choosing us', name: 'message', trim: true
        choice choices: ['development', 'production'], name: 'select_your_env'
    }

    stages {
        stage('build_dev') {
            when { 
                expression { params.select_your_env == 'development' }
                beforeAgent true
            }
            agent {
                label 'agent01'
            }
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
        stage('test_dev') {
             when { 
                expression { params.select_your_env == 'development' }
                beforeAgent true
            }
            agent {
                label 'agent01'
            }
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
        stage('Deploy_dev') {
             when { 
                expression { params.select_your_env == 'development' }
                beforeAgent true
            }
            agent {
                label 'agent01'
            }
            steps {
                echo "Deploying Jar package"
                sh 'java -jar $WORKSPACE/target/my-app-1.0-SNAPSHOT.jar'
                echo "###########################"
                echo "$build is sucessfully deployed by $server2"
                echo "###########################"
                echo "${params.message}"
            }
        }

        stage('build_prod') {
            when { 
                expression { params.select_your_env == 'production' }
                beforeAgent true
            }
            agent {
                label 'agent02'
            }
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
                    dir("$WORKSPACE/target"){
                        stash includes: '*.jar', name: 'ProdArtifactsFiles'
                    }
                }
            }
        }
        stage('test_prod') {
             when { 
                expression { params.select_your_env == 'production' }
                beforeAgent true
            }
            agent {
                label 'agent02'
            }
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
        stage('Deploy_prod') {
             when { 
                expression { params.select_your_env == 'production' }
                beforeAgent true
            }
            agent {
                label 'agent02'
            }
            steps {
                echo "Deploying Jar package"
                sh 'java -jar $WORKSPACE/target/my-app-1.0-SNAPSHOT.jar'
                echo "###########################"
                echo "$build is sucessfully deployed by $server1"
                echo "###########################"
                echo "${params.message}"
            }
        }

        stage('Parallel Stage') {
            parallel {
                stage('stage A') {
                    agent {
                        label "agent01"
                    }
                    steps {
                        dir("$WORKSPACE/target/surefire-reports"){
                            stash includes: '*', name: 'DevTestFiles'
                        }
                        echo "Parallel run on agent 01"
                    }
                }
                stage('stage B') {
                    agent {
                        label "agent02"
                    }
                    steps {
                        dir("/tmp"){
                            unstash 'DevTestFiles'
                            echo "test files from development server of stash cache is saved into /tmp folder"
                        }
                        echo "Parallel run on agent 02"
                    }
                }
            }
        }
    }

}