pipeline {
    agent {
        docker {
            image 'maven:3-alpine' 
            args '-v /root/.m2:/root/.m2' 
        }
    }
     options {
        skipStagesAfterUnstable()
    }

    stages {
        stage('Build') { 
            steps {
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
    post { 
        always { 
      
            echo "Running ${env.BUILD_ID} on ${env.JENKINS_URL}"
            script {
                response = httpRequest authentication: 'charekId', url: "http://saxon1.fyre.ibm.com:8080/job/simple-java-maven-app/12/wfapi"
                println("Post stage view rest api Status: "+response.status)
                // println("Content: "+response.content)
                response2 = httpRequest consoleLogResponseBody: true, contentType: 'APPLICATION_JSON', httpMode: 'POST', requestBody: response.content, url: "http://leach1.fyre.ibm.com:8080/jenkins", validResponseCodes: '200'
                  println("Post to logstash status: "+response2.status)
                println("Content: "+response2.content)
            }
 
        }
    }
}
