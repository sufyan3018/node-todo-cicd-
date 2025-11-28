pipeline {
    agent any

    stages {

        stage('Checkout Code') {
            steps {
                git branch: 'master', url: 'https://github.com/sufyan3018/node-todo-cicd-.git'
            }
        }

        stage('Deploy to Dev Server') {
            steps {
                sshCommand remote: [
                    host: '13.204.42.72',
                    user: 'ubuntu',
                    identityFile: '/home/ubuntu/.ssh/id_rsa'
                ], command: '''
                    cd /home/ubuntu/node-todo-cicd-

                    git pull origin master

                    npm install

                    pkill node || true

                    nohup node app.js > app.log 2>&1 &
                '''
            }
        }
    }
}
