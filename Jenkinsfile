pipeline {
    agent { label 'server1-arya' }

    stages {
        stage('Pull SCM from Aryas Repo') {
            steps {
                git branch: 'main', url: 'https://github.com/aryaprasetyo/simple-apps.git'
            }
        }
        
        stage('Build') {
            steps {
                sh'''
                cd app
                npm install
                '''
            }
        }
        
        stage('Testing') {
            steps {
                sh'''
                cd app
                APP_PORT=4001 npm test
                APP_PORT=4001 npm run test:coverage
                '''
            }
        }
        
        stage('Code Review') {
            steps {
                sh'''
                cd app
                sonar-scanner \
                    -Dsonar.projectKey=simple-apps \
                    -Dsonar.sources=. \
                    -Dsonar.host.url=http://172.23.3.11:9000 \
                    -Dsonar.login=sqp_02696f61c8908a4c6554720203a06ed0f43b3d83
                '''
            }
        }
        
        stage('Deploy') {
            steps {
                sh'''
                docker compose up --build -d
                '''
            }
        }
        
        stage('Backup') {
            steps {
                 sh 'docker compose push' 
            }
        }
    }
}