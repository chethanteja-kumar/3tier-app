pipeline {
    agent any

    stages {

        stage('Clone Repository') {
            steps {
                echo 'Cloning Repository'
            }
        }

        stage('Build Frontend') {
            steps {
                dir('frontend') {
                    sh '''
                    npm install
                    '''
                }
            }
        }

        stage('Deploy Frontend') {
            steps {
                sh '''
                scp -r frontend/* ec2-user@13.220.42.209:/usr/share/nginx/html/
                '''
            }
        }

        stage('Deploy Backend') {
            steps {
                sh '''
                ssh ec2-user@3.94.153.78 "
                    rm -rf 3tier-app
                    git clone https://github.com/chethanteja-kumar/3tier-app.git
                    cd 3tier-app/backend
                    npm install
                    pkill node || true
                    nohup node server.js > app.log 2>&1 &
                "
                '''
            }
        }
    }
}
