pipeline {
    agent any

    triggers {
        githubPush()
    }

    

    stages {

        stage('Checkout All Repos') {
            steps {
                // backend
                dir('backend') {
                    git branch: 'main',
                    url: 'https://github.com/caolthanhdai/todo-backend.git'
                    sh 'git reset --hard'
                    sh 'git clean -fdx'
                }

                // frontend
                dir('frontend') {
                    git branch: 'main',
                    url: 'https://github.com/caolthanhdai/todo-frontend.git'
                    sh 'git reset --hard'
                    sh 'git clean -fdx'
                }

                // repo chứa docker-compose
                dir('deploy') {
                    sh 'ls -l -a ../backend'
                    sh 'ls -l ../frontend'
                    git branch: 'main',
                    url: 'https://github.com/caolthanhdai/todo-fullsrc-takingcomposefileforjenkins.git'
                    sh 'git reset --hard'
                    sh 'git clean -fdx'
                }
            }
        }

        stage('Backend CI') {
            steps {
                build job: 'backend-ci'
            }
        }

        stage('Frontend CI') {
            steps {
                build job: 'frontend-ci'
            }
        }

        stage('Deploy with Docker Compose') {
            steps {
                sh '''
                cd deploy
                
                docker-compose down || true
                docker rm -f postgres || true
                docker rm -f backend || true
                docker rm -f frontend || true
                docker-compose up -d --build
                '''
            }
        }
    }
}
