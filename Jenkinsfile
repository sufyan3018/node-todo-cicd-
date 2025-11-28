pipeline {
    agent any

    environment {
        DEV_SERVER = "13.204.42.72"        // Your dev server IP
        SSH_KEY = "/home/ubuntu/.ssh/id_rsa" // Path to private SSH key
    }

    stages {

        stage('Checkout Code') {
            steps {
                echo "Checking out code from GitHub..."
                git branch: 'master', url: 'https://github.com/sufyan3018/node-todo-cicd-.git'
            }
        }

        stage('Deploy to Dev Server') {
            steps {
                echo "Deploying to development server..."

                // Ensure private key has correct permissions
                sh "chmod 600 $SSH_KEY"

                // SSH into dev server and run deployment steps
                sh """
                    ssh -o StrictHostKeyChecking=no -i $SSH_KEY ubuntu@$DEV_SERVER '
                        cd /home/ubuntu/node-todo-cicd-

                        # Load NVM environment
                        export NVM_DIR="\$HOME/.nvm"
                        [ -s "\$NVM_DIR/nvm.sh" ] && \. "\$NVM_DIR/nvm.sh"
                        [ -s "\$NVM_DIR/bash_completion" ] && \. "\$NVM_DIR/bash_completion"

                        echo "Pulling latest code..."
                        git pull origin master || exit 1

                        echo "Installing dependencies..."
                        npm install || exit 1

                    

                        echo "Starting application..."
                        node app.js
                        
                        echo "Deployment completed successfully!"
                    '
                """
            }
        }
    }
}
