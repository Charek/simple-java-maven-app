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
            def response = httpRequest authentication: 'charekId', url: "http://saxon1.fyre.ibm.com:8080/job/simple-java-maven-app/12/wfapi"
            println("Status: "+response.status)
            println("Content: "+response.content)
        }
    }
}
