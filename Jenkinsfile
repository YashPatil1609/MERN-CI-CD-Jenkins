pipeline{
    agent any
    stages{
        stage("Increment application versions"){
            steps{
                script{
                    echo "Incrementing application versions..."
                    dir('./mern/frontend') {
                    sh "npm version patch --no-git-tag-version"
                    env.FRONTEND_VERSION = sh(script: "jq -r '.version' package.json", returnStdout: true).trim()
                    echo "Frontend Version: ${env.FRONTEND_VERSION}"

                }
                    dir('./mern/backend') {
                    sh "npm version patch --no-git-tag-version"
                    env.BACKEND_VERSION = sh(script: "jq -r '.version' package.json", returnStdout: true).trim()
                    echo "Frontend Version: ${env.BACKEND_VERSION}"

                }
                }
                echo "Frontend Version: ${env.FRONTEND_VERSION}"
                echo "Backend Version: ${env.BACKEND_VERSION}"
            }
        }
    }
}