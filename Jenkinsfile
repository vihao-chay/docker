pipeline {
    agent any

    environment {
    DEPLOY_PATH = '/home/azureuser/docker'
    }

    stages {

        stage('hello') {

            steps{
                withCredentials([sshUserPrivateKey(credentialsId: 'SSH_KEY', keyFileVariable: 'KEY', usernameVariable: 'SSH_USER')])
                {

                    sh """
                    ssh -o StrictHostKeyChecking=no -i ${KEY} ${SSH_USER}@20.78.59.201 '
                    sudo chown -R azureuser:azureuser /home/azureuser/docker
                    git config --global --add safe.directory /home/azureuser/docker
                    cd ${DEPLOY_PATH} && git pull
                    sudo docker build . -t img-html
                    sudo docker rm -f cons-html || true
                    sudo docker run -d -p 3001:80 --name cons-html img-html
                    '
                    """
                }   
                }
            }
        }
}