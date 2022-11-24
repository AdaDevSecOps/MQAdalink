def githubRepo = 'https://github.com/AdaDevSecOps/MQAdalink.git'
def githubBranch = 'main'

pipeline
{
    agent any
    environment
    {
        imagename = "mqadalink:5.20002.3.04"
        dockerImage = ''
    }
    stages{
        stage("Git Clone")
        {
            steps
            {
                echo "========Cloning Git========"
                git url: githubRepo,
                    branch: githubBranch
            }
            post
            {
                success
                {
                    echo "========Cloning Git successfully========"
                }
                failure
                {
                    echo "========Cloning Git failed========"
                }
            }
        }
        stage('Build Image')
        {
            steps
            {
                echo 'Building...'
                script
                {
                    dockerImage = docker.build imagename
                }
            }
        }
        stage('Delete container')
        {
            steps
            {
                echo 'Delete container...'
                script
                {
                    bat 'docker rm -f mqadalink-master'
                    bat 'docker rm -f mqadalink-sale-1'
                    bat 'docker rm -f mqadalink-sale-2'
                }
            }
        }
        stage('Run container')
        {
            steps
            {
                echo 'Run container...'
                script
                {
                    bat 'docker container create --name mqadalink-master mqadalink:5.20002.3.04'
                    bat 'docker container create --name mqadalink-sale-1 mqadalink:5.20002.3.04'
                    bat 'docker container create --name mqadalink-sale-2 mqadalink:5.20002.3.04'
                }
            }
        }
        stage('Copy file')
        {
            steps
            {
                echo 'Copy file...'
                script
                {
                    bat 'docker cp ./Appsetting/Master/. mqadalink-master:/app'
                    bat 'docker cp ./Appsetting/Sale1/. mqadalink-sale-1:/app'
                    bat 'docker cp ./Appsetting/Sale2/. mqadalink-sale-2:/app'
                }
            }
        }
        stage('Start container')
        {
            steps
            {
                echo 'Copy file...'
                script
                {
                    bat 'docker start mqadalink-master'
                    bat 'docker start mqadalink-sale-1'
                    bat 'docker start mqadalink-sale-2'
                }
            }
        }
    }
}
