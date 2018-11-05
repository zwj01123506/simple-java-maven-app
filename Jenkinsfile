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
                 sh '''sed -i \'$d\' /usr/share/maven/conf/setting.xml
                    echo "<proxies>" >> /usr/share/maven/conf/setting.xml
                    echo "<proxy>" >> /usr/share/maven/conf/setting.xml
                    echo "<id>my-proxy</id>" >> /usr/share/maven/conf/setting.xml
                    echo "<active>true</active>" >> /usr/share/maven/conf/setting.xml
                    echo "<protocol>http</protocol>" >> /usr/share/maven/conf/setting.xml
                    echo "<host>web-proxy.sgp.hp.com</host>" >> /usr/share/maven/conf/setting.xml
                    echo "<port>8080</port>" >> /usr/share/maven/conf/setting.xml
                    echo "</proxy>" >> /usr/share/maven/conf/setting.xml
                    echo "</proxies>" >> /usr/share/maven/conf/setting.xml
                    echo "</settings>" >> /usr/share/maven/conf/setting.xml'''
                input "Does the staging environment look ok?"
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
    }
}
