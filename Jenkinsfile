node('nodejs') {
    stage('Checkout') {
        git branch: 'main',
            url: 'https://github.com/ykoremura/do400-pipelines-control'
    }
    stage('Backend Tests') {
        sh 'node ./backend/test.js'
    }
    stage('Frontend Tests') {
        sh 'node ./frontend/test.js'
    }
}
pipeline {
    agent {
        node {
            label 'nodejs'
        }
    }
    parameters {
        booleanParam(name: "RUN_FRONTEND_TESTS", defaultValue: true)
    }
    stages {
        stage('Run Tests') {
            parallel {
                stage('Backend Tests') {
                    steps {
                        sh 'node ./backend/test.js'
                    }
                }
                stage('Frontend Tests') {
                    when { expression { params.RUN_FRONTEND_TESTS } }
                        steps {
                            sh 'node ./frontend/test.js'
                        }
                    }
                }
            }
    stage('Deploy') {
        when {
            expression { env.GIT_BRANCH == 'origin/main' }
                beforeInput true
        }
        input {
            message 'Deploy the application?'
        }
        steps {
            echo 'Deploying...'
        }
    }
    }
}
