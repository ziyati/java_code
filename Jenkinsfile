pipeline {
    agent any
        tools{
        maven 'maven'
    }
    environment {
        NEXUS_DOCKER_REPO = 'nexus:8082'
        REPOSITORY ='/repository/myrep'
    }
    stages {
        stage('Getting') {
            steps {
                // Get some code from a GitHub repository
git branch: 'main', credentialsId: '2bfc22ce-995e-4d2c-bdb5-d707df6dbbd1', url: 'https://github.com/ziyati/java_code'                // Run Maven on a Unix agent.
            }
        }
        stage('Build') {
            steps {
                sh '''
                    echo "Starting Maven build..."
                    mvn -Dmaven.test.failure.ignore=true clean package || {
                      echo "Maven build failed."
                      exit 1
                    }
                '''
            }
        }
        stage('Deploy') {
            steps {
                sh '''
                    echo "Starting Maven build..."
                    docker build -t myimage:v1 .
                    docker tag myimage:v2 $NEXUS_DOCKER_REPO/$REPOSITORY/myimage:v1 
                    docker push $NEXUS_DOCKER_REPO/$REPOSITORY/myimage:v1 $NEXUS_DOCKER_REPO
                '''
            }
        }
    }
}
