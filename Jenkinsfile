pipeline {
    agent {
        docker {
            image 'maven:3-alpine' 
            args '-v /root/.m2:/root/.m2' 
        }
    }
    stages {
        stage('Build') { 
            steps {
                sh 'export https_proxy=http://web-proxy.sgp.hp.com:8080'
                sh 'export http_proxy=http://web-proxy.sgp.hp.com:8080'
                sh 'mvn -B -DskipTests clean package' 
            }
        }
    }
}
