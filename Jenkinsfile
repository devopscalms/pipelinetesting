pipeline {
    agent any
    stages {

        stage('Clone Repo') {
          steps {
            sh 'rm -rf pipelinetesting'
            sh 'https://github.com/devopscalms/pipelinetesting.git'
            }
        }

        stage('Build Docker Image') {
          steps {
            sh 'cd /var/lib/jenkins/workspace/pipelinetesting/pipelinetesting'
            sh 'cp /var/lib/jenkins/workspace/pipelinetesting/pipelinetesting/* /var/lib/jenkins/workspace/pipelinetesting' 
            sh 'docker build -t devopscalms/pipelinetestingmaster:${BUILD_NUMBER} .'
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
