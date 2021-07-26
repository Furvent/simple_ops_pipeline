pipeline {
    agent any
    tools {
        maven 'MyMaven'
    }
    stages {
        stage('Cleaning workspace') {
            steps {
                sh 'echo "---=--- Cleaning stage ---=---"'
                sh 'mvn clean'
                
                script {
                    try {
                        sh 'docker stop myboot && docker rm myboot'
                    } catch (Exception e) {
                        sh 'echo "---=--- No container to remove ---=---"'
                    }
                }
                
            }
        }
        
        stage('Checkout') {
            steps {
                sh 'echo "---=--- Checkout stage ---=---"'
                git branch:'main', url:'https://github.com/Furvent/simple_ops_pipeline'
            }
        }
        
        stage('Compile') {
            steps {
                sh 'echo "---=--- Compile stage ---=---"'
                sh 'mvn compile'
            }
        }
        
        stage('Test') {
            steps {
                sh 'echo "---=--- Test stage ---=---"'
                sh 'mvn test'
            }
        }
        
        stage('Package') {
            steps {
                sh 'echo "---=--- Package stage ---=---"'
                sh 'mvn package -DskipTests'
            }
        }
        
        stage('Docker') {
            steps {
                sh 'echo "---=--- Docker stage ---=---"'
                sh 'docker build -t furvent/myboot:1.0 .'
            }
        }
        
        stage('Deploy to Local') {
            steps {
                sh 'echo "---=--- Deploy to Local stage ---=---"'
                sh 'docker run -d --name myboot -p 8180:8180 furvent/myboot:1.0'
            }
        }
    }
}
