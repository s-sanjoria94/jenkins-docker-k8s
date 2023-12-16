pipeline
{
    agent any
    environment {
        AUTHOR = "pragra"
        team = "devops"
        batch = "Jan23"
    }
    stages {
        stage('BUILD')
        {
            steps
            {
                git branch: 'main', url: 'https://github.com/bobbybabu007/jenkins-first-build-project'
                sh 'mvn clean install -DskipTests'
            } 
        }
        stage('UNIT TESTS')
        {
            steps
            {
                sh 'printenv'
                sh 'mvn test'
            }
        }

        stage('INTEGRATION TESTS')
        {
            steps
            {
                sh ''
                sh 'mvn verify -DskipUnitTests'
            }
        }

        stage('CODE ANALYSIS') 
        {
            steps
            {
                sh 'mvn checkstyle:checkstyle'
            }
        }
    }
}