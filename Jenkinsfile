pipeline {
    agent any
    stages {

        stage('Clone Repo') {
          steps {
            sh 'rm -rf dockertest1'
            sh 'git clone https://github.com/mavrick202/dockertest1.git'
            }
        }

        stage('Build Docker Image') {
          steps {
            sh 'docker build -t sdevopscalms/pipelinetestingdev:${BUILD_NUMBER} .'
            }
        }

        stage('Push Image to Docker Hub') {
          steps {
           sh    'docker push devopscalms/pipelinetestingdev:${BUILD_NUMBER}'
           }
        }

        stage('Deploy to Docker Host') {
          steps {
            sh    'docker -H tcp://10.0.2.200:2375 stop prodwebapp1 || true'
            sh    'docker -H tcp://10.0.2.200:2375 run --rm -dit --name prodwebapp1 --hostname prodwebapp1 -p 9000:80 devopscalms/pipelinetestingdev:${BUILD_NUMBER}'
            }
        }

        stage('Check WebApp Rechability') {
          steps {
          sh 'sleep 10s'
          sh ' curl http://10.0.2.200:9000'
          }
        }

    }
}
