pipeline {
   agent any
   environment {
        ENV = "dev"
        NODE = "Build-server"
    }

   stages {
    stage('Build Image') {
        agent {
            node {
                label "Build-server"
                // customWorkspace "/Users/linhdv/Download/jenkins/multi-branch/devops-training-$ENV/"
                }
            }
        environment {
            TAG = sh(returnStdout: true, script: "git rev-parse -short=10 HEAD | tail -n +2").trim()
        }
         steps {
            // sh "security unlock-keychain -p LInkseven2011 /Users/linhdv/Library/Keychains/login.keychain; security set-keychain-settings -t 3600 -l /Users/linhdv/Library/Keychains/login.keychain;"
            sh "docker build nodejs/. -t devops-training-nodejs-$ENV:latest --build-arg BUILD_ENV=$ENV -f nodejs/Dockerfile"


            sh "cat docker.txt | docker login -u linhdv6513 --password-stdin"
            // tag docker image
            sh "docker tag devops-training-nodejs-$ENV:latest linhdv6513/nodejs-devops:$TAG"

            //push docker image to docker hub
            sh "docker push linhdv6513/nodejs-devops:$TAG"

	    // remove docker image to reduce space on build server	
            sh "docker rmi -f linhdv6513/nodejs-devops:$TAG"

           }
         
       }
	  stage ("Deploy ") {
	    agent {
        node {
            label "Target-Server"
                customWorkspace "/Users/linhdv/Download/jenkins/multi-branch/devops-training-$ENV/"
            }
        }
        environment {
            TAG = sh(returnStdout: true, script: "git rev-parse -short=10 HEAD | tail -n +2").trim()
        }
	steps {
            sh "security unlock-keychain -p LInkseven2011 /Users/linhdv/Library/Keychains/login.keychain; security set-keychain-settings -t 3600 -l /Users/linhdv/Library/Keychains/login.keychain;"
            sh "sed -i '' 's/{tag}/$TAG/g' /Users/linhdv/Download/jenkins/multi-branch/devops-training-$ENV/docker-compose.yaml"
            sh "docker compose up -d"
        }      
       }
   }
    
}
