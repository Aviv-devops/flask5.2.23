pipeline {
    agent any
    //options {
    //    skipStagesAfterUnstable()
    //}
    stages {
         stage('Clone repository') { 
            steps { 
                script{
                    checkout scm
                }
            }
        }

        stage('Build') { 
            steps { 
                script{
                    //app = docker.build("underwater")
                }
            }
        }
        
        stage('build & deploy to ECR') {
            steps {
                
                //Authenticate aws
                withEnv (["AWS_ACCESS_KEY_ID=${env.AWS_ACCESS_KEY_ID}", "AWS_SECRET_ACCESS_KEY=${env.AWS_SECRET_ACCESS_KEY}", "AWS_DEFAULT_REGION=${env.AWS_DEFAULT_REGION}"]){
                    
                    //login to docker with aws user and the password will be taken from the variable above.
                    sh 'docker login -u AWS -p $(aws ecr get-login-password --region us-east-1) 808447716657.dkr.ecr.us-east-1.amazonaws.com'
                    sh 'docker build -t flask_image .'
                    sh 'docker tag flask_image:latest 808447716657.dkr.ecr.us-east-1.amazonaws.com/flask_image:""$BUILD_ID""'
                    sh 'docker push 808447716657.dkr.ecr.us-east-1.amazonaws.com/flask_image:""$BUILD_ID""'
                }
            }
        }
    }
}

