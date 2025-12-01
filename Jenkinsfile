pipeline {
    agent any

    environment {
        DEV_SERVER = "3.111.40.206"
    }

    stages {
stage('Checkout Code') {
    steps {
        checkout scm
    }
}
        stage('Deploy to Dev Server') {
            steps {
                echo "Deploying master to development server..."

                sshagent(credentials: ['dev-server-ssh']) {
                    sh """
                        ssh -o StrictHostKeyChecking=no ubuntu@$DEV_SERVER '
                            cd /home/ubuntu/node-todo-cicd-

                            export NVM_DIR="\$HOME/.nvm"
                            [ -s "\$NVM_DIR/nvm.sh" ] && . "\$NVM_DIR/nvm.sh"
                            [ -s "\$NVM_DIR/bash_completion" ] && . "\$NVM_DIR/bash_completion"

                            echo "Pulling latest code..."
                            git fetch origin development
                            git reset --hard origin/development

                            echo "Installing dependencies..."
                            npm install || exit 1

                            echo "Stopping existing Node process..."
                            pkill node || true

                            echo "Starting application..."
                            nohup node app.js > app.log 2>&1 &

                            echo "Deployment Successful!"
                        '
                    """
                }
            }
        }
    }
}
