pipeline{
    agent {label "dev"};
    
    stages{
        stage("code"){
            steps{
                git url: "https://github.com/sufiyannadeem/two-tier-flask-app.git" ,branch: "master"
            }
        }
        stage("build & test"){
            steps{
                sh "docker build . -t flaskapp"
            }
        }
       stage("push to dockerhub"){
           steps{
               withCredentials([usernamePassword(credentialsId:"Dockerhubcreds",
               passwordVariable:"DockerHubpass",usernameVariable:"DockerHubUser")]){
                   sh "docker login -u ${env.DockerHubUser} -p ${env.DockerHubpass}"
                   sh "docker tag flaskapp ${env.DockerHubUser}/flaskapp:latest"
                   sh "docker push ${env.DockerHubUser}/flaskapp:latest"
               }
           }
       }
        stage("deploy"){
            steps{
                sh "docker compose up -d --build flask-app "
            }
        }
    }
post{
    success{
        script{
            emailext from: "nadeemsufiyan149@gmail.com",
            to: "nadeemsufiyan149@gmail.com",
            body: "build success for test1 flask-app",
            subject: "build success"
        }
    }
 
    failure{
        script{
            emailext from: "nadeemsufiyan149@gmail.com",
            to: "nadeemsufiyan149@gmail.com",
            body: "build fail for test1 flask-app",
            subject: "build failed" 
        }
    }
}

    
}
