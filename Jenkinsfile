
pipeline {
    agent any

    tools {
        maven 'Maven-3.9'
        jdk 'JDK-17'
    }

    stages {
        stage('1. Checkout Code') {
            steps {
                echo 'Fetching source code from GitHub...'
                checkout scm
            }
        }

        stage('2. Build') {
            steps {
                echo 'Compiling the Java application...'
                bat 'mvn clean compile'
            }
        }

        stage('3. Unit Test') {
            steps {
                echo 'Running unit tests...'
                bat 'mvn test'
            }
            post {
                always {
                    junit '**/target/surefire-reports/*.xml'
                }
            }
        }

        stage('4. Package') {
            steps {
                echo 'Packaging application into JAR/WAR...'
                bat 'mvn package -DskipTests'
            }
        }

        stage('5. Deploy') {
            steps {
                echo 'Deploying application artifact...'
                bat '''
                    echo Deploying target file to destination folder...
                    if exist target\\*.jar (
                        copy /Y target\\*.jar "%TEMP%\\deployed-app.jar"
                    )
                    echo Deployment completed.
                '''
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed. Check the logs for details.'
        }
    }
}
