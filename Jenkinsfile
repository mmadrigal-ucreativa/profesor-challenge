pipeline {
    agent {
        label 'worker-prod'
    }


    stages {

        stage('Compilacion') {
            steps {
               sh 'mvn -DskipTests clean install package'
            }
        }


        stage('Build docker image') {
            steps {
                sh 'docker image build -t spring-webapp .'
            }
        }

        stage('Tag docker image') {
            steps {
                sh 'docker image tag spring-webapp mmadrigal/spring-webapp:latest'
            }
        }

        stage('Upload docker image') {
            steps {
                withCredentials([string(credentialsId: 'dockerpwd-id', variable: 'dockerpwd')]) {
                    sh 'docker login -u mmadrigal -p ${dockerpwd}'
                    sh 'docker image push mmadrigal/spring-webapp:latest'
                }
            }
        }
    }
}
