pipeline {
    agent any

    stages {
        stage('Test') {
            steps {
                echo "testing the application..."
                sh 'mvn test'
            }
        }
        
        stage('Build Jar') {
            steps {
                echo "building the application..."
                sh 'mvn package'
            }
        }
        
        stage('Build Docker Image') {
            steps {
                echo "building the docker image..."
                withCredentials([usernamePassword(credentialsId: 'dockerhub', passwordVariable: 'PASS', usernameVariable: 'USER')]) {
                    script {
                        sh 'docker build -t anshu049/demo-app:jma-2.0 .'
                        sh "echo $PASS | docker login -u $USER --password-stdin"
                        sh 'docker push anshu049/demo-app:jma-2.0'
                    }
                }
            }
        }

        stage('deploy') {
            environment {
               AWS_ACCESS_KEY_ID = credentials('access_key')
               AWS_SECRET_ACCESS_KEY = credentials('secret_access_key')
            }
            steps {
                script {
                   echo 'deploying docker image...'
                   sh 'kubectl create deployment nginx-deployment --image=nginx'
                }
            }
        }
    }
}
