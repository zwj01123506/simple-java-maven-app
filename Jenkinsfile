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
                 sh '''
                    sed -i \'257d\' /usr/share/maven/conf/settings.xml
                    sed -i \'90,105d\' /usr/share/maven/conf/settings.xml
                    echo "<proxies>" >> /usr/share/maven/conf/settings.xml
                    echo "<proxy>" >> /usr/share/maven/conf/settings.xml
                    echo "<id>my-proxy</id>" >> /usr/share/maven/conf/settings.xml
                    echo "<active>true</active>" >> /usr/share/maven/conf/settings.xml
                    echo "<protocol>http</protocol>" >> /usr/share/maven/conf/settings.xml
                    echo "<host>web-proxy.sgp.hp.com</host>" >> /usr/share/maven/conf/settings.xml
                    echo "<port>8080</port>" >> /usr/share/maven/conf/settings.xml
                    echo "</proxy>" >> /usr/share/maven/conf/settings.xml
                    echo "</proxies>" >> /usr/share/maven/conf/settings.xml
                    echo "</settings>" >> /usr/share/maven/conf/settings.xml'''
                 sh 'mvn -B -DskipTests clean package' 
            }
        }
        stage('Test') {
            steps {
                sh 'mvn test'
            }
            post {
                always {
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }
        stage('Deliver') {
            steps {
                sh './jenkins/scripts/deliver.sh'
            }
        }
    }
}
