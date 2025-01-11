pipeline{
    agent any
    environment{
        DOCKER_HUB_USER = "yashpatil16"
    }
    stages{
        stage("Increment application versions"){
            steps{
                script{
                    echo "Incrementing application versions..."

                    dir('./mern/frontend') {
                        sh "npm version patch --no-git-tag-version"
                        def version = sh(script: "jq -r '.version' package.json", returnStdout: true).trim()
                        env.FRONTEND_VERSION = "$version-$BUILD_NUMBER"
                        echo "Frontend Version: ${env.FRONTEND_VERSION}"
                    
                    }
                    dir('./mern/backend') {
                    sh "npm version patch --no-git-tag-version"
                    def version = sh(script: "jq -r '.version' package.json", returnStdout: true).trim()
                    env.BACKEND_VERSION = "$version-$BUILD_NUMBER"
                    echo "Backend Version: ${env.BACKEND_VERSION}"

                }
            }
        }
        }
        stage("Building Docker images and pushing them to dockerhub"){
            steps{
                script{
                    echo "Building Docker images and pushing them to dockerhub..."
                    withCredentials([usernamePassword(credentialsId: 'dockerhub-credentials', usernameVariable: 'env.DOCKER_HUB_USER', passwordVariable: 'PASSWORD')]){
                        dir('./mern/frontend'){
                            sh "docker build -t ${DOCKER_HUB_USER}/mern-frontend:${env.FRONTEND_VERSION} ."
                            sh "echo $PASSWORD | docker login -u ${DOCKER_HUB_USER} --password-stdin"
                            sh "docker push ${DOCKER_HUB_USER}/mern-frontend:${env.FRONTEND_VERSION}"
                        }
                        dir('./mern/backend'){
                            sh "docker build -t ${DOCKER_HUB_USER}/mern-backend:${env.BACKEND_VERSION} ."
                            sh "echo $PASSWORD | docker login -u ${DOCKER_HUB_USER} --password-stdin"
                            sh "docker push ${DOCKER_HUB_USER}/mern-backend:${env.BACKEND_VERSION}"
                        }
                    }
                }
            }
        } 
        stage("Commiting version update"){
            steps{
                script{
                    withCredentials([usernamePassword(credentialsId: 'github-PAT', usernameVariable: 'USER', passwordVariable: 'PASS')]){
                        sh 'git config --global user.email "jenkins@example.com"'
                        sh 'git config --global user.name "jenkins"'

                        sh 'git remote set-url origin https://${USER}:${PASS}@github.com/YashPatil1609/MERN-CI-CD-Workflow.git'

                
                        sh 'git add .'
                        sh 'git commit -m "Incremented application versions"'

                
                        sh 'git push origin HEAD:main'
                    }
                }
            }
        }
        stage("Deploying to the Server"){
            steps{
                script{
                    sshAgent(['ec2-server-key']){
                        // Copy the docker-compose file to the EC2 instance
                        sh '''
                            scp -o StrictHostKeyChecking=no docker-compose.yml ubuntu@3.107.196.93:/home/ubuntu/docker-compose.yml
                        '''

                // Execute docker-compose commands on the EC2 instance
                        sh '''
                            ssh -o StrictHostKeyChecking=no ubuntu@3.107.196.93 << EOF
                            export FRONTEND_IMAGE=${DOCKER_HUB_USER}/mern-frontend:${env.FRONTEND_VERSION}
                            export BACKEND_IMAGE=${DOCKER_HUB_USER}/mern-backend:${env.BACKEND_VERSION}
                            cd /home/ubuntu
                            docker-compose up -d
                            EOF
                        '''
                    }
                }
            }
        }       
    }
}
