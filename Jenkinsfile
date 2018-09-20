pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                echo 'Running build automation'
                sh './gradlew build --no-daemon'
                archiveArtifacts artifacts: 'dist/trainSchedule.zip'
            }
        }
        
        stage('Build docker image') {
            when {
                branch 'master' 
            }
            steps {
                script {
                    app = docker.build("train-schedule-docker")
                    app.inside {
                        sh 'echo $(curl localhost:8080)'
                    }
                }
            }
        }
        stage('push to docker') {
            when {
                branch 'master'
            }
                    steps {
                        script {
                            docker.withRegistry('https://registry.hub.docker.com','docker_hub_login')
                            {
                                app.push("${env.BUILD_NUMBER}")
                                app.push("latest")
                            }
                        }
                    }
                }
                   
               
    } 
}
     
