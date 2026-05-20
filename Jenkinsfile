pipeline {
    agent any

    stages {

        stage('Build Frontend') {
            steps {
                dir('frontend') {
                    sh '''
                    npm ci
                    npm run build
                    '''
                }
            }
        }

        stage('Deploy Frontend') {
            steps {
                sh '''
                scp -r frontend/build/* ec2-user@13.220.42.209:/usr/share/nginx/html/
                '''
            }
        }

        stage('Deploy Backend') {
            steps {
                sh '''
                ssh ec2-user@3.94.153.78 "

                    if [ ! -d 3tier-app ]; then
                        git clone https://github.com/chethanteja-kumar/3tier-app.git
                    fi

                    cd 3tier-app

                    git pull

                    cd backend

                    npm ci

                    pm2 restart server || pm2 start server.js --name server
                "
                '''
            }
        }
    }
}
