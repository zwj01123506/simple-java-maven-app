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
                sh 'sed -i '$d' /usr/share/maven/config/setting.xml
                sh 'echo "<proxies>" >> /usr/share/maven/config/setting.xml'
                sh 'echo "<proxy>" >> /usr/share/maven/config/setting.xml'
                sh 'echo "<id>my-proxy</id>" >> /usr/share/maven/config/setting.xml'
                sh 'echo "<active>true</active>" >> /usr/share/maven/config/setting.xml'
                sh 'echo "<protocol>http</protocol>" >> /usr/share/maven/config/setting.xml'
                sh 'echo "<host>web-proxy.sgp.hp.com</host>" >> /usr/share/maven/config/setting.xml'
                sh 'echo "<port>8080</port>" >> /usr/share/maven/config/setting.xml'
                sh 'echo "</proxy>" >> /usr/share/maven/config/setting.xml'
                sh 'echo "</proxies>" >> /usr/share/maven/config/setting.xml'
                sh 'echo "</settings>" >> /usr/share/maven/config/setting.xml'
                input "Does the staging environment look ok?"
                sh 'export https_proxy=http://web-proxy.sgp.hp.com:8080'
                sh 'export http_proxy=http://web-proxy.sgp.hp.com:8080'
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
