pipeline {
    agent any

    environment {
        DEV_SERVER = "3.111.40.206"         // Your dev server IP
        SSH_KEY = "/home/ubuntu/.ssh/id_rsa"  // Path to private SSH key on Jenkins
    }

    stages {

        stage('Checkout Code') {
            steps {
                echo "Checking out branch: ${env.BRANCH_NAME}"

                checkout([
                    $class: 'GitSCM',
                    branches: [[name: "*/${env.BRANCH_NAME}"]],
                    userRemoteConfigs: [[url: 'https://github.com/sufyan3018/node-todo-cicd-.git']]
                ])
            }
        }

        stage('Deploy to Dev Server') {
            when {
                expression {
                    // 🚀 Only deploy for master or development
                    env.BRANCH_NAME == "master" || env.BRANCH_NAME == "development"
                }
            }
            steps {
                echo "Deploying ${env.BRANCH_NAME} to development server..."

                sh "chmod 600 $SSH_KEY"

                sh """
                    ssh -o StrictHostKeyChecking=no -i $SSH_KEY ubuntu@$DEV_SERVER '
                        cd /home/ubuntu/node-todo-cicd- || exit 1

                        # Load NVM properly
                        export NVM_DIR="\$HOME/.nvm"
                        [ -s "\$NVM_DIR/nvm.sh" ] && . "\$NVM_DIR/nvm.sh"
                        [ -s "\$NVM_DIR/bash_completion" ] && . "\$NVM_DIR/bash_completion"

                        echo "Pulling latest code for branch: ${env.BRANCH_NAME}"
                        git fetch --all
                        git reset --hard origin/${env.BRANCH_NAME}

                        echo "Installing dependencies..."
                        npm install || exit 1

                        echo "Stopping previous Node process..."
                        pkill node || true

                        echo "Starting application..."
                        nohup node app.js > app.log 2>&1 &

                        echo "Deployment successful for branch: ${env.BRANCH_NAME}"
                    '
                """
            }
        }
    }
}
