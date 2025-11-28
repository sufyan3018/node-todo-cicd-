pipeline {
    agent any

    environment {
        DEV_SERVER = "13.204.42.72"        // correct dev server
        SSH_KEY = "/home/ubuntu/.ssh/id_rsa"
    }

    stages {

        stage('Checkout Code') {
            steps {
                git branch: 'master', url: 'https://github.com/sufyan3018/node-todo-cicd-.git'
            }
        }

        stage('Deploy to Dev Server') {
            steps {

                // Fix permission of private key
                sh "chmod 600 $SSH_KEY"

                // SSH into real dev server
                sh """
                    ssh -o StrictHostKeyChecking=no -i $SSH_KEY ubuntu@$DEV_SERVER '
                        cd /home/ubuntu/node-todo-cicd-

                        git pull origin master

                        npm install

                        pkill node || true

                        nohup node app.js > app.log 2>&1 &
                    '
                """
            }
        }
    }
}
